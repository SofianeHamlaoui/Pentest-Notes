# DNS on TCP
- Check for zone transfers
- Maybe DNS Sec enabled

# Dnsrecon
```
dnsrecon -r 127.0.0.0/24 -n 10.10.10.29
dnsrecon -r 127.0.1.0/24 -n 10.10.10.29
dnsrecon -r 10.10.10.0/24 -n 10.10.10.29
```

# Configuration files
```
host.conf
resolv.conf
named.conf
```

# Order of name resolution
```
/etc/nsswitch.conf
```

# DNS sever Information
```
/etc/resolv.conf
```

# Forward Lookup

## whois
```
whois example.com
whois 50.7.67.186 (reverse)
```

## host
```
host -t ns example.com
host -t mx example.com

host www.example.com -> results in IP address
host nonexistent.example.com -> results in not found error
```

## nslookup
```
nslookup <ip>
```
```
> set type=a
> google.com

> server ns1.google.com
> google.com
```

## dig
```
Usage:  dig [@global-server] [domain] [q-type] [q-class] {q-opt}
            {global-d-opt} host [@local-server] {local-d-opt}
            [ host [@local-server] {local-d-opt} [...]]

dig google.com
dig google.com mx
dig @ns1.google.com google.com
```

# Reverse Lookup
- write the IP-address in reverse order (for e.g. 192.168.1.1 will be 1.1.168.192)
- append “.in-addr.arpa.” to it.

```
dig 1.1.168.192.in-addr.arpa. PTR
```

```
for ip in $(seq 155 190);do host 50.7.67.$ip;done |grep -­‐v "not found"
```
> https://stackoverflow.com/questions/23981098/how-forward-and-reverse-dns-works

# Zone Transfers

Copying of the zone file from a master DNS server to a slave server

```
host -t axfr domain.name dns-server
``` 

Root zone:
```
dig axfr @dns-server
```

Domain name:
```
dig axfr @dns-server domain.name
```
```
host -l example.com ns1.example.com
```
```
dnsrecon -d $ip -t axfr
```
```
nmap $ip --script=dns-zone-transfer -p 53
```

```
#/bin/bash
# Simple Zone Transfer Bash Script
# $1 is the first argument given after the bash script
# Check if argument was given, if not, print usage
if [ -­‐z "$1" ]; then
echo "[*] Simple Zone transfer script"
echo "[*] Usage : $0 <domain name> "
exit 0
fi

# if argument was given, identify the DNS servers for the domain
for server in $(host -­‐t ns $1 |cut -­‐d" " -­‐f4);do
# For each of these servers, attempt a zone transfer
host -­l $1 $server |grep "has address"
done
```

# Bruteforcing

## Subdomain bruteforcing
```
for ip in $(cat list.txt); do host $ip.$website; done
```

## Reverse dns lookup bruteforcing
```
for ip in $(seq 155 190);do host 50.7.67.$ip;done |grep -v "not found"
```

# DNS Related Recon Tools

## fierce
- scanner that helps locate non-contiguous IP space and hostnames against specified domains.
- pre-cursor to nmap, unicornscan, nessus, nikto, etc, since all of those require that you already know what IP space you are looking for
- GitHub: https://github.com/davidpepper/fierce-domain-scanner

General checks:
```
fierce -dns example.com
```

Wordlist attack:
```
fierce -dns example.com -wordlist hosts.list
```

## recon-ng
```
use recon/contacts/gather/http/api/whois_pocs
set DOMAIN example.com
run

use recon/hosts/enum/http/web/xssed
use recon/hosts/gather/http/web/google_site
use recon/hosts/gather/http/web/ip_neighbor
```

## dnsenum
```
dnsenum $ip
```

## dnsrecon
```
dnsrecon
dnsrecon ‐d example.com ‐t axfr

dnsenum
dnsenum example.com
```

## subbrute
- Recursively crawls enumerated DNS records
- Uses open resolvers as a kind of proxy to circumvent DNS rate-limiting

## knock
- Wordlist based subdomain bruteforcing
- Virustotal search

## Sublist3r
- Subdomains with Google, Yahoo, Bing, Baidu, Ask, Netcraft, Virustotal, ThreatCrowd, DNSdumpster, and ReverseDNS
- Can do "subbrute" scans internally
- Can do port scans internally

## Online Services
- https://dnsdumpster.com/

# DNS Recon Workflow (WIP)

1) Get the host's addresse (A record).
2) Get the namservers (threaded).
3) Get the MX record (threaded).
4) Perform axfr queries on nameservers and get BIND VERSION (threaded).
5) Get extra names and subdomains via google scraping (google query = "allinurl: -www site:domain").
6) Brute force subdomains from file
7) Calculate C class domain network ranges and perform whois queries on them (threaded).
8) Perform reverse lookups on netranges (C class or/and whois netranges) (threaded).
9) Write to domain_ips.txt file ip-blocks.
10)

# References
- Payload Delivery Over DNS: https://github.com/no0be/DNSlivery
- DNS Rebind Toolkit https://github.com/Kinimiwar/dns-rebind-toolkit
- Global DNS Hijacking Campaign: DNS Record Manipulation at Scale: https://eforensicsmag.com/global-dns-hijacking-campaign-dns-record-manipulation-at-scale-by-muks-hirani-sarah-jones-ben-read/
