# Port forwarding on windows

```
netsh interface portproxy delete v4tov4 listenport=4422 listenaddress=192.168.1.111 connectport=80 connectaddress=192.168.0.33
```

Source:
(link)[https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731068(v=ws.10)]


