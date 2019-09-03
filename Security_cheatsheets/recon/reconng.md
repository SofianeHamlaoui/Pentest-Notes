General commands:
```
show modules
keys list

workspace add

show schema
show domains
show hosts
add companies
add domains

search reporting
show dashboard
```

Import emails from harvester, etc.:
```
set TABLE contacts
set COLUMN email
set FILENAME united_emails.txt
run
```

Search showdan for host names:
```
use recon/domains-hosts/shodan_hostname
run
show hosts
show ports
```

Reporting:
```
use report/list
show options
set FILNAME /location/on/file/system
run
```
```
use reporting/html
show options
set CREATOR Pentester
set COMPANY United Airlines
```

# References
> - https://null-byte.wonderhowto.com/how-to/hack-like-pro-reconnaissance-with-recon-ng-part-1-getting-started-0169854/
> - http://securenetworkmanagement.com/recon-ng-tutorial-part-1/
> - http://securenetworkmanagement.com/recon-ng-tutorial-part-2/
> - http://securenetworkmanagement.com/recon-ng-tutorial-part-3/
