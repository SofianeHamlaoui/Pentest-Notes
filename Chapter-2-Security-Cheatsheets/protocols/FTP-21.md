# Scan for anonymous FTP
```
nmap ‐v ‐p 21 -­‐script=ftp‐anon.nse 192.168.11.200-254
```
# Anonymous login
```
ftp ip_address
Username: anonymous
Password: any@email.com (if prompted)
```

# Bruteforce

`Hydra`
`Medusa`
`Brutus`

# Config files
```
ftpusers
ftp.conf
proftpd.conf
```

# MITM

- pasvagg.pl: https://packetstormsecurity.com/0007-exploits/pasvagg.pl
