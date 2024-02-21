# HackThisBox
HackTheBox Windows Edition

Według legendy rok temu w zaledwie jeden miesiąc zyskałem rangę hakiera na HTB. 
Miałem potem zrobić pierwszą fortecę ale najpierw chciałem przejść kilka maszyn z Windowsem. 
Tutaj będę sprawdzać czy cokolwiek udało mi się zapamiętać.

Żeby nie było gdzie niby napisano, że nie można pisać poradników ? 
Mówią tylko, by nie dawać gotowych rozwiązań.

# Przypominajka

```sudo openvpn --config lab_onliniak.ovpn```
```sudo nano /etc/hosts``` || dnsmasq

Katalog:
```
|-projekty
|-- Maszyna
|--- notes
|---- README.md
|---- Sprawdzić.txt
|--- ważne
|---- domeny.txt
|---- użytkownicy.txt
|---- ip.txt
```

## Infiltracja

### Serwery w sieci

[Szybkie skanowanie](https://explainshell.com/explain?cmd=nmap+-sT)
[Pełne skanowanie](https://explainshell.com/explain?cmd=nmap+-p-+-sC+-A) trwające na Ryzenie 7 5800X ECO 15 minut.

### Skręcać włosy na barana

https://owasp.org/www-community/Fuzzing

Generalnie wordlist jest ważniejszy od programu ale zdarzyło mi się, że dirb z identyczną listą znalazł więcej katalogów od wfuzz.

```wfuzz -w wordlist/general/common.txt --hc 404,403 http://testphp.vulnweb.com/FUZZ```
```wfuzz -w wordlist/general/common.txt --hc 404,403,400,302 -u 'http://testphp.vulnweb.com/' -H "Host: FUZZ.vulnweb.com" ``` [Apache, nie wiem jak na innych serwerach]

#### Słowolisty

```
/usr/share/seclists/Discovery/Web-Content/api/api-endpoints.txt
/usr/share/seclists/Discovery/Web-Content/dirsearch.txt
/usr/share/wordlists/dirb/medium.txt
/usr/share/wordlists/fuzzdb/discovery/predictable-filepaths/filename-dirname-bruteforce/raft-small-directories.txt
```

## Foothold

Nie wiem jak w Windowsie ale na Linuksie zawsze były 3 błędy:

- SuDo
- Cron
- Pliki konfiguracyjne
  - Git
