# Configuration files
```
sendmail.cf
submit.cf
```

# General
```
nc -‐nv 192.168.11.215 25
```

# "HELP" Command

# User enumeration with: "VRFY", "EXPN" & "RCPT" commands:

```
VRFY username (verifies if username exists - enumeration of accounts)
EXPN username (verifies if username is valid - enumeration of accounts)
```

```
  VRFY existing_user
  Results in: 250

  VRFY nonexisting_user
  Results in: 550

  for user in $(cat users.txt); do echo VRFY $user | nc -nv -w <ip> 25 2>/dev/null | grep ^"250"; done
```

# Mail Spoofing
```
HELO anything MAIL FROM: spoofed_address RCPT TO:valid_mail_account DATA . QUIT
```
> http://0daysecurity.com/penetration-testing/enumeration.html

# Mail Relay
```
HELO anything

Identical to/from - mail from: <nobody@domain> rcpt to: <nobody@domain>
Unknown domain - mail from: <user@unknown_domain>
Domain not present - mail from: <user@localhost>
Domain not supplied - mail from: <user>
Source address omission - mail from: <> rcpt to: <nobody@recipient_domain>

Use IP address of target server - mail from: <user@IP_Address> rcpt to: <nobody@recipient_domain>
Use double quotes - mail from: <user@domain> rcpt to: <"user@recipent-domain">

User IP address of the target server - mail from: <user@domain> rcpt to: <nobody@recipient_domain@[IP Address]>

Disparate formatting - mail from: <user@[IP Address]> rcpt to: <@domain:nobody@recipient-domain>

Disparate formatting2 - mail from: <user@[IP Address]> rcpt to: <recipient_domain!nobody@[IP Address]>
```
> http://0daysecurity.com/penetration-testing/enumeration.html

# Tools

## smtp_enum
```
auxiliary/scanner/smtp/smtp_enum
```

## smtp-enum-users.nse
```
nmap –script smtp-enum-users.nse 172.16.212.133
```
