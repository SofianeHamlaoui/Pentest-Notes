# Widows

## Images
- Mount: https://accessdata.com/product-download/ftk-imager-version-4.2.0
- Autopsy: http://www.sleuthkit.org/autopsy/
  - Show attribute ID of NTFS file systems (fls)
- The Sleuth Kit: http://www.sleuthkit.org/sleuthkit/
- NTFS Log Tracker: https://sites.google.com/site/forensicnote/ntfs-log-tracker
- SIFT
  - Beginning of partition: mmls <image>
  - Assemble image in raw: ewfmount <image> /mnt/disk1
  - obtain shadow file information: vshadowinfo -o <val> /mnt/disk1/ewf1
    - val = partition start value from mmls * 512
  - Mount shadow: vshadowmount -o <val>  /mnt/disk1/ewf1 /mnt/vss
    - mount -o ro,loop /mnt/vss/vss1 /mnt/disk2
- Volume serial number
  - Position 0x48 of the BPB (Bios Parameter Block), which is part of the boot sector.
  - $ boot - mount with FTK Imager
  - Or use "vol" command
  - https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc781134(v=ws.10)
  - https://www.digital-detective.net/documents/Volume%20Serial%20Numbers.pdf

## Registry
- http://www.regxplor.com/
- https://ericzimmerman.github.io/#!index.
- RegRipper - Etracting/parsing information (keys, values, data) from the Registry and presenting it for analysis: https://github.com/keydet89/RegRipper2.8
- RECmd - Command line access to the Registry: https://github.com/EricZimmerman/RECmd

# Respone

- CIRTKit - Tools For The Computer Incident Response Team: https://github.com/opensourcesec/CIRTKit
