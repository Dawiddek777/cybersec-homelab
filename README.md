# cybersec-homelab

Odizolowane środowisko testowe w VirtualBoxie do nauki administracji siecią, rekonesansu, ofensywy oraz utwardzania systemów Linux.

## Topologia i założenia

* **Izolacja:** wirtualna sieć wewnętrzna `cyberlab` (`10.10.10.0/24`) bez dostępu do sieci domowej i internetu w trybie roboczym.
* **Hosty:** Linux Server (Ubuntu, statyczny adres `10.10.10.1/24` + serwer DHCP `dnsmasq`) oraz Kali Linux (klient DHCP, np. `10.10.10.41/24`).
* **Zasady:** dedykowane pliki konfiguracyjne zamiast ręcznych obejść na żywo oraz dokumentowanie każdego etapu w osobnym katalogu.

## Etapy laboratorium

* **[01 — Izolowana sieć wewnętrzna i własny serwer DHCP (`dnsmasq`)](./01-network-and-dhcp/)** — konfiguracja Netplan, uruchomienie `dnsmasq` oraz weryfikacja dzierżawy IP i łączności.
* **02 — Rekonesans sieciowy (Nmap)** — w przygotowaniu.

## Zastrzeżenie

Wszystkie testy i techniki opisane w tym repozytorium są wykonywane wyłącznie na własnych maszynach w odizolowanym środowisku laboratoryjnym w celach edukacyjnych.
