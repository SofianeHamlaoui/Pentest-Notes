# Distribution
```
cat /etc/issue
cat /etc/*-release
  cat /etc/lsb-release      # Debian based
  cat /etc/redhat-release   # Redhat based
```

# Kernel
```
cat /proc/version
uname -a
uname -mrs
rpm -q kernel
dmesg | grep Linux
ls /boot | grep vmlinuz-
```

# Environment
```
cat /etc/profile
cat /etc/bashrc
cat ~/.bash_profile
cat ~/.bashrc
cat ~/.bash_logout
env
set
```

# Printers
```
lpstat -a
```

# Sys calls
```
/usr/include /i386-linux-gnu/asm/unistd_32.h
```

# Kernel tuning
Temporary:
```
sysctl
```

Permanent:
```
/etc/sysctl.conf
```

View configuration:
```
sysctl -a |less
```

View  configuration files for the installed modprobe modules:
```
ls -l /etc/modprobe.d/
ls -R /lib/modules/$( uname -r )/kernel
```

# Kernel Modules
Insert module:
```
insmod
```

Remove module:
```
modprobe -r
rmmod
```

List modules:
```
modprobe -l
lsmod
```

# Installed Applications / Versions
```
ls -alh /usr/bin/
ls -alh /sbin/
dpkg -l
rpm -qa
ls -alh /var/cache/apt/archivesO
ls -alh /var/cache/yum/
```

# Scheduled Jobs
```
crontab -l
ls -alh /var/spool/cron
ls -al /etc/ | grep cron
ls -al /etc/cron*
cat /etc/cron*
cat /etc/at.allow
cat /etc/at.deny
cat /etc/cron.allow
cat /etc/cron.deny
cat /etc/crontab
cat /etc/anacrontab
cat /var/spool/cron/crontabs/root
```
