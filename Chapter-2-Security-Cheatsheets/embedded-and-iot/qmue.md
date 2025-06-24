# Run binaries inside a firmware

```
whereis qemu-mips-static
cp /etc/example/qemu-mips-static squashfs-root
```

```
# From squashfs-root
chroot ./ ./qemu-mips-static bin/ls
```
