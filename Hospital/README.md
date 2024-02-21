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

https://www.exploit-db.com/exploits/49510 wymaga posiadania konta na serwerze </br>
https://www.exploit-db.com/exploits/40892 to samo </br>
https://www.exploit-db.com/exploits/39245 też </br>

Wcześniejszych nie ma co patrzeć.

---

Z HTTP mam jeszcze HTTP Proxy i HTTP RPC Epmap. Sprawdzam proxy. 

http://hospital.htb:8080/login.php pokazuje stronę logowania do czegoś.

```
$ whatweb hospital.htb:8080
ERROR Opening: http://hospital.htb:8080 - Net::ReadTimeout

$ whatweb hospital.htb:8080
http://hospital.htb:8080 [302 Found] Apache[2.4.55], Cookies[PHPSESSID], Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][Apache/2.4.55 (Ubuntu)], IP[10.10.11.241], RedirectLocation[login.php]
http://hospital.htb:8080/login.php [200 OK] Apache[2.4.55], Bootstrap, Cookies[PHPSESSID], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.55 (Ubuntu)], IP[10.10.11.241], JQuery[3.2.1], PasswordField[password], Script, Title[Login]
```

http://hospital.htb:8080/index.php pozwala mi wysłać coś. Ale wysyła się w nieskończoność ... </br>
Zgaduję, że plik example.jpg będzie można odczytać z ich serwera.

Coś się ruszyło.

```
Successful upload!

Your medical record has been successfully uploaded.
```

Dobra, to gdzie ja to teraz wysłałem ?

```
$ wfuzz -w /usr/share/seclists/Discovery/Web-Content/combined_directories.txt --hc 404 http://hospital.htb:8080/FUZZ
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://hospital.htb:8080/FUZZ
Total requests: 1377715

=====================================================================
ID           Response   Lines    Word       Chars       Payload     
=====================================================================

000000015:   301        9 L      28 W       317 Ch      "css"       
000000009:   301        9 L      28 W       316 Ch      "js"        
000000002:   301        9 L      28 W       320 Ch      "images"    
000000069:   301        9 L      28 W       321 Ch      "uploads"   
000000266:   301        9 L      28 W       315 Ch      "m"         
000000267:   301        9 L      28 W       319 Ch      "fonts"     
000000374:   301        9 L      28 W       315 Ch      "w"         
000000696:   301        9 L      28 W       315 Ch      "u"
000000864:   301        9 L      28 W       315 Ch      "l"         
000001385:   301        9 L      28 W       320 Ch      "vendor"
```

<s>http://hospital.htb:8080/upload/example.jpg not found</s>
http://hospital.htb:8080/uploads/example.jpg -> moje zdjęcie.

Kiedyś już było coś takiego i tam chodziło o pliki .phar
