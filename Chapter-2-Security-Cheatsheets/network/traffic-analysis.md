# Tools

- Netcap - A framework for secure and scalable network traffic analysis: https://github.com/dreadl0ck/netcap

# PA Toolkit (Pentester Academy Wireshark Toolkit)

PA Toolkit is a collection of traffic analysis plugins to extend the functionality of Wireshark from a micro-analysis tool and protocol dissector to the macro analyzer and threat hunter.

- GitHub: https://github.com/pentesteracademy/patoolkit

# Sample Captures:
- https://wiki.wireshark.org/SampleCaptures

# TShark

## General

Supported network interfaces
```
tshark -D
```

Sniff on eth0
```
tshark -i eth0
```

Open `pcap`
```
tshark -r HTTP_traffic.pcap
```

Read 100 packets from `pcap`
```
tshark -r HTTP_traffic.pcap -c 100
```

Print full details for first 10 Packets
```
tshark -r HTTP_traffic.pcap -c 10 -V
```

List of protocols in `pcap`
```
tshark -r HTTP_traffic.pcap -z io,phs -q
```

Export into PDML
```
tshark -r HTTP_traffic.pcap  -T pdml > http.xml
```

PDML to HTML
```
xsltproc /usr/share/wireshark/pdml2html.xsl http.xml > http.html
```

## HTTP

Only the HTTP traffic
```
tshark -Y 'http' -r HTTP_traffic.pcap
```

IP packets sent from IP address 192.168.252.128 to 52.32.74.91?
```
tshark -r HTTP_traffic.pcap -Y "ip.src==192.168.252.128 && ip.dst==52.32.74.91"
```

Packets containing GET requests
```
tshark -r HTTP_traffic.pcap -Y "http.request.method==GET"
```

Print only source IP and URL for all GET request packets
```
tshark -r HTTP_traffic.pcap -Y "http.request.method==GET" -Tfields -e frame.time -e ip.src -e http.request.full_uri
```

Packets contain the "password" string
```
tshark -r HTTP_traffic.pcap -Y "http contains password”
```

Destination IP for GET requests sent to New York Times (www.nytimes.com)
```
tshark -r HTTP_traffic.pcap -Y "http.request.method==GET && http.host==www.nytimes.com" -Tfields -e ip.dst
```

Session ID being used by 192.168.252.128 for Amazon India store (amazon.in)?
```
tshark -r HTTP_traffic.pcap -Y "ip contains amazon.in && ip.src==192.168.252.128" -Tfields -e ip.src -e http.cookie
```

Type of OS 192.168.252.128 is using
```
tshark -r HTTP_traffic.pcap -Y "ip.src==192.168.252.128 && http" -Tfields -e http.user_agent
```

## HTTPS

Only show SSL traffic?
```
tshark -r HTTPS_traffic.pcap -Y 'ssl'
```

Only print the source IP and destination IP for all SSL handshake packets
```
tshark -r HTTPS_traffic.pcap -Y "ssl.handshake" -Tfields -e ip.src -e ip.dst
```

List issuer name for all SSL certificates exchanged
```
tshark -r HTTPS_traffic.pcap -Y "ssl.handshake.certificate" -Tfields -e x509sat.printableString
```

Print the IP addresses of all servers accessed over SSL
```
tshark -r HTTPS_traffic.pcap -Y "ssl && ssl.handshake.type==1" -Tfields -e ip.dst
```

IP addresses associated with Ask Ubuntu servers (askubuntu.com)
```
tshark -r HTTPS_traffic.pcap -Y "ip contains askubuntu"
```

The IP address of the user who interacted with with Ask Ubuntu servers (askubuntu.com)
```
tshark -r HTTPS_traffic.pcap -Y "ip.dst==151.101.1.69 || ip.dst==151.101.193.69 || ip.dst==151.101.129.69 || ip.dst==151.101.65.69" -Tfields -e ip.src
```

What DNS servers were used
```
tshark -r HTTPS_traffic.pcap -Y "dns && dns.flags.response==0" -Tfields -e ip.dst
```

