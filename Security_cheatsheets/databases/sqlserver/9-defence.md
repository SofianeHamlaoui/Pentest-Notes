* Audit links, trusts, privileges and credentials.
* Service Accounts for databases should not be high privilege domain account.
* Known dangerous Stored Procedures are disabled.
* Use audit features to log interesting events.
* Monitor the logs (`Management Studio -> Management -> SQL Server Logs`)
* Error log @ `Program-Files\Microsoft SQL Server\MSSQL.1MSSQL\LOG\ERRORLOG`
* Logs are also written to Windows Application logs with `MSSQLSERVER` as source.
* Good password policy.
* Not using same username across databases.
* Logon failures are logged by default (source `MSSQLSERVER`).

| Event ID | Description |
| -------- |------------ |
| 18456 | Authentication failures |
| 5084 | Setting TRUSTWORTHY to on/off |
| 17135 | Launch of startup stored procedures |
| 33090 | Successful DLL loading |
| 17750 | Failed DLL loading |
| 15457 | Using sp_configure (command execution) |
