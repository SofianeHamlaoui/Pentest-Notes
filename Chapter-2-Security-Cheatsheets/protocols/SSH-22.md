# Configuration Files
```
ssh_config
sshd_config
authorized_keys
ssh_known_hosts
.shosts
```

# Tools

# scanssh
```
scanssh -p -r -e excludes random(no.)/Network_ID/Subnet_Mask
```

## SSH Auditor
```
- Re-check all known hosts as new credentials are added. It will only check the new credentials.
- Queue a full credential scan on any new host discovered.
- Queue a full credential scan on any known host whose ssh version or key fingerprint changes.
- Attempt command execution as well as attempt to tunnel a TCP connection.
- Re-check each credential using a per credential scan_interval - default 14 days.
```
- GitHub: https://github.com/ncsa/ssh-auditor

## HASSH - a Profiling Method for SSH Clients and Servers.

"HASSH" is a network fingerprinting standard which can be used to identify specific Client and Server SSH implementations. The fingerprints can be easily stored, searched and shared in the form of an MD5 fingerprint.

- GitHub: https://github.com/salesforce/hassh/
