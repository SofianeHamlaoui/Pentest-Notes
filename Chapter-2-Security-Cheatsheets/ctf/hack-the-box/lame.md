- IP: 10.10.10.3
- nmap -sC -sV -oA nmap 10.10.10.3

```
Nmap scan report for 10.10.10.3 (10.10.10.3)
Host is up (0.17s latency).
Not shown: 996 filtered ports
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to 10.10.14.9
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey:
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -2d22h57m52s, deviation: 0s, median: -2d22h57m52s
| smb-os-discovery:
|   OS: Unix (Samba 3.0.20-Debian)
|   NetBIOS computer name:
|   Workgroup: WORKGROUP\x00
|_  System time: 2019-04-18T01:16:39-04:00
|_smb2-time: Protocol negotiation failed (SMB2)

```

```
=========================================
Getting domain SID for 10.10.10.3    |
=========================================
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 359.
Domain Name: WORKGROUP
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

====================================
|    OS information on 10.10.10.3    |
====================================
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 458.
Use of uninitialized value $os_info in concatenation (.) or string at ./enum4linux.pl line 464.
[+] Got OS info for 10.10.10.3 from smbclient:
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 467.
[+] Got OS info for 10.10.10.3 from srvinfo:
  LAME           Wk Sv PrQ Unx NT SNT lame server (Samba 3.0.20-Debian)
  platform_id     :	500
  os version      :	4.9
  server type     :	0x9a03

===========================
|    Users on 10.10.10.3    |
===========================
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 866.
index: 0x1 RID: 0x3f2 acb: 0x00000011 Account: games	Name: games	Desc: (null)
index: 0x2 RID: 0x1f5 acb: 0x00000011 Account: nobody	Name: nobody	Desc: (null)
index: 0x3 RID: 0x4ba acb: 0x00000011 Account: bind	Name: (null)	Desc: (null)
index: 0x4 RID: 0x402 acb: 0x00000011 Account: proxy	Name: proxy	Desc: (null)
index: 0x5 RID: 0x4b4 acb: 0x00000011 Account: syslog	Name: (null)	Desc: (null)
index: 0x6 RID: 0xbba acb: 0x00000010 Account: user	Name: just a user,111,,	Desc: (null)
index: 0x7 RID: 0x42a acb: 0x00000011 Account: www-data	Name: www-data	Desc: (null)
index: 0x8 RID: 0x3e8 acb: 0x00000011 Account: root	Name: root	Desc: (null)
index: 0x9 RID: 0x3fa acb: 0x00000011 Account: news	Name: news	Desc: (null)
index: 0xa RID: 0x4c0 acb: 0x00000011 Account: postgres	Name: PostgreSQL administrator,,,	Desc: (null)
index: 0xb RID: 0x3ec acb: 0x00000011 Account: bin	Name: bin	Desc: (null)
index: 0xc RID: 0x3f8 acb: 0x00000011 Account: mail	Name: mail	Desc: (null)
index: 0xd RID: 0x4c6 acb: 0x00000011 Account: distccd	Name: (null)	Desc: (null)
index: 0xe RID: 0x4ca acb: 0x00000011 Account: proftpd	Name: (null)	Desc: (null)
index: 0xf RID: 0x4b2 acb: 0x00000011 Account: dhcp	Name: (null)	Desc: (null)
index: 0x10 RID: 0x3ea acb: 0x00000011 Account: daemon	Name: daemon	Desc: (null)
index: 0x11 RID: 0x4b8 acb: 0x00000011 Account: sshd	Name: (null)	Desc: (null)
index: 0x12 RID: 0x3f4 acb: 0x00000011 Account: man	Name: man	Desc: (null)
index: 0x13 RID: 0x3f6 acb: 0x00000011 Account: lp	Name: lp	Desc: (null)
index: 0x14 RID: 0x4c2 acb: 0x00000011 Account: mysql	Name: MySQL Server,,,	Desc: (null)
index: 0x15 RID: 0x43a acb: 0x00000011 Account: gnats	Name: Gnats Bug-Reporting System (admin)	Desc: (null)
index: 0x16 RID: 0x4b0 acb: 0x00000011 Account: libuuid	Name: (null)	Desc: (null)
index: 0x17 RID: 0x42c acb: 0x00000011 Account: backup	Name: backup	Desc: (null)
index: 0x18 RID: 0xbb8 acb: 0x00000010 Account: msfadmin	Name: msfadmin,,,	Desc: (null)
index: 0x19 RID: 0x4c8 acb: 0x00000011 Account: telnetd	Name: (null)	Desc: (null)
index: 0x1a RID: 0x3ee acb: 0x00000011 Account: sys	Name: sys	Desc: (null)
index: 0x1b RID: 0x4b6 acb: 0x00000011 Account: klog	Name: (null)	Desc: (null)
index: 0x1c RID: 0x4bc acb: 0x00000011 Account: postfix	Name: (null)	Desc: (null)
index: 0x1d RID: 0xbbc acb: 0x00000011 Account: service	Name: ,,,	Desc: (null)
index: 0x1e RID: 0x434 acb: 0x00000011 Account: list	Name: Mailing List Manager	Desc: (null)
index: 0x1f RID: 0x436 acb: 0x00000011 Account: irc	Name: ircd	Desc: (null)
index: 0x20 RID: 0x4be acb: 0x00000011 Account: ftp	Name: (null)	Desc: (null)
index: 0x21 RID: 0x4c4 acb: 0x00000011 Account: tomcat55	Name: (null)	Desc: (null)
index: 0x22 RID: 0x3f0 acb: 0x00000011 Account: sync	Name: sync	Desc: (null)
index: 0x23 RID: 0x3fc acb: 0x00000011 Account: uucp	Name: uucp	Desc: (null)

=======================================
|    Share Enumeration on 10.10.10.3    |
=======================================
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 640.

   Sharename       Type      Comment
   ---------       ----      -------
   print$          Disk      Printer Drivers
   tmp             Disk      oh noes!
   opt             Disk      
   IPC$            IPC       IPC Service (lame server (Samba 3.0.20-Debian))
   ADMIN$          IPC       IPC Service (lame server (Samba 3.0.20-Debian))
Reconnecting with SMB1 for workgroup listing.

   Server               Comment
   ---------            -------

   Workgroup            Master
   ---------            -------
   WORKGROUP            LAME

```

