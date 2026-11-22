# 01 — Izolowana sieć wewnętrzna i własny serwer DHCP (`dnsmasq`)

Pierwszy etap budowy odizolowanego środowiska testowego w VirtualBoxie: konfiguracja sieci wewnętrznej `cyberlab`, przypisanie statycznego adresu IP na serwerze Ubuntu oraz uruchomienie własnego serwera DHCP rozdającego adresy klientowi Kali Linux.

## Architektura sieci

* **Sieć wirtualna:** Sieć wewnętrzna `cyberlab` (podsieć `10.10.10.0/24`), odizolowana od sieci domowej i internetu (interfejs NAT na serwerze wyłączony po etapie instalacji/pobierania pakietów).
* **Linux Server (Ubuntu):** interfejs `enp0s3` ze statycznym adresem `10.10.10.1/24` (Netplan) oraz aktywny serwer DHCP (`dnsmasq`) rozdający pulę `10.10.10.10`–`10.10.10.50`.
* **Kali Linux (klient):** interfejs `eth0` pobierający adres dynamicznie przez DHCP (np. `10.10.10.41/24`).

## Pliki konfiguracyjne

* `configs/99-cyberlab.yaml` — statyczny adres `10.10.10.1/24` na interfejsie `enp0s3` serwera.
* `configs/cyberlab.conf` — konfiguracja `dnsmasq` przypisana do `enp0s3` (`bind-interfaces`, zakres `10.10.10.10,10.10.10.50,255.255.255.0,12h`).

## Kluczowe komendy i weryfikacja

* `ip -br a` — szybki podgląd stanu interfejsów (`UP`/`DOWN`) oraz przypisanych adresów IP.
* `sudo netplan apply` — zastosowanie konfiguracji z `/etc/netplan/99-cyberlab.yaml`.
* `sudo dnsmasq --test && sudo systemctl restart dnsmasq` — sprawdzenie składni konfiguracji `dnsmasq` i restart usługi.
* `sudo systemctl restart NetworkManager` — wymuszenie ponownego pobrania dzierżawy DHCP na Kali Linux.
* `ping -c 4 10.10.10.1` — weryfikacja łączności sieciowej między Kali a serwerem.
* `cat /var/lib/misc/dnsmasq.leases` — podgląd aktywnych dzierżaw DHCP na serwerze.

## Wnioski i napotkane problemy

* **Wcięcia w YAML:** w Netplanie kluczowe są równe wcięcia ze spacji (bez tabulatorów); edycja w `nano` pozwala szybko wyłapać błędy składni.
* **Spójność sieci w VirtualBoxie:** oba hosty muszą mieć identyczną nazwę sieci wewnętrznej (`cyberlab`) i podłączony kabel sieciowy, aby pakiety DHCP/ICMP docierały między maszynami.
