# Setuid - Set User ID

The process's effective user ID gets set to that of the program file itself (rather than that of the user running it).

- `S` - just the setuid bit
- `s` - setuid bit and execute x
- Dir - No effect on DIRs

# Setgid - Set Group ID

The process's effective group ID gets set to that of the program file (rather than that of the user's primary group).

- Dir - any files created in that directory will have the same group as that directory

> http://www.tutonics.com/2012/12/linux-file-permissions-chmod-umask.html

# Permission Flags
```
r w x
4 2 1 = 7
```

# Changing Permissions
```
chmod g-w ChangeLog
chmod 744 ChangeLog
```
