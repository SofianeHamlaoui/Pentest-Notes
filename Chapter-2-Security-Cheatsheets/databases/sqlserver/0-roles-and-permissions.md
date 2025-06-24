# Principals

Principals are entities that can request SQL Server resources.

## SQL Server-level principals

* SQL Server authentication Login
  * sa
    * Created when instance is installed
    * Default database is `master`
    * Member of `sysadmin` database role
  * public
    * Every login belongs to the this role
* Windows authentication login for a Windows user
* Windows authentication login for a Windows group
* Azure Active Directory authentication login for a AD user
* Azure Active Directory authentication login for a AD group
* Server Role

## Database-level principals

* Database User (There are 11 types of users. For more information, see CREATE USER.)
  * dbo
    * Created for each database
    * Has all permissions in the database
    * Owns `dbo` schema (`dbo` schema is the default schema for all users, and cannot be dropped)
  * guest
    *  Permissions granted are inherited by users who have access to the database, but who do not have a user account in the database.
    * Cannot be dropped
    * Can be disabled by revoking it's CONNECT permission (`REVOKE CONNECT FROM GUEST;`)
* Database Role
* Application Role

# Special Schemas

`INFORMATION_SCHEMA`
`sys`

# Server-Level Roles and Permissions

## Fixed Roles

| Role | Description |
| ---- | ----------- |
| sysadmin | Can perform any activity in the server. |
| serveradmin | Can change server-wide configuration options and shut down the server. |
| securityadmin | <ul><li>Manage logins and their properties.</li><li>Can GRANT, DENY, and REVOKE server-level permissions.</li><li>Can also GRANT, DENY, and REVOKE database-level permissions if they have access to a database.</li><li>Can reset passwords for SQL Server logins.</li><li>Should be treated as equivalent to the sysadmin role.</li></ul> |
| processadmin | Can end processes that are running in an instance of SQL Server. |
|setupadmin | Can add and remove linked servers by using Transact-SQL statements. (sysadmin membership is needed when using Management Studio.) |
| bulkadmin | Can run the BULK INSERT statement. |
| diskadmin | Used for managing disk files. |
| dbcreator | Can create, alter, drop, and restore any database.
| public | <ul><li>Every SQL Server login belongs to the public server role.</li><li>When a server principal has not been granted or denied specific permissions on a securable object, the user inherits the permissions granted to public on that object.</li><li>Only assign public permissions on any object when you want the object to be available to all users.</li><li>You cannot change membership in public.</li><li>Public is implemented differently than other roles, and permissions can be granted, denied, or revoked from the public fixed server roles.</li></ul>|

## Fixed Roles and Permissions

![Server level roles and permissions](https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/media/permissions-of-server-roles.png?view=sql-server-2017)
> https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/server-level-roles?view=sql-server-2017

## Working with Server-Level Roles

> https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/server-level-roles?view=sql-server-2017#working-with-server-level-roles

# Database Level Roles and Permissions

`CREATE LOGIN ... WITH PASSWORD = ...;`

## Fixed Roles

| Role | Description |
| ---- | ----------- |
| db_owner | <ul><li>Can perform all configuration and maintenance activities on the database.</li><li>Can drop the database in SQL Server.</li><li>(In SQL Database and SQL Data Warehouse, some maintenance activities require server-level permissions and cannot be performed by db_owners.)</li></ul> |
| db_securityadmin | <ul><li>Can modify role membership and manage permissions.</li><li>Adding principals to this role could enable unintended privilege escalation.</li></ul> |
| db_accessadmin | Can add or remove access to the database for Windows logins, Windows groups, and SQL Server logins.</li></ul> |
| db_backupoperator |	Can back up the database. |
| db_ddladmin | Can run any Data Definition Language (DDL) command. |
| db_datawriter | Can add, delete, or change data in all user tables. |
| db_datareader |	Can read all data from all user tables. |
| db_denydatawriter |	Cannot add, modify, or delete any data in the user tables within a database. |
| db_denydatareader |	Cannot read any data in the user tables within a database. |

## Fixed Roles and Permissions

![Database level roles and permissions](https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/media/permissions-of-database-roles.png?view=sql-server-2017)
> https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/media/permissions-of-database-roles.png?view=sql-server-2017

## Special Roles for SQL Database and SQL Data Warehouse

* Exist only in the virtual master database.
* Permissions are restricted to actions performed in master.


* Only database users in master can be added to these roles.
* Logins cannot be added to these roles, but users can be created based on logins and then those users can be added to the roles. Contained database users in master, can also be added to these roles.

| Role | Description |
| ---- | ----------- |
| dbmanager |	<ul><li>Can create and delete databases.</li><li>A member of the dbmanager role that creates a database, becomes the owner of that databasee which allows that user to connect to that database as the dbo user.</li><li>The dbo user has all database permissions in the database.</li><li>Members of the dbmanager role do not necessarily have permission to access databases that they do not own.</li> |
| loginmanager | Can create and delete logins in the virtual master database. |

## msdb Roles

| Role | Description |
| ---- | ----------- |
| db_ssisadmin</br>db_ssisoperator</br>db_ssisltduser | <ul><li>Can administer and use SSIS.</li><li>Instances of SQL Server that are upgraded from an earlier version might contain an older version of the role that was named using Data Transformation Services (DTS) instead of SSIS.</li><li><b>db_ssisadmin</b> may be able to elevate their privileges to sysadmin. [1]</li></ul> |
| dc_admin<br>dc_operator</br>dc_proxy |	Can administer and use the data collector. |
| PolicyAdministratorRole | <ul><li>Can perform all configuration and maintenance activities on Policy-Based Management policies and conditions.</li><li><b>dc_admin</b> may be able to elevate their privileges to sysadmin. [1]</li></ul>
| ServerGroupAdministratorRole</br>ServerGroupReaderRole | Can administer and use registered server groups. |
| dbm_monitor |	<ul><li>Created in the msdb database when the first database is registered in Database Mirroring Monitor.</li><li>Has no members until a system administrator assigns users to the role.</li></ul> |

[1] These roles can modify Integration Services packages and Integration Services packages can be executed by SQL Server using the sysadmin security context of SQL Server Agent. To guard against this elevation of privilege when running maintenance plans, data collection sets, and other Integration Services packages, configure SQL Server Agent jobs that run packages to use a proxy account with limited privileges or only add sysadmin members to the db_ssisadmin and dc_admin roles.

> https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/database-level-roles?view=sql-server-2017#msdb-roles

## R Services

| Role | Description |
| ---- | ----------- |
| rpkgs-users | Allows using any shared packages that were installed by members of the rpkgs-shared role. |
| rpkgs-private | <ul><li>Provides access to shared packages with the same permissions as the rpkgs-users role.</li><li>Members of this role can also install, remove and use privately scoped packages.</li></ul> |
| rpkgs-shared | <ul><li>Provides the same permissions as the rpkgs-private role.</li><li>Users who are members of this role can also install or remove shared packages.</li></ul> |

## Working with Database-Level Roles

> https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/database-level-roles?view=sql-server-2017#working-with-database-level-roles

# Application Roles

* Enable access to specific data to only those users who connect through a particular application.
* Enabled by using sp_setapprole
