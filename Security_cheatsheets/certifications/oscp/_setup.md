- Terminator Configuration / Zsh: https://guide.offsecnewbie.com/kali-configuration/terminator-shortcuts
- CherryTree template: https://guide.offsecnewbie.com/cherrytree-oscp-template

# Aliases
```
## ALIASES
alias vpn='openvpn /root/pwnshare/VPN/OS-XXXXX-PWK.ovpn'
alias rdp='rdesktop -g 85% -u offsec -p PASSWORD_HERE 10.11.14.134 &'
alias mapshare='ln -s /mnt/hgfs/Pwn_Share/ /root/pwnshare'
alias l='ls -la'
alias webup='python -m SimpleHTTPServer 80'
//  run tcpdump’s from VPN interface and to only look at source IP’s from the other attackers range and with a destination of my assigned IP
alias shieldsup='tcpdump -i tap0 -nnvv src net 10.11.0.0/24 and dst 10.11.0.54 -w - | tee capture.pcap | tcpdump -n -r -'
alias ss='searchsploit $1'
alias ssx='searchsploit -x $1'
```


# Important Locations
```
/etc/
/etc/passwd
/etc/fstab
/etc/hosts
/etc/init.d
/usr/sbin
```
