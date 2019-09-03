# Direct Access

* SQLPS module
* SQL Server Management Modules (SMO)
* .NET (System.Data.SQL / System.Data.SQLClient)

# Modules

PowerUpSQL - Toolkit for Attacking SQL Server: https://github.com/NetSPI/PowerUpSQL

# Discovery

* PowerUpSQL: `Get-SQLInstanceScanUDP -ComputerName 192.168.1.2 -verbose`
* .NET (UDP Broadcast): `[System.Data.Sql.SqlDataSourceEnumeration]::Instance.GetDataSources()`


# Local Enumeration

```
Import-Module -Name SQLPS
Get-ChildItem SQLSERVER:\SQL\<machinename>
```

```
Get-Service -Name MSSQL*
sqlinstances = Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Microsoft SQL Server' foreach ($SQLInstance in $SQLInstances) { foreach ($s in $SQLInstance.InstalledInstances) { [PSCustomObject]@{ PSComputerName = $SQLInstance.PSComputerName InstanceName = $s}}}
```

```
Get-SQLInstanceLocal
```

> http://www.powershellmagazine.com/2014/07/21/using-powershell-to-discover-information-about-your-microsoft-sql-servers/

# Domain Enumeration

* Search AD user attribute: `servicePrincipalName=MSSQL*`

```
Import-Module -Name PowerUpSQL
Get-SQLInstanceDomain -verbose
```

# Queries

| Information Obtained | Query |
| -------------------- | ----- |
| Version | SELECT @@version |
| Current User | SELECT SUSER_SNAME() <br/> SELECT SYSTEM_USER |
| Current Role | SELECT IS_SRVROLEMEMBER('sysadmin') <br/> SELECT user |
| Current Database | SELECT db_name() |
| List All Databases | SELECT name FROM master..sysdatabases |
| List All Logins | SELECT * FROM sys.server_principals WHERE type_desc != 'SERVER_ROLE' |
| List All Users for Database | SELECT * FROM sys.database_principals WHERE type_desc != 'DATABASE_ROLE' |
| List All Sysadmins | SELECT name,type_desc,is_disabled FROM sys.server_principals WHERE IS_SRVROLEMEMBER ('sysadmin',name) = 1 |
| List All Roles | SELECT DP1.name AS DatabaseRoleName,<br/> isnull (DP2.name, 'No members') AS DatabaseUserName<br/> FROM sys.database_role_members AS DRM<br/> RIGHT OUTER JOIN sys.database_principals AS DP1<br/> ON DRM.role_principal_id = DP1.principal_id<br/> LEFT OUTER JOIN sys.database_principals AS DP2<br/> ON DRM.member_principal_id = DP2.principal_id<br/> WHERE DP1.type = 'R'<br/> ORDER BY DP1.name; |
| Effective Permissions for Server | SELECT * FROM fn_my_permissions(NULL, 'SERVER'); |
| Effective Permissions for Database  | SELECT * FROM fn_my_permissions(NULL, 'DATABASE');  |
| Active User Tokens | SELECT * FROM sys.user_token |
| Active Login Tokens | SELECT * FROM sys.login_token |
| Impersonatable Accounts | SELECT distinct b.name<br/> FROM sys.server_permissions a<br/> INNER JOIN sys.server_principals b<br/> ON a.grantor_principal_id = b.principal_id<br/> WHERE a.permission_name = 'IMPERSONATE' |
| Find Trustworthy Databases | SELECT name as database_name <br/>, SUSER_NAME(owner_sid) AS database_owner <br/>, is_trustworthy_on AS TRUSTWORTHY <br/>from sys.databases |
> https://docs.microsoft.com/en-us/sql/relational-databases/system-catalog-views/sys-database-role-members-transact-sql


# Information Gathering

Looking for interesting databases

```
Get-SQLDatabaseThreaded -Threads 10 -Username sa -Password pw -Instance instance -verbose | select -ExpandProperty DatabaseName
```

```
Get-SQLDatabaseThreaded -Threads 10 -Username sa -Password pw -Instance instance | Where-Object {$_.is_encrypted -eq “True"}
```

```
Get-SQLColumnSampleDataThreaded -Threads 10 -Keywords "password, credit" -SampleSize 5 -ValidateCC -NoDefaults -Username sa -Password pw -Instance instance -Verbose
```
