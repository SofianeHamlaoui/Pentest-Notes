# Server Message Block (SMB) Versions

| SMB Version     | Windows version     |
| :-------------- | :------------------ |
| CIFS | Microsoft Windows NT 4.0 |
| SMB 1.0 | Windows 2000, Windows XP, Windows Server 2003 and Windows Server 2003 R2 |
| SMB 2.0 | Windows Vista & Windows Server 2008 |
| SMB 2.1 | Windows 7 and Windows Server 2008 R2 |
| SMB 3.0 | Windows 8 and Windows Server 2012 |
| SMB 3.0.2 | Windows 8.1 and Windows Server 2012 R2 |
| SMB 3.1.1 | Windows 10 and Windows Server 2016 |
|   |   |

# Ports

netbios-ns `137/tcp` # (NBT over IP) NETBIOS Name Service
netbios-ns `137/udp`

netbios-dgm `138/tcp` # (NBT over IP) NETBIOS Datagram Service
netbios-dgm `138/udp`

netbios-ssn `139/tcp` # (NBT over IP) NETBIOS session service
netbios-ssn `139/udp`

microsoft-ds `445/tcp` # (SMB over IP) If you are using Active Directory (used when SMB is used directly on TCP stack, without using NetBIOS)

# Configuration Files
```
smb.conf
lmhosts
```

# Scanning

## nbtstat
```
nbtstat <ip>
nbtscan -‐r 192.168.11.0/24
```
## nmblookup
```
nmblookup -A $ip
```

## enum4linux
Wrapper for `smbclient`, `rpcclient`, `net` and `nmblookup`:
```
enum4linux -‐a 192.168.11.227
enum4linux -U 192.168.11.227
```
> https://hackercool.com/2016/07/smb-enumeration-with-kali-linux-enum4linuxacccheck-smbmap/

## nmap
```
nmap --script smb-protocols -p445 (target/range)
```
```
nmap -p 139,446 <ip-range> --open
```
```
nmap ‐v ‐p 139, 445 -‐script=smb‐security‐mode 192.168.11.236
nmap ‐v ‐p 139, 445 -‐script=smb‐os-discovery 192.168.11.227
nmap ‐v ‐p 139, 445 -‐script=smb‐check-vulns --script-args=unsafe=1 192.168.11.227
```

## acccheck - Password Attacks
```
acccheck -v -t <ip> -u <user> -P <password_file>
```

## SMBMap - enumerate samba share drives across an entire domain
```
smbmap -u <user> -p <password> -d <workgroup> -H <ip>
smbmap -u <user> -p <password> -d <workgroup> -H <ip> -L  #test command execution
smbmap -u <user> -p <password> -d <workgroup> -H <ip> -r  #read drive
```

```
smbmap -H $ip
```

Recursively list dirs, and files:
```
smbmap -R $sharename -H $ip
```

Downloads a file in quiet mode:
```
smbmap -R $sharename -H $ip -A $fileyouwanttodownload -q
```


## mblookup

NetBIOS over TCP/IP client used to lookup NetBIOS names

# SMB Client
```
smbclient -L //server/share password options
```

# Enable / Disable / Status

> Detect, enable and disableyeha SMBv1, SMBv2, and SMBv3 in Windows and Windows Server: https://support.microsoft.com/en-gb/help/2696547/how-to-detect-enable-and-disable-smbv1-smbv2-and-smbv3-in-windows-and

## Windows Server 2012 R2 & 2016: PowerShell methods
### SMB v1
- Detect: `Get-WindowsFeature FS-SMB1`
- Disable: `Disable-WindowsOptionalFeature -Online -FeatureName smb1protocol`
- Enable: `Enable-WindowsOptionalFeature -Online -FeatureName smb1protocol`

### SMB v2/v3
- Detect: `Get-SmbServerConfiguration | Select EnableSMB2Protocol`
- Disable: `Set-SmbServerConfiguration -EnableSMB2Protocol $false`
- Enable: `Set-SmbServerConfiguration -EnableSMB2Protocol $true`

## Windows 8.1 and Windows 10: PowerShell method
### SMB v1 Protocol
- Detect:	`Get-WindowsOptionalFeature –Online –FeatureName SMB1Protocol`
- Disable:	`Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol`
- Enable:	`Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol`