- smbmap -R -H 10.10.10.3
```
root@kali:~/HTB/lame# smbmap -R -H 10.10.10.3
[+] Finding open SMB ports....
[+] User SMB session establishd on 10.10.10.3...
[+] IP: 10.10.10.3:445	Name: 10.10.10.3                                        
	Disk                                                  	Permissions
	----                                                  	-----------
	print$                                            	NO ACCESS
	tmp                                               	READ, WRITE
	.\
	dr--r--r--                0 Thu Apr 18 03:12:06 2019	.
	dw--w--w--                0 Sun May 20 14:36:11 2012	..
	dr--r--r--                0 Wed Apr 17 08:10:09 2019	.ICE-unix
	dr--r--r--                0 Wed Apr 17 08:10:39 2019	.X11-unix
	-w--w--w--               11 Wed Apr 17 08:10:39 2019	.X0-lock
	-w--w--w--                0 Wed Apr 17 08:11:21 2019	5264.jsvc_up
	.\\.X11-unix\
	dr--r--r--                0 Wed Apr 17 08:10:39 2019	.
	dr--r--r--                0 Thu Apr 18 03:12:06 2019	..
	-r--r--r--                0 Wed Apr 17 08:10:39 2019	X0
	opt                                               	NO ACCESS
	IPC$                                              	NO ACCESS
	ADMIN$                                            	NO ACCESS

```

- Use SMB exploit:  https://github.com/amriunix/cve-2007-2447

- ROOT: 92caac3be140ef409e45721348a4e9df