Name of the antivirus solution
```
tshark -r HTTPS_traffic.pcap -Y "ip contains avast" -Tfields -e ip.src
```
> Answer: Avast antivirus, 192.168.10.9, 192.168.0.1, 192.168.0.136

## WiFi

Show only WiFi traffic
```
tshark -r WiFi_traffic.pcap -Y "wlan"
```


View the deauthentication packets
```
tshark -r WiFi_traffic.pcap -Y "wlan.fc.type_subtype==0x000c"
```

Display WPA handshake packets
```
tshark -r WiFi_traffic.pcap -Y "eapol"
```

Print the SSID and BSSID values for all beacon frames
```
tshark -r WiFi_traffic.pcap -Y "wlan.fc.type_subtype==8" -Tfields -e wlan.ssid -e wlan.bssid
```

What is BSSID of SSID "LazyArtists"
```
tshark -r WiFi_traffic.pcap -Y "wlan.ssid==LazyArtists" -Tfields -e wlan.bssid
```

Channel of SSID "Home_Network"
```
tshark -r WiFi_traffic.pcap -Y "wlan.ssid==Home_Network" -Tfields -e wlan_radio.channel
```

Devices that received deauth messages
```
tshark -r WiFi_traffic.pcap -Y "wlan.fc.type_subtype==0x000c" -Tfields -e wlan.ra
```

Which device does MAC 5c:51:88:31:a0:3b belongs to
```
tshark -r WiFi_traffic.pcap -Y "wlan.ta==5c:51:88:31:a0:3b && http" -Tfields -e http.user_agent
```

Beacon frames present
```
tshark -r WiFi_traffic.pcap -Y 'wlan.fc.type_subtype == 0x0008'
```

Unique list of all AP BSSIDs
```
tshark -r WiFi_traffic.pcap -Y 'wlan.fc.type_subtype == 0x0008' -T fields -e wlan.bssid | sort  | uniq
```

Unique list of all AP SSIDs
```
tshark -r WiFi_traffic.pcap -Y 'wlan.fc.type_subtype == 0x0008' -T fields -e wlan.ssid | sort  | uniq
```

Only non-null SSIDs
```
tshark -r WiFi_traffic.pcap -Y 'wlan.fc.type_subtype == 0x0008 && !(wlan.tag.length ==0)' -T fields -e wlan.ssid | sort  | uniq
```

Unique list of SSID and BSSIDs side by side for all AP networks
```
tshark -r WiFi_traffic.pcap -Y 'wlan.fc.type_subtype == 0x0008' -T fields -e wlan.ssid -e wlan.bssid | sort  | uniq
```

## VoIP

Show VoIP traffic
```
tshark -r VoIP_traffic.pcap -Y "sip or rtp"
```

Print all REGISTER packets
```
tshark -r VoIP_traffic.pcap -Y "sip.Method==REGISTER"
```

Only print the source IP, sender extension and authorization digest response for REGISTER packets
```
tshark -r VoIP_traffic.pcap -Y "sip.Method==REGISTER" -Tfields -e ip.src -e sip.from.user -e sip.auth.digest.response
```

Print all codecs being used by RTP protocol
```
tshark -r VoIP_traffic.pcap -Y "sdp" -Tfields -e sdp.media
```

User who is using the Zoiper VoIP client
```
tshark -r VoIP_traffic.pcap -Y "sip contains Zoiper" -Tfields -e ip.src
```

IP address of the SIP server used to place calls
```
tshark -r VoIP_traffic.pcap -Y "sip.Method==REGISTER" -Tfields -e ip.dst
```

Content of the text message sent to +918108591527?
```
tshark -r VoIP_traffic.pcap -Y "sip.Method == MESSAGE" -V             (Read the content)
```

Extensions completed a call successfully
```
tshark -r VoIP_traffic.pcap -Y "sip.Method==BYE" -Tfields -e sip.from.user -e sip.to.user
```

## CapAnalysis

> https://drive.google.com/uc?authuser=0&id=1Lf8yU7alFZdRlT6WstKjngN2RAp-1jG8&export=download
