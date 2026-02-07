## Comfast CF-E7 Router

- **Supplier:** COMFAST

- **Product:** COMFAST CF-E7

- **Firmware Version:** V2.6.0.9

- **Vulnerability:** Command Injection

### Overview

A command injection vulnerability has been discovered in the COMFAST CF-E7 V2.6.0.9 Router. Attackers can exploit this vulnerability to execute arbitrary commands by sending malicious HTTP POST packets. The vulnerability is triggered when the request path is `/cgi-bin/mbox-config?method=SET&section=ping_config`, prompting the user to log in and obtain cookies.

### Vulnerability Details

The trigger point is located in the `sub_441CF4` method of the `webmggnt` component. Here, the `destination` field is not validated and is concatenated using `sprintf`, and then executed using `system`.

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-07-15-26-47-image.png)

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-07-15-27-14-image.png)

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-07-15-27-36-image.png)

### poc

```http

```

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-07-15-29-37-image.png)
