## Check Capabilities
```
davtest -url http://grandpa.htb/
```

## Interact
- Command-line WebDAV client for Unix: http://www.webdav.org/cadaver/
- Commands: https://www.systutorials.com/docs/linux/man/1-cadaver/

```
cadaver http://10.10.10.15


```

## Exploits:
- IIS 6.0 in Microsoft Windows Server 2003 R2 (CVE-2017-7269): iis_webdav_scstoragepathfromurl
    - https://www.rapid7.com/db/modules/exploit/windows/iis/iis_webdav_scstoragepathfromurl
    - https://github.com/edwardz246003/IIS_exploit
    - https://blog.0patch.com/2017/03/0patching-immortal-cve-2017-7269.html
    - https://github.com/gottburgm/Exploits/blob/master/CVE-2017-7269/CVE_2017_7269.pl
- IIS 6.0
