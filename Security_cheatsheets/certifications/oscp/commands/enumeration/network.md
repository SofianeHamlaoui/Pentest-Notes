# Scan for hosts
```
nmap -sn $iprange -oG - | grep Up | cut -d' ' -f2 > network.txt
```

# Port scanning

## TCP Top 1000:
```
nmap -Pn -sC -sV -oA tcp -vv $ip
```

## All TCP Ports:
```
nmap -Pn -sC -sV -oA all -vv -p- $ip
```

## UDP Top 100:
```
nmap -Pn -sU --top-ports 100 -oA udp -vv $ip
```
