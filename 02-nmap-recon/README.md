# 02 — Rekonesans usługi `dnsmasq` za pomocą Nmap

Skanowanie hosta `10.10.10.1` z poziomu Kali Linux w odizolowanej sieci wewnętrznej `cyberlab` (`10.10.10.0/24`), mające na celu potwierdzenie widoczności usług DNS i DHCP oraz ocenę otwartej powierzchni sieciowej.

## Cel i wyniki skanowania (`10.10.10.1`)

* **Skan TCP (top 1000):** otwarty wyłącznie port `53/tcp` (`domain`, wykryto `dnsmasq 2.92`), pozostałe 999 portów w stanie `closed`.
* **Pełny skan TCP (wszystkie 65535 portów):** 65534 porty zamknięte (`closed`), jedynym otwartym portem TCP pozostaje `53/tcp` (`dnsmasq-2.92`, odczytany m.in. przez skrypt `dns-nsid`).
* **Skan UDP (porty `53,67,68`):**
  * `53/udp` — `open` (`domain`, `dnsmasq 2.92`, obsługa zapytań DNS),
  * `67/udp` — `open` (`dhcps?`, serwer DHCP w `dnsmasq`),
  * `68/udp` — `closed` (`dhcpc`, port klienta DHCP).
* **Adres MAC interfejsu:** `08:00:27:AD:98:F2` (karta wirtualna Oracle VirtualBox).

## Użyte komendy Nmap

* `nmap -sS -sV -T4 10.10.10.1` — szybkie skanowanie SYN (`-sS`) z detekcją wersji usług (`-sV`) i szablonem czasowym `T4`.
* `sudo nmap -sU -sV -p 53,67,68 -T4 10.10.10.1 -oN 02-nmap-udp-53-67-68.txt` — skanowanie wybranych portów UDP (`-sU`) związanych z DNS/DHCP z zapisem raportu (`-oN`).
* `nmap -sS -sC -p- -T4 10.10.10.1 -oN 02-nmap-all-tcp.txt` — skan wszystkich 65535 portów TCP (`-p-`) z domyślnymi skryptami NSE (`-sC`) i zapisem wyniku do pliku.

## Wnioski

* Skanowanie TCP pokazuje wyłącznie usługę na porcie `53/tcp`, natomiast pełny obraz działania `dnsmasq` wymaga również weryfikacji UDP (szczególnie portu `67/udp` dla DHCP).
* Skanowanie UDP trwa wyraźnie dłużej niż skanowanie TCP ze względu na brak natychmiastowych odpowiedzi w części przypadków.
* W skanie portów widoczna jest wersja usługi (`dnsmasq 2.92`), ale samo skanowanie portów nie pokazuje podatności logicznych protokołu DHCP (np. w warstwie rozgłoszeń/dzierżaw w sieci lokalnej).
