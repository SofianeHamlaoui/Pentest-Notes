# List of Services
```
cat /etc/services
```

# Commons service configurations
```
cat /etc/syslog.conf
cat /etc/chttp.conf
cat /etc/lighttpd.conf
cat /etc/cups/cupsd.conf
cat /etc/inetd.conf
cat /etc/apache2/apache2.conf
cat /etc/my.conf
cat /etc/httpd/conf/httpd.conf
cat /opt/lampp/etc/httpd.conf
ls -aRl /etc/ | awk '$1 ~ /^.*r.*/
```

# Check if certain service is up:
```
update-­‐rc.d ssh enable
```

# Auto start a service:
```
update-­‐rc.d ssh enable
```

# Systemd services
```
Example:

/lib/systemd/system/snapd.service
```

# Systemd socket unit file
```
Example:

[Socket]
ListenStream=/run/snapd.socket
ListenStream=/run/snapd-snap.socket
SocketMode=0666
```
`0666` - Allow any process to connect and communicate with the socket.
