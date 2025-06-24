- Bash cheatsheet: https://devhints.io/bash.html

# Startup Process

![](https://img.wonderhowto.com/img/44/77/63591930046359/0/linux-basics-for-aspiring-hacker-using-start-up-scripts.w1456.jpg)
> https://null-byte.wonderhowto.com/how-to/linux-basics-for-aspiring-hacker-using-start-up-scripts-0168875/

## Run levels
```
0 - halt the system
1 - single user mode (minimal services)
2 - multi-user modes
3 - multi-user mode
4 - multi-user mode
5 - multi-user mode
6 - reboot the system
```

## Init.d Process

- Has process ID: 1
- `/etc/init.d` scripts with 755 permission
- init process then hands over the boot-up processes to `rc.d` daemon

## rc.local

```
/etc/init.d/rc.local
```
Script to start necessary processes in the background when the system boots up

# Managing Hard Disks
`hda` for hard disks.
`sda` for newer SATA disks (SCSI).

Partitions within `sda` are `sda1`, `sda2`, ...

- Basic disk Information: `df -h`
- Partitions on disk: `fdsisk -l`
- Block device information: `lsblk`
- Editing and displaying partitions: `parted` / `cfdisk`
  - `(parted) print`
  - `(parted) select /dev/sdb`
- Change HDD parameters: `hdparm`
# General Text Manipulation Commands
```
cat
head
tail
nl
wc
grep
sed s/ex/ex1/ text.txt
sed s/ex/ex1/g text.txt
sed s/ex/ex1/3 text.txt
```

# StdIn StdOut and SrdErr
- 0 StdIn
- 1 StdOut
- 2 SrdErr

# inetd, xinetd

Inetd always runs in the background and it then decides when to start and stop other daemons.

# rlinetd
```
rlinetd.conf
/etc/rlinetd.d
```
- Disable unnecessary demons
- Configure IPs that can access a demon

# Archiving
- https://null-byte.wonderhowto.com/how-to/linux-basics-for-aspiring-hacker-archiving-compressing-files-0166153/
