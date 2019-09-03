# Display information

```
binwalk -t -vvv example-firmware
```

# Extract

```
binwalk -e -t -vvv example-firmware
```

# Entropy Analysis (identity compression / encryption)

```
binwalk -E example-firmware
```

> http://www.devttys0.com/2013/06/differentiate-encryption-from-compression-using-math/

# Repacking Firmware

> https://github.com/rampageX/firmware-mod-kit/wiki

```
./extract-firmware.sh example-firmware.bin
./build-formware.sh
```
