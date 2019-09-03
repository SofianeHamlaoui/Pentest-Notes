> Originally from http://sysforensics.org/2014/01/know-your-windows-processes. Link is not longer working.

# Idle and System

- Created by ntoskrnl.exe via the process manager function, which creates and terminates processes and threads.
- No visible parent processes
- System has a static PID of 4
- System creates smss.exe
- There should only be one system process running

# SMSS - Session Manager

- First user mode process
- Parent process is System
- Base Priority of 11
- Username: NT AUTHORITY\SYSTEM
- Performs delayed file delete/rename changes
- Loads known dlls
- Runs from %systemroot%\System32\smss.exe
- Creates session 0 (OS services)
- Creates session 1 (User session)
- Creates csrss and winlogon then exits, which is why they have no parent process and they both have session ids of 1
- Runs within session 0
- Only one smss.exe process should be running at one time. The second smss.exe process exits, so you will only see the one running in session 0.
- There can be more sessions if more users are logged on to the system. 0 and 1 are for a single user logged onto the system.

# CSRSS.EXE - Client/Server Run

- Windows subsystem process.
- Base Priority of 13
- %SystemRoot%\system32\csrss.exe
- Username: NT AUTHORITY\SYSTEM
- Creates/Deletes processes and threads, Temp files, etc.
- In XP its used to draw text based console windows. Under Windows 7, the conhost process now does that functionality. For example, cmd.exe
- One csrss process per session
- Its name is often used by malware to hide on systems (CSSRS.EXE, CSRSSS.EXE, etc.)
- Runs within session 0

# WININIT.EXE - Windows Initialization Process

- Parent to services.exe (SCM), lsass.exe and lsm.exe
- Created by smss.exe, but since smss.exe exits there is no parent to WININIT.
- Base Priority of 13
- Username: NT AUTHORITY\SYSTEM
- %SystemRoot%\system32\wininit.exe
- Performs user-mode initialization tasks
- Creates %windir%\temp
- Runs within session 0

# SERVICES.EXE - Service Control Manager

- Child to WININIT.EXE
- Parent to services such at svchost.exe, dllhost.exe, taskhost.exe, spoolsv.exe, etc. Services are defined in SYSTEM\CurrentControlSet\Services
- %SystemRoot%\System32\wininit.exe
- Username: NT AUTHORITY\SYSTEM
- Base Priority of 9
- Loads a database of services into memory
- Runs within session 0
- There should only be one services.exe process running

# LSASS.EXE - Local Security Authority

- Child to WININIT.EXE
- Only one lsass.exe process
- %SystemRoot%\System32\lsass.exe
- Responsible for local security policy to include managing users allowed to login, password policies, writing to the security event log, etc.
- Often targeted by malware as a means to dump passwords. Also mimicked by malware to hide on a system (lass.exe, lssass.exe, lsasss.exe, etc.). These "fake" names will not be a children of wininit.exe.
- Base Priority of 9
- Username: NT AUTHORITY\SYSTEM
- Runs within session 0
- It should not have child processes

# SVCHOST.EXE - Service Hosting Process

- Multiple instances of svchost.exe can/do exist/run
- %SystemRoot%\System32\svchost.exe
- Username: Should only be one of three options: NT AUTHORITY\SYSTEM, LOCAL SERVICE, or NETWORK SERVICE
- Should always have a parent of services.exe
- Base Priority of 8
- Often mimicked (scvhost, svch0st, etc.) When they are mimicked they will not be running as children to services.exe.
- Command Line: svchost.exe -k <name>
- -k <name> values should exist within the Software\Microsoft\Windows NT\CurrentVersion\Svchost registry key
- Often times when malware uses the actual svchost.exe to load their malicious service they will not include -k command line parameters and be running under a username that does not match on of the three listed in bullet 3.
- They should all be running within session 0

# LSM.EXE - Load Session Manager Service

- Manages the state of terminal server sessions on the local machine. Sends the requests to smss.exe to start new sessions.
- Child to wininit.exe
- It should not have child processes
- Receives logon/off, shell start and termination, connect/disconnects from a session, and lock/unlock desktop
- I have not personally seen malware try and impersonate LSM.exe, but there is always a first so keep your eyes open.
- %systemroot%\System32\lsm.exe
- Base Priority of 8
- Username: NT AUTHORITY\SYSTEM
- Runs within session 0

# WINLOGON.EXE - Windows Logon Process

- No parent process
- Could have a child process of LogonUI if smartcard, etc. are used to authenticate
- LogonUI will terminate once the user enters their password. Once password is entered the verification is sent over to LSASS and it's verified via Active Directory or SAM (the registry hive SAM), which stores local users and group information.
- Base Priority of 13
- Runs within session one
- Handles interactive user logons/logoffs when SAS keystroke combination is entered (Ctrl+Alt+Delete)
- Loads Userinit within Software\Microsoft\Windows NT\CurrentVersion\Winlogon
- The userinit value in the registry should be: Userinit.exe, (note the comma). Malware will sometimes add additional values to this key, which will load malware upon successful logons.
- Userinit.exe exits once it runs so you wont see this process running when you look.
- Userinit initializes the user environment. This includes running GPOs and logon scripts.
- Will run Shell value located at Software\Microsoft\Windows NT\CurrentVersion\Winlogon within the registry. The value of shell should be Explorer.exe. Malware will also use this sometimes to execute malware by adding values.
- Since Userinit exists this is also why Explorer.exe doesn't have a parent process.

# Explorer.exe - AKA Windows Explorer

- No parent process since Userinit.exe exits
- The value "Explorer.exe" is stored in shell value within the registry. The registry location is here: Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell
- Base Priority of 8
- Username: The logged on user account.
- %Systemroot%\Explorer.exe
- This will contain multiple child processes.
- Some of you might know this better as, "Windows Explorer"
- This process is often targeted by malware. Malware will often times inject this process. One indication of this is if Explorer.exe is connecting out to the internet. There are other indicators, but that's another post. We are keeping it simple here.



# Simple checklist to review while looking for malicious/suspect process activity.

- Check the parent/child relationships of processes.
- Check which users names the processes are running under
- Check their command line parameters for those processes that use them.
- Check their digital signatures
- Check their base priorities
- Check the location they are being from
- Check their spellings
- Leverage memory analysis to detect hidden and/or injected process. Some malware can hide processes by unlinking them (among other ways). Memory analysis is a must these days.
- When you get comfortable with everything here, dig deeper and check what modules are typically loaded for each process.
- Check and see if processes that should not be connecting out to the internet are not
- Check process privileges
- If wscript.exe process is running check the command line of what it is running.
- Investigate processes running inside %temp%, root of %appdata%, %localappdata%, recycle bin, etc.
- If rundll32.exe is running check its command line as well.
- "Most" legitimate user applications like Adobe, Web browsers, etc. don't spawn child processes like cmd.exe. If you see this, they should be investigated.
- Core Windows processes shouldn't be communicating out to the internet. If you see communication from these processes, dig deeper. Look for suspicious URLs/IPs, check process strings, etc.

# References
- https://web.archive.org/web/20151113022252/http://blogs.sans.org/windows-security/files/Process_Hacker_SANS_Jason_Fossen.pdf
