```
Nmap 7.70 ( https://nmap.org ) at 2019-04-21 06:32 EDT
Nmap scan report for 10.10.10.6 (10.10.10.6)
Host is up (0.20s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 5.1p1 Debian 6ubuntu2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 3e:c8:1b:15:21:15:50:ec:6e:63:bc:c5:6b:80:7b:38 (DSA)
|_  2048 aa:1f:79:21:b8:42:f4:8a:38:bd:b8:05:ef:1a:07:4d (RSA)
80/tcp open  http    Apache httpd 2.2.12 ((Ubuntu))
|_http-server-header: Apache/2.2.12 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

gobuster -u http://10.10.10.6 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt

```
2019/04/21 07:11:49 Starting gobuster
=====================================================
/index (Status: 200)
/test (Status: 200)
/torrent (Status: 301)
```

Use file upload (thumbnail) to upload a PHP shell / command executor.

# Privilege Escalation
ls -lAR /home/george reveals an uncommon file (motd.legal-displayed) in the .cache
https://www.exploit-db.com/exploits/14339/
