# Bezpieczeństwo podstawowych usług sieci lokalnej: DHCP, DNS, NAT, HTTP, FTP, itp.

## 1. Rola i identyfikacja usług sieciowych

* **Model Klient-Serwer:** Usługi sieciowe na serwerach nasłuchują połączeń na zarezerwowanych, powszechnie znanych portach komunikacyjnych (*well-known ports*).
* **Kluczowe porty usług:**
  * **FTP:** Porty 20/21 (transfer plików).
  * **SSH:** Port 22 (bezpieczna powłoka).
  * **DNS:** Port 53 UDP/TCP (system nazw domenowych).
  * **DHCP:** Porty 67/68 UDP (dynamiczne przydzielanie adresów IP).
  * **HTTP / HTTPS:** Port 80 (tekst jawny) / Port 443 (szyfrowany TLS/SSL).
  * **SMB / Samba:** Port 445 TCP (udostępnianie plików i drukarek).

---

## 2. Analiza bezpieczeństwa i zagrożeń w podstawowych usługach

* **DHCP (Dynamic Host Configuration Protocol — port 67/68 UDP):**
  * **Zastosowanie:** Automatyczne przydzielanie adresów IP, masek podsieci oraz adresów bramy i serwerów DNS dla hostów w sieci lokalnej.
  * **Zagrożenia:** Atak *DHCP Spoofing* / *Rogue DHCP Server* (uruchomienie nieautoryzowanego serwera DHCP narzucającego hostom fałszywą bramę lub DNS w celu przechwytywania ruchu) oraz *DHCP Starvation* (wyczerpanie puli adresów IP przez generowanie fałszywych adresów MAC).
  * **Ochrona:** Wdrażanie mechanizmu *DHCP Snooping* na przełącznikach sieciowych.

* **DNS (Domain Name System — port 53 UDP/TCP):**
  * **Zastosowanie:** Mapowanie słownych nazw domenowych na adresy IP.
  * **Zagrożenia:** Zmiana wpisów DNS (*DNS Cache Poisoning* / zatrucie pamięci podręcznej) oraz nieautoryzowany transfer strefy (*Zone Transfer*), pozwalający atakującemu na pełne wyliczenie subdomen i struktury sieci organizacji.
  * **Analiza i ochrona:** Wykorzystywanie narzędzi takich jak `nslookup`, `dig`, `host` oraz `DNSRecon` do weryfikacji rekordów DNS, wdrażanie rozszerzenia **DNSSEC** (podpisywanie cyfrowe rekordów) oraz blokowanie transferu stref dla obcych adresów IP.

* **NAT (Network Address Translation):**
  * **Zastosowanie:** Mapowanie prywatnych (lokalnych) adresów IP na publiczny adres IP na brzegowym routerze lub zaporze.
  * **Znaczenie dla bezpieczeństwa:** Ukrywa wewnętrzną strukturę i adresację IP urządzeń w sieci LAN przed bezpośrednim dostępem z Internetu, tworząc podstawową barierę ochronną.

* **HTTP (port 80) vs HTTPS (port 443):**
  * **HTTP:** Protokół przesyłający dane tekstem jawnym bez szyfrowania. Podatny na pasywny podsłuch (*sniffing*) oraz modyfikację pakietów w sieci lokalnej za pomocą narzędzi takich jak Wireshark czy tcpdump.
  * **HTTPS:** Wersja zabezpieczona szyfrowaniem TLS/SSL, gwarantująca poufność oraz autentyczność komunikacji z serwerem WWW.

* **FTP (port 20/21) vs SSH/SFTP (port 22):**
  * **FTP:** Tradycyjny protokół przesyłania plików, który przesyła poświadczenia (login i hasło) jawnym tekstem w sieci.
  * **Ochrona:** Zastępowanie FTP protokołem **SFTP / SSH**, który wykorzystuje kryptografię asymetryczną do szyfrowania całej sesji.

* **SMB / Samba (port 445 TCP):**
  * **Zastosowanie:** Usługa udostępniania plików, folderów i drukarek w środowiskach Windows oraz Linux (Samba).
  * **Zagrożenia i enumeracja:** Jeden z głównych celów ataków w sieciach wewnętrznych. Atakujący wykorzystują skrypty NSE Nmapa (`smb-enum-users`, `smb-enum-groups`, `smb-enum-shares`) oraz narzędzia takie jak `enum4linux` do wyliczania kont użytkowników, grup oraz udziałów sieciowych pod kątem braku autoryzacji.

---

## 3. Inwentaryzacja i zabezpieczanie usług lokalnych

* **Wykrywanie nieautoryzowanych usług:** Stosowanie poleceń systemowych (`netstat -abno` w Windows, `sudo netstat -tunap` w Linux) do identyfikacji otwartych portów i procesów nasłuchujących w tle.
* **Skanowanie portów:** Testerzy i skanery podatności wykorzystują narzędzie **Nmap** do skanowania portów TCP (skanowanie półotwarte `-sS`, skanowanie pełne `-sT`) oraz UDP (`-sU` dla DNS i DHCP) w celu wykrycia działających usług.
* **Filtrowanie i ochrona (Default Deny):** Konfiguracja zapory ogniowej (np. Windows Defender Firewall) polegająca na otwieraniu wyłącznie niezbędnych portów i automatycznym odrzucaniu każdego nieautoryzowanego ruchu przychodzącego.

---

## 4. Podsumowanie do wypowiedzi na obronie

> *"Bezpieczeństwo podstawowych usług sieciowych opiera się na eliminacji nieszyfrowanych protokołów transmisji (zamiana FTP na SFTP/SSH oraz HTTP na HTTPS) i zabezpieczeniu usług zarządzania siecią. Usługi takie jak DHCP i DNS wymagają ochrony przed atakami typu spoofing i zatruwaniem pamięci podręcznej (np. poprzez DHCP Snooping oraz DNSSEC). Usługi udostępniania zasobów, takie jak SMB na porcie 445, stanowią częsty cel enumeracji użytkowników i udziałów, dlatego kluczowe jest stosowanie zasady minimalnych uprawnień, blokowanie niepotrzebnych usług na zaporze ogniowej (zgodnie z zasadą Default Deny) oraz stałe monitorowanie otwartych portów narzędziami netstat i Nmap."*

## Podsumowanie

- **DHCP:** zagrożenia rogue DHCP i starvation → **DHCP snooping**, port security; **DNS:** poisoning, amplifikacja, tunelowanie → **DNSSEC, wyłączenie otwartej rekursji, filtrowanie, DoT/DoH**; **NAT:** nie jest zaporą – ryzyka port forwarding/UPnP → zapora stanowa; **HTTP:** HTTPS+HSTS, nagłówki, WAF, utwardzony serwer; **FTP:** jawny → **SFTP/FTPS**, Telnet → **SSH**, SNMPv3, wyłączenie SMBv1.
- Dla każdej usługi: minimalna ekspozycja, szyfrowanie, uwierzytelnianie, najmniejsze uprawnienia, aktualizacje, logowanie.

---
[⬅️ Poprzedni temat](9_Najczęstsze_ataki_na_aplikacje_webowe_i_mechanizmy_ich_ograniczania.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](11_Proces_reagowania_na_incydenty_i_podstawy_analizy_powłamaniowej.md)