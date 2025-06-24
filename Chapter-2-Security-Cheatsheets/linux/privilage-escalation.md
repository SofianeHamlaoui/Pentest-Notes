# Tools

- BeRoot: https://github.com/AlessandroZ/BeRoot/tree/master/Linux

# File Permissions

* Check file permissions of /etc/passwd and /etc/shadow

Find writable files
```
find -type f -maxdepth 1 -writable
```

Generate password hash (md5):
```
openssl passwd -1
```
```
echo 'joske' | openssl passwd -1 -stdin
```

Generate password hash (sha256):
```
python -c "import crypt; print crypt.crypt('joske')"
```

# SUID / SGID Binaries

Find SUID
```
find "$DIRECTORY" -perm /4000
```

Find GUID
```
find "$DIRECTORY" -perm /2000
```

Find SUID / SGID
```
find "$DIRECTORY" -perm /6000
```

Find and ls SUID / SGID
```
find "$DIRECTORY" -perm /6000 -exec ls -la {} \;
```

# Searching world writable files
```
find / -perm -w ~ -type l -ls 2?/dev/null
```

# Plain text username / password
```
grep -i user [filename]
grep -i pass [filename]
grep -C 5 "password" [filename]
find . -name "*.php" -print0 | xargs -0 grep -i -n "var $password"   # Joomla
```

# Commands with sudo
```
sudo -l
```

# New file Permissions
```
umask
```

# Important Payloads

- Mempodipper compiled (Ubuntu 11 -> gimmeroot.c)

# Exploits

- Ubuntu (<= 18.10) - Dirty Sock: https://shenaniganslabs.io/2019/02/13/Dirty-Sock.html
  - https://github.com/initstring/dirty_sock/
- Ubuntu 14.04 and 16.04: (CVE-2017-1000112) https://cxsecurity.com/issue/WLB-2018010018
- Linux PAM 1.1.0 (Ubuntu 9.10/10.04) - MOTD File Tampering Privilege Escalation (2)
    - https://www.exploit-db.com/exploits/14339
    - HTB: Beep
## overlayfs

- Linux Kernel 3.13.0 < 3.19 (Ubuntu 12.04/14.04/14.10/15.04) - 'overlayfs' Local Privilege Escalation: https://www.exploit-db.com/exploits/37292
- Linux Kernel 4.3.3 (Ubuntu 14.04/15.10) - 'overlayfs' Local Privilege Escalation (1): https://www.exploit-db.com/exploits/39166
- Linux Kernel 4.3.3 - 'overlayfs' Local Privilege Escalation (2): https://www.exploit-db.com/exploits/39230

# Tools

- unix-privesc-check: https://github.com/pentestmonkey/unix-privesc-check
- Linux: linuxprivchecker.py - http://www.securitysift.com/download/linuxprivchecker.py
- Linux: LinEnum - https://github.com/rebootuser/LinEnum

# References

- Linux Local Privilege Escalation via SUID /proc/pid/mem Write - https://git.zx2c4.com/CVE-2012-0056/about/
