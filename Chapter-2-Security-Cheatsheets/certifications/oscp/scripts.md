# Enumeration

## TCP
`nmap -sS -sV -sC -n [IP]`
`nmap -vv -Pn -A -sC -sS -T 4 -p- 10.0.0.1`

## UDP
`nmap -sU -sV -n --top-ports 200 [IP]`

## SNMP (UDP 161)
`snmp-check -t [IP] -c public`

## SMTP
```
nmap –script=smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 -p 25 10.0.0.1
```

## SNMP
```
snmpwalk -c public -v1 10.0.0.0
```

## SMB (TCP 139/TCP 445)
`enum4linux [IP]`

## WebDav
`davtest -url http(s)://[IP]`

## FTP:
```
ftp [IP]
Username: anonymous Password: anonymous
```
```
nmap –script=ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221,tftp-enum -p 21 10.0.0.1
```

## SMB (anonymous)
```
smbclient -L \\[IP]
Username: root Password: None
```
```
enum4linux 10.0.0.1
```
```
nmap –script=smb-enum-domains.nse,smb-enum-groups.nse,smb-enum-processes.nse,smb-enum-sessions.nse,smb-enum-shares.nse,smb-enum-users.nse,smb-ls.nse,smb-mbenum.nse,smb-os-discovery.nse,smb-print-text.nse,smb-psexec.nse,smb-security-mode.nse,smb-server-stats.nse,smb-system-info.nse,smb-vuln-conficker.nse,smb-vuln-cve2009-3103.nse,smb-vuln-ms06-025.nse,smb-vuln-ms07-029.nse,smb-vuln-ms08-067.nse,smb-vuln-ms10-054.nse,smb-vuln-ms10-061.nse,smb-vuln-regsvc-dos.nse,smbv2-enabled.nse 10.0.0.1
```

## MySQL
```
nmap -sV -Pn -vv –script=mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 10.0.0.1 -p 3306
```

# Web Applications

## Structure Discovery
```
dirb http://10.0.0.1 /usr/share/wordlists/dirb/common.txt
```

## Vulnerability Discovery
`nikto -h http(s)://[IP]:[PORT]/[DIRECTORY]`


# Password Attacks
- https://hashkiller.co.uk

```
john hashes.txt
hashcat -m 500 -a 0 -o output.txt –remove hashes.txt /usr/share/wordlists/rockyou.txt
```
```
hydra 10.0.0.1 http-post-form “/admin.php:target=auth&mode=login&user=^USER^&password=^PASS^:invalid” -P /usr/share/wordlists/rockyou.txt -l admin

hydra -l admin -P /usr/share/wordlists/rockyou.txt -o results.txt ssh://10.0.0.1
```

# Tunneling
Tunnels traffic through 10.0.0.1 and makes a route for all traffic destined for 10.10.10.0/24 through your sshuttle tunnel.
```
sshuttle -r root@10.0.0.1 10.10.10.0/24
```
```
sshuttle -l (any port) -r root@10.10.0.1 10.10.0.0/24
```

# AV Bypassing
```
root@kali:~/Hyperion-1.0# wine hyperion.exe ../backdoor.exe ../backdoor_mutation.exe
```





https://github.com/furrukhtaj/Enumerator

# Scripts from awansec
https://awansec.com/oscp-review.html