### SMB v2/v3 Protocol
- Detect:	`Get-SmbServerConfiguration | Select EnableSMB2Protocol`
- Disable: `Set-SmbServerConfiguration –EnableSMB2Protocol $false`
- Enable:	`Set-SmbServerConfiguration –EnableSMB2Protocol $true`

## Windows 8 and Windows Server 2012
### SMB v1 on SMB Server
- Detect:	`Get-SmbServerConfiguration | Select EnableSMB1Protocol`
- Disable:	`Set-SmbServerConfiguration -EnableSMB1Protocol $false`
- Enable:	`Set-SmbServerConfiguration -EnableSMB1Protocol $true`

### SMB v2/v3 on SMB Server
- Detect:	`Get-SmbServerConfiguration | Select EnableSMB2Protocol`
- Disable:	`Set-SmbServerConfiguration -EnableSMB2Protocol $false`
- Enable:	`Set-SmbServerConfiguration -EnableSMB2Protocol $true`

##  Windows 7, Windows Server 2008 R2, Windows Vista, and Windows Server 2008
### SMB v1 on SMB Server
Default configuration = Enabled (No registry key is created), so no SMB1 value will be returned

- Detect: `Get-Item HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath}`
- Disable: `Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 0 –Force`
- Enable: `Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 1 –Force`

### SMB v2/v3 on SMB Server
- Detect: `Get-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath}``
- Disable: `Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 0 –Force`
- Enable: `Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 1 –Force`

## Disable SMB Client
### SMB v1 on SMB Client
- Detect:	`sc.exe qc lanmanworkstation`
- Disable:
```
sc.exe config lanmanworkstation depend= bowser/mrxsmb20/nsi
sc.exe config mrxsmb10 start= disabled
```
- Enable:
```
sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
sc.exe config mrxsmb10 start= auto
```

### SMB v2/v3 on SMB Client
- Detect:	`sc.exe qc lanmanworkstation`
- Disable:
```
sc.exe config lanmanworkstation depend= bowser/mrxsmb10/nsi
sc.exe config mrxsmb20 start= disabled
```
- Enable:
```
sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
sc.exe config mrxsmb20 start= auto
```

# Null Session Enumeration

Null Session Enumeration (enabled by default in SMB1)
```
net use \\192.168.1.1\ipc$ "" /u:""
net view \\ip_address
```

```
rpcclient -U "" ip (give empty password)
  > srvinfo
  > enumdomusers
  > getdompwinfo
```

# Tools

- nbtstat
- SuperScan
- Hyena
- Winfingerprint
- NetBIOS enumerator
- nbtscan
- enum4linux
- rpcclient

# Samba Configuration
Configuration file:
```
/etc/samba/smb.conf
```

Test & reload configuration
```
testparm -v
service smb restart
```

User creation:
```
smbpasswd -a <username>
```

# Samba Enumeration
```
#!/bin/sh
#Author: rewardone
#Description:
# Requires root or enough permissions to use tcpdump
# Will listen for the first 7 packets of a null login
# and grab the SMB Version
#Notes:
# Will sometimes not capture or will print multiple
# lines. May need to run a second time for success.
if [ -z $1 ]; then echo "Usage: ./smbver.sh RHOST {RPORT}" && exit; else rhost=$1; fi
if [ ! -z $2 ]; then rport=$2; else rport=139; fi
tcpdump -s0 -n -i tap0 src $rhost and port $rport -A -c 7 2>/dev/null | grep -i "samba\|s.a.m" | tr -d '.' | grep -oP 'UnixSamba.*[0-9a-z]' | tr -d '\n' & echo -n "$rhost: " &
echo "exit" | smbclient -L $rhost 1>/dev/null 2>/dev/null
echo "" && sleep .1
```

# References
- https://www.youtube.com/watch?v=jUc1J31DNdw&t=445s

# Vulnerabilities
-  Samba versions 3.0.20 through 3.0.25rc3
    - https://www.rapid7.com/db/modules/exploit/multi/samba/usermap_script
    - https://github.com/amriunix/cve-2007-2447
