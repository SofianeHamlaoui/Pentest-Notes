# Port Related Information:
```
/usr/share/nmap/nmap-servies
```

# ICMP (Ping) Sweep:
```
nmap -sn <pi-rage>
nmap -sn <ip-range> -oG filename (grepable format)
```

# Sweep TCP/UDP ports
```
nmap -p 80 <ip-range> -oG filename
```

# Sweep full network for top ports
```
nmap -sT -A --top-ports=20 <ip-range> -G filename
```

# Script engine:
```
ls -l /usr/share/nmap/scripts | grep smb
```

# Important flags
`-O` for OS Fingerprinting
`-sV` for banner grabbing
