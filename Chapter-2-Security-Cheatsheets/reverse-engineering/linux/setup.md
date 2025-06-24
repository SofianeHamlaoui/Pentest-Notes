Temporarily disable ASLR
```
$ sudo sysctl -w kernel.randomize_va_space=0
```

Allow ptrace processes
```
$ sudo sysctl -w kernel.yama.ptrace_scope=0
```

Installing 32bit Libraries
```
dpkg --add-architecture i386
apt-get install libc6:i386
```
