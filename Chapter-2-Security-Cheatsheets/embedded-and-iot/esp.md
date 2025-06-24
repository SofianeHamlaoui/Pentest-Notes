# Read Flash
```
esptool.py -p /dev/ttyUSB0 -b 460800 read_flash 0 0x200000 flash.bin
```

# Check Device config
```
espefuse.py --port /dev/ttyUSB0 summary
```
