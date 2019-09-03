# Open a PCAP
```
tcpdump -­r password_cracking_filtered.pcap
```

# Cut only IP addresses from the traffic
```
tcpdump -­r password_cracking_filtered.pcap  | awk-­‐F" " '{print $3}' | sort -­‐u | head
```

# Filter Destination or Source
```
tcpdump -n src host <ip> -­r password_cracking_filtered.pcap
tcpdump -n dst host <ip> ‐r password_cracking_filtered.pcap
tcpdump -n port <port> -­r password_cracking_filtered.pcap
```

# Advanced Header Filtering
```
tcpdump -A -n 'tcp[13] = 24' -­‐r password_cracking_filtered.pcap
```

![](https://github.com/ayomawdb/cheatsheets/raw/master/.data/.images/tcpdump_001.png)

# Other important flags

- `-nn` stop DNS and service names lookup (performance+)
- `-X` and `-XX` can be used to print each packet in hex and ascii
- `-A` print packets in ASCII
- `-S` to print absolute sequence numbers
- `-s` can be used to increase the default snap-length from 262144 to higher
- `-s` 0 to capture full packet
