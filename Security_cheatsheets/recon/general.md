# References
- Exploiting Vulnerabilities Through Proper Reconnaissance: https://docs.google.com/presentation/d/1xgvEScGZ_ukNY0rmfKz1JN0sn-CgZY_rTp2B_SZvijk/edit#slide=id.g4052c4692d_0_0
- Recon My Way: https://github.com/ehsahil/recon-my-way

# Vanquish

Vanquish is Kali Linux based Enumeration Orchestrator.

```
| NMap | Hydra | Nikto | Metasploit | | Gobuster | Dirb | Exploitdb | Nbtscan | | Ntpq | Enum4linux | Smbclient | Rpcclient | | Onesixtyone | Sslscan | Sslyze | Snmpwalk | | Ident-user-enum | Smtp-user-enum | Snmp-check | Cisco-torch | | Dnsrecon | Dig | Whatweb | Wafw00f | | Wpscan | Cewl | Curl | Mysql | Nmblookup | Searchsploit | | Nbtscan-unixwiz | Xprobe2 | Blindelephant | Showmount |
```

# LazyRecon

An automated approach to performing recon for bug bounty hunting and penetration testing.

- Subdomain Enumeration:
  - [Amass](https://github.com/OWASP/Amass)
  - [Subfinder](https://github.com/subfinder/subfinder)
- Subdomain Takeover:
  - [subjack](https://github.com/haccer/subjack)
- CORS Configuration:
  - [CORScanner](https://github.com/chenjj/CORScanner)
- IP Discovery:
  - [Massdns](https://github.com/blechschmidt/massdns)
- Port Scanning:
  - [Masscan](https://github.com/robertdavidgraham/masscan)
  - [Nmap](https://nmap.org/)
  - [Nmap Bootstrap Stylesheet](https://github.com/honze-net/nmap-bootstrap-xsl/)
- Visual Recon:
  - [Aquatone](https://github.com/michenriksen/aquatone)
- Content Discovery:
  - [Dirsearch](https://github.com/maurosoria/dirsearch)
- Wordlists:
  - [JHaddix's all.txt](https://gist.github.com/jhaddix/f64c97d0863a78454e44c2f7119c2a6a)
  - [SecLists' raft-large-words.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-words.txt)


- GitHub: https://github.com/capt-meelo/LazyRecon/

# chomp-scan

Streamline the bug bounty/penetration test reconnaissance phase

- Subdomain Discovery (3 different sized wordlists)
  - dnscan
  - subfinder
  - sublist3r
  - massdns + altdns
  - subjack
- Screenshots (optional)
  - aquatone
- Port Scanning (optional)
  - masscan and/or nmap
- Content Discovery (optional) (4 different sized wordlists)
  - ffuf
  - bfac
  - nikto
  - whatweb
- Wordlists
  - Subdomain Bruteforcing
    - subdomains-top1mil-20000.txt - 22k words - From [Seclists](https://github.com/danielmiessler/SecLists)
    - sortedcombined-knock-dnsrecon-fierce-reconng.txt - 102k words - From [Seclists](https://github.com/danielmiessler/SecLists)
    - huge-200k - 199k words - A combination I made of various wordlists, including Seclists
  - Content Discovery
    - big.txt - 20k words - From [Seclists](https://github.com/danielmiessler/SecLists)
    - raft-large-combined.txt - 167k words - A combination of the raft wordlists in [Seclists](https://github.com/danielmiessler/SecLists)
    - seclists-combined.txt - 215k words - A larger combination of all the Discovery/DNS lists in [Seclists](https://github.com/danielmiessler/SecLists)
    - haddix_content_discovery_all.txt - 373k words - Jason Haddix's [all](https://gist.github.com/jhaddix/b80ea67d85c13206125806f0828f4d10/) content discovery list
    - haddix-seclists-combined.txt - 486k words - A combination of the two previous lists
  - Misc.
    - altdns-words.txt - 240 words - Used for creating domain permutations for [masscan](https://github.com/robertdavidgraham/masscan) to resolve. Borrowed from [altdns](https://github.com/infosec-au/altdns/blob/master/words.txt).
    - interesting.txt - 42 words - A list I created of potentially interesting words appearing in domain names.


- GitHub: https://github.com/SolomonSklash/chomp-scan

# pown-recon

A powerful target reconnaissance framework powered by graph theory.

- GitHub: https://github.com/pownjs/pown-recon
