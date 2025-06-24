# Types of port scans

## TCP Connect Scan
- Full 3 way handshake
```
nc -nvv -w 1 -z <ip> <port-range>
```

## SYN / Stealth Scan
- Send SYN
- SYN-ACK means open
- RST means closed

## UDP Scanning
- ICMP port unreachable means closed
- No response means open
```
nc -nv -u -z -w 1 <ip> <port-range>
```
