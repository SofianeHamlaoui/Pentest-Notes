# APT sources
```
/etc/apt/sources.list
```

# Order of name resolution
```
/etc/nsswitch.conf
```

# DNS Hosts File
```
/etc/hosts
```

# DNS sever Information
```
/etc/resolv.conf
```

# Kernel module config
```
/etc/sysctl.conf
```

# Sys calls
```
/usr/include /i386-linux-gnu/asm/unistd_32.h
```

# OS version info
```
/etc/issue
/proc/version
```

# Cron
```
/etc/crontab
```

# Bootloader - GRUB2

## Main configuration file (replaces `menu.lst` in GRUB (v1))
```
/boot/grub/grub.cfg
```

## Directory contains the scripts that build the `grub.cfg`
```
/etc/grub.d
```
- `00_header` - Loads the settings from `/etc/default/grub`
- `05_debian_theme` - Defines the colors, background, etc.
- `10_linux` - Loads the menu entries
- `20_memtest86` - Loads the memory tester
- `30_os-prober` - Scans the hard drives for other operating systems
- `40_custom` - Template for manually adding other menu entries

## File contains the GRUB menu settings
```
/etc/default/grub
```

Run `update-grub` after modifying.

# Samba
```
/etc/samba/smb.conf
```

# Syslog
```
/etc/rsyslog.conf
```

Log mail events of all priorities to /var/log/mail.
```
mail.* /var/log/mail
```

\*.emerg \*
```
Log all events of the emergency priority (emerg) to all logged on users.
```
