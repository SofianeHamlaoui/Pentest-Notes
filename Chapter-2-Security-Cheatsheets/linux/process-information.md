# Running processes
```
ps aux

  all processes (a)
  the user (u)
  processes not associated with a terminal (x)

ps -ef
ps -eF

top
```

Tree of processes (processes & threads):
```
pstree -aclp
```
# Process priority
- `-20` is highest priority
- `19` is lowest priority
```
nice -n -20 <command>
```

```
renice <nice-value> <pid>
```

# Memory map for a process
```
 cat /proc/1234/maps
```
```
gdb
info proc mappings
```
```
pmap -d 1234
```

# /proc
- `/proc/<id>/environ` environment variables
- `/proc/<id>/cmdline` command line args/command used to run the process
- `/proc/<id>/maps` memory map
- `/proc/<id>/fd` open file descriptors

# System and library calls
- `ltrace`
- `strace`

# Access control
- `access` - Check permissions for the UID and GID of the process (executable file owner / group)
  - Check is done using the calling process's real UID and GID, rather than the effective IDs as is done when actually attempting an operation (e.g., open(2)) on the file.

# Other
```
killall <name>
kill -9 <pid>
kill <pid>
```

```
fg
bg
[Ctrl+Z]
```
