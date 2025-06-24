# Update Download locations
```
C:\Windows\SoftwareDistribution\Download  
```
# Collections

- Living Off The Land Binaries and Scripts: https://lolbas-project.github.io/ li, https://github.com/LOLBAS-Project/LOLBAS

# wbadmin / ntbackup

> https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/wbadmin

Perform backups and restores of operating systems, drive volumes, computer files, folders, and applications from a command-line interface.

Delete any recovery catalogs:
```
cmd.exe /c wbadmin.exe delete catalog -quiet
```

# BCDEdit

> https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/bcdedit-command-line-options

Tool for managing Boot Configuration Data (BCD). BCD files provide a store that is used to describe boot applications and boot application settings.

Usable to creating new stores, modifying existing stores, adding boot menu options, and so on.

Windows recovery console does not attempt to repair anything:
```
cmd.exe /c bcdedit.exe /set {default} bootstatuspolicy ignoreallfailures & bcdedit /set {default} recoveryenabled no
```

# wevtutil

> https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil

Enables you to retrieve information about event logs and publishers. You can also use this command to install and uninstall event manifests, to run queries, and to export, archive, and clear logs.

Clear System and Security logs:
```
cmd.exe /c wevtutil.exe cl System
cmd.exe /c wevtutil.exe cl Security
```

# DUMPBIN

> https://docs.microsoft.com/en-us/cpp/build/reference/dumpbin-reference?view=vs-2017

Displays information about Common Object File Format (COFF) binary files. You can use DUMPBIN to examine COFF object files, standard libraries of COFF objects, executable files, and dynamic-link libraries (DLLs).

# HTA

Application where source code consists of HTML, Dynamic HTML, and one or more scripting languages supported by Internet Explorer, such as VBScript or JScript. An HTA executes without the constraints of the internet browser security model; it executes as a "fully trusted" application.

# Prefetch Files Created

- AT.EXE (scheduled jobs/tasks)
- SCHTASKS.EXE (scheduled jobs/tasks)
- CMD.EXE (Obviously common, but I included it anyway. Especially if the prefetch hash doesn't match the legitimate ones. )
- NET.EXE (net view, etc.)
- NET1.EXE (net use)
- NETSTAT.EXE (netstat -ano)
- REG.EXE (reg query and reg add)
- SC.EXE (interact with services)
- SYSTEMINFO.EXE (system profiling)
- TASKKILL.EXE (kill running processes)
- TASKLIST.EXE (tasklist /v)
- POWERSHELL.EXE (interact with powershell)
- NBTSTAT.EXE (profile)
- XCOPY.EXE (copy files around)
- NSLOOKUP.EXE (profile)
- QUSER.EXE (profile)
- RAR.EXE (Exfil or Tool dropping) * And other archive utilities (Ex. 7zip)
- PING.EXE (check connectivity)
- FTP.EXE (download/upload)
- Various Sysinternal tools (Psexec, sdelete, etc.)
- BITSADMIN.EXE (download/upload)
- ROUTE.EXE (adding persistent routes)
- REGSVR32.EXE (services)
- MAKECAB.EXE (compression before exfil)

> Originally form: http://www.sysforensics.org/2014/01/lateral-movement/. Link is no longer working

# Runonce.exe, msdt.exe, Openwith.exe

> https://medium.com/@mattharr0ey/lolbas-blowing-in-the-binaries-path-c480176cc636

# sethc.exe (Sticky keys)

By replacing the “Sticky Keys” binary, C:\Windows\System32\sethc.exe, with the Windows Command Processor cmd.exe, the attackers then accessed a privileged Windows console session without authenticating to the system. “Sticky Keys” is an accessibility feature that allows users to activate Windows modifier keys without pressing more than one key at a time. Pressing the shift key five times activates “Sticky Keys” and executes sethc.exe, which, when replaced with cmd.exe, opens a System-level command shell. From this shell, the attackers can execute arbitrary Windows commands, including adding or modifying accounts on the system, even from the logon screen (pre-authentication).
