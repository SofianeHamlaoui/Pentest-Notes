# localrecon.cmd

>>> https://github.com/bitsadmin/miscellaneous/blob/master/localrecon.cmd
```
@echo off
REM
REM This software is provided under under the BSD 3-Clause License.
REM See the accompanying LICENSE file for more information.
REM
REM Utility to generate a summary of a Windows system
REM
REM Author:
REM  Arris Huijgen (@_bitsadmin)
REM
REM Website:
REM  https://github.com/bitsadmin/
REM
REM TODO:
REM * Check drives other than C:\ (if available)
REM * Add commandline parameter to perform a brief check or more extended
REM * Make sure that the output of this script is not again included in the output
REM
REM Some more (untested) commands:
REM pushd C:\ ^&^& findstr /I /N /S /P /R flag password secret confidential account payroll credit record * ^&^& popd
REM "dir C:\ /S /O-D /TW ^| findstr /C:"Directory of" /C:"%d%""!;!
REM "dir C:\ /a-r-d /s /b
REM To add: for /F "tokens=2* delims= " %%i in ('sc query ^| find /I "ce_name"') do @sc qc %i %j
REM

setlocal enabledelayedexpansion
set o="%tmp%\recon%random%.out"
set d=-12-2016
REM Define a separator character (!;!)
(set ;=^
%=Do not remove this line=%
)
set t[0][name]=Basic 1
set t[0]=COMPUTERNAME!;!USERDNSDOMAIN!;!USERNAME!;!HOMEDRIVE!;!HOMEPATH
set t[1][name]=Basic 2
set t[1]=systeminfo!;!wmic logicaldisk get caption,description,drivetype,providername,volumename ^| more
set t[2][name]=Users
set t[2]=query session!;!net user!;!net localgroup Administrators!;!net user Administrator!;!tree /F "C:\Users"!;!dir /s /b /A:H "C:\Users\"^|find /V "desktop.ini"!;!tree /F "C:\Documents and Settings"!;!dir /s /b /A:H "C:\Documents and Settings\"^|find /V "desktop.ini"
set t[3][name]=Network
set t[3]=ipconfig /all!;!route print!;!arp -a!;!netstat -ano!;!net session ^| find "\\"!;!net view!;!netsh advfirewall show allprofiles!;!netsh firewall show state!;!netsh firewall show config!;!nltest /dclist:
set t[4][name]=Processes, services and tasks
set t[4]=wmic PROCESS get Processid,Caption,Commandline ^| more!;!query process *!;!tasklist /SVC!;!net start!;!DRIVERQUERY /V!;!schtasks /query /fo LIST /v!;!at!;!dir /B C:\WINDOWS\Tasks!;!set ^| find /V "t["
set t[5][name]=Patch-level
set t[5]=wmic qfe get Caption,Description,HotFixID,InstalledOn ^| more
set t[6][name]=Base image configuration files
set t[6]=dir /s /b C:\sysprep.inf C:\sysprep.xml C:\unattend.txt C:\Unattended.xml C:\Services.xml C:\ScheduledTasks.xml C:\Printers.xml C:\Drives.xml C:\DataSources.xml
set t[7][name]=Configuration
set t[7]=reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated!;!reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated!;!reg query HKLM\System\CurrentControlSet\Services\NfsSvr\Parameters\SafeSetUidGidBits!;!reg query HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\SafeSetUidGidBits!;!gpresult /z
set t[8][name]=Credentials
set t[8]=cmdkey /list!;!dir /s /b /a:-D C:\*pass* C:\*cred* C:\*vnc* C:\*.config* C:\*.kdb*!;!findstr /si password *.xml *.ini *.txt *.ps1 *.cmd!;!reg query HKLM /f password /t REG_SZ /s!;!reg query HKCU /f password /t REG_SZ /s!;!reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /V Default*
set t[9][name]=Remote Desktop applications and VPN
set t[9]=dir /b /s C:\*vnc.ini!;!reg query HKCU\SOFTWARE\ORL\WinVNC3\Password!;!dir /s /b C:\*.rdp C:\*.pbk!;!reg query HKCU\Software\SimonTatham\PuTTY\Sessions!;!reg query "HKCU\Software\Microsoft\Terminal Server Client\Servers" /s
set t[9][name]=Configuration 2
set t[9]=wmic service get name,displayname,pathname,startmode^|findstr /i "Auto"^|findstr /i /v "C:\Windows\\"^|findstr /i /v """"
set t[10][name]=Files
set t[10]=dir /S /A /B C:\network-secret.txt C:\proof.txt C:\local.txt C:\*.vsd* C:\*flag* C:\*password* C:\*bank* C:\*competit* C:\*finance* C:\*invoice* C:\*risk* C:\*key* C:\*.pcf C:\*.crt C:\*.key C:\*.log ^| find /V /I "%SystemRoot%\winsxs\" ^| find /I /V "%SystemRoot%\Microsoft.NET\"
set t[11][name]=E-mail and documents
set t[11]=dir /S /A /B *.dbx *.wab *.doc *.docx *.xlsx
set t[12][name]=Pre-installed languages
set t[12]=where python!;!where perl!;!where ruby!;!dir /s /b C:\python.exe C:\pythonw.exe C:\perl.exe C:\ruby.exe C:\rubyw.exe C:\bash.exe C:\java.exe C:\javaw.exe C:\php.exe C:\powershell.exe
set last=12

:: CMD variables
echo --------------- [0/%last%] %t[0][name]% --------------- >%o% 2>&1
echo --------------- [0/%last%] %t[0][name]% ---------------
FOR /F %%i IN ("!t[0]!") DO (
	echo =^> %%~i
	echo %%i: !%%i! >>%o% 2>&1
)

:: Commands
FOR /L %%n IN (1,1,%last%) DO (
	echo. >>%o% 2>&1
	echo. >>%o% 2>&1
	echo --------------- [%%n/%last%] !t[%%n][name]! --------------- >>%o% 2>&1
	echo.
	echo --------------- [%%n/%last%] !t[%%n][name]! ---------------
	FOR /F "delims=;" %%i IN ("!t[%%n]!") DO (
		set c=%%~i
		echo =^> !c! >>%o% 2>&1
		echo =^> !c!
		REM If command does not contain a pipe, execute directly
		REM Otherwise execute using cmd /c
		IF !c!==!c:^|=! (
			!c! >>%o% 2>&1
		) ELSE (
			cmd /C "!c! >>%o% 2>&1"
		)
		echo. >>%o% 2>&1
		echo. >>%o% 2>&1
	)
)

REM Results
echo.
echo Done. Results in %o%.
```
