W tej chwili jedyną łatwą maszyną windows jest crafty. Jestem biedny i nie mam MineCrafta. 
Trudnych nie robię, a ze średnich spróbujmy ze szpitalem.

---

```
$ nmap -sT 10.10.11.241
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-02-21 13:16 CET
Nmap scan report for 10.10.11.241
Host is up (0.066s latency).
Not shown: 987 filtered tcp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh
53/tcp   open  domain
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
443/tcp  open  https
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd5
593/tcp  open  http-rpc-epmap
1801/tcp open  msmq
2105/tcp open  eklogin
2179/tcp open  vmrdp
3389/tcp open  ms-wbt-server
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 7.53 seconds
```
Niech no zgadnę ... muszę wyciągnąć hasła z kpasswd5 czymkolwiek jest i próbować dostać się do SSH.

---

https://10.10.11.241/ pokazuje Hospital Webmail. Z designu wygląda mi na RoundMail czy jak mu tam było.

Copyright (C) The Roundcube Dev Team -> czyli miałem rację. Nie widzę wersji. 
wfuzz pokazuje same 403.

```
$ whatweb https://10.10.11.241/
https://10.10.11.241/ [200 OK] Apache[2.4.56], Bootstrap, Content-Language[en], Cookies[roundcube_sessid], Country[RESERVED][ZZ], HTML5, HTTPServer[Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.0.28], HttpOnly[roundcube_sessid], IP[10.10.11.241], JQuery, OpenSSL[1.1.1t], PHP[8.0.28], PasswordField[_pass], RoundCube, Script, Title[Hospital Webmail :: Welcome to Hospital Webmail], UncommonHeaders[x-robots-tag], X-Frame-Options[sameorigin], X-Powered-By[PHP/8.0.28]
```
