Count packets sent and recieved:
    iptables -Z && iptables -F
    iptables -I INPUT 1 -s IP -j ACCEPT
    iptables -I OUTOUT 1 -d IP -j ACCEPT
    ip tables -vn -L
