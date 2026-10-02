# Bezpieczeństwo podstawowych usług sieci lokalnej: DHCP, DNS, NAT, HTTP, FTP itp.

> Wykład: W2 (slajdy 19–25, 28, 53: serwery, usługi i porty, laboratorium Nmap/Telnet/UFW), W1 (slajdy 66–67: SMB, udziały sieciowe), kursy Cisco z poprzednich przedmiotów (Wireshark: Telnet vs SSH, NetFlow). Zagrożenia i zabezpieczenia poszczególnych usług – głównie ***(uzupełnienie)***.

## Zasada ogólna

Usługa = proces nasłuchujący na **porcie** (wykład W2, slajd 20). Każda usługa zwiększa **powierzchnię ataku**. Dlatego: uruchamiać **tylko niezbędne**, na właściwych interfejsach, **z najmniejszymi uprawnieniami**, szyfrowaną komunikację, aktualizacje, zapora, logowanie, monitoring (temat 1). Inwentaryzacja: `ss -tulpn`, `netstat -ano`, **Nmap** (laboratorium W2: *wykrywanie otwartych portów*), skanery podatności.

## Tabela portów i protokołów (wykład W2, slajdy 20–21) z oceną bezpieczeństwa

| Port | Usługa | Szyfrowanie | Zalecenie |
| :-: | :--- | :-: | :--- |
| 20/21 | **FTP** | nie | zastąpić **SFTP/FTPS** |
| 22 | **SSH** | tak | klucze, bez roota, ograniczenia |
| 23 | **Telnet** | **nie** (hasła jawnie) | **wyłączyć**, użyć SSH |
| 25 | SMTP | opcjonalnie (STARTTLS) | TLS, uwierzytelnianie, SPF/DKIM/DMARC, bez open relay |
| 53 | **DNS** | nie (domyślnie) | DNSSEC, ograniczenie rekursji, DoT/DoH |
| 67/68 | **DHCP** | nie | DHCP snooping |
| 69 | TFTP | nie | ograniczyć (tylko sieć zarządzania) |
| 80 | **HTTP** | nie | przekierowanie na HTTPS, HSTS |
| 110 / 143 | POP3 / IMAP | nie | wersje z TLS (995/993) |
| 123 | NTP | nie | ograniczyć, aktualizacje |
| 161/162 | **SNMP** | v1/v2c nie | **SNMPv3** |
| 443 | **HTTPS** | tak | TLS 1.2/1.3, silne szyfry |
| *(uzup.)* 445 | SMB | v3 opcjonalnie | wyłączyć SMBv1, podpisywanie/szyfrowanie |
| *(uzup.)* 3389 | RDP | tak | NLA, MFA, VPN, nie do Internetu |
| *(uzup.)* 389 / 636 | LDAP / LDAPS | nie / tak | LDAPS/StartTLS |

Wykład W2 (slajd 25 i laboratorium 53): **testowanie usług TCP za pomocą Telnet** i konfiguracja **UFW**, aby **blokować niezabezpieczony ruch Telnet** i wyłączyć usługę. Laboratorium W3 z poprzedniego przedmiotu (Wireshark): **Telnet jest czytelny w przechwyceniu, SSH – zaszyfrowany.**

## 1. DHCP – Dynamic Host Configuration Protocol

**Funkcja:** automatyczne przydzielanie klientom **adresu IP, maski, bramy, DNS** (UDP 67 serwer / 68 klient; proces DORA: Discover–Offer–Request–Ack). Brak uwierzytelniania.

| Zagrożenie | Opis | Obrona |
| :--- | :--- | :--- |
| **rogue DHCP** | fałszywy serwer podaje własną bramę/DNS → MITM, przekierowanie | **DHCP snooping** (porty trusted/untrusted) na przełącznikach, 802.1X |
| **DHCP starvation** | wyczerpanie puli żądaniami z fałszywych MAC | port security (limit MAC), limit żądań, większa pula |
| **spoofing / podszywanie** | użycie cudzego adresu | DAI, IP Source Guard (powiązania MAC–IP–port) |
| ujawnianie informacji | opcje DHCP (nazwy, serwery) | minimalne opcje |

Dobre praktyki: **rezerwacje** dla krytycznych urządzeń, logi DHCP (powiązanie IP–MAC–czas – ważne dla analizy powłamaniowej), segmentacja VLAN, **DHCPv6/RA Guard** dla IPv6.

## 2. DNS – Domain Name System

**Funkcja:** tłumaczenie nazw na adresy (UDP/TCP 53); rekursywny resolver, serwery autorytatywne, cache. Domyślnie **bez uwierzytelniania i szyfrowania**.

| Zagrożenie | Opis | Obrona |
| :--- | :--- | :--- |
| **cache poisoning / spoofing** | fałszywe odpowiedzi w cache → przekierowanie ruchu | **DNSSEC** (podpisy), losowe porty i ID, aktualne oprogramowanie |
| **amplifikacja DDoS** | otwarty resolver wzmacnia ruch ofiary (spoofing źródła) | **wyłączenie otwartej rekursji** (tylko dla klientów), rate limiting (RRL), BCP 38 |
| **zone transfer (AXFR) ujawnia strefę** | wyciek wszystkich rekordów | ograniczenie do serwerów wtórnych (ACL, TSIG) |
| **tunelowanie DNS / C2 / eksfiltracja** | dane w zapytaniach | monitoring (długie/entropia), filtrowanie DNS, ograniczenie wychodzącego DNS do własnych resolverów |
| **domeny złośliwe, DGA** | malware | **filtrowanie DNS**, reputacja |
| **przejęcie domeny/subdomain takeover, DNS hijacking** | błędna konfiguracja/rejestratora | MFA u rejestratora, przegląd rekordów |
| **ujawnianie wewnętrznych nazw** | wyciek topologii | split-horizon DNS |
| podsłuch zapytań | jawny DNS | **DNS over TLS (853) / over HTTPS (443)** |

## 3. NAT – Network Address Translation

**Funkcja:** translacja adresów prywatnych na publiczne (**NAT/PAT** – wielu hostów za jednym adresem); oszczędza adresy IPv4.

**Bezpieczeństwo – NAT nie jest zaporą:** daje pewną **ukrytość** (zewnętrzny host nie może zainicjować połączenia do hosta za NAT bez przekierowania), ale to *efekt uboczny*, nie mechanizm bezpieczeństwa (wykład z poprzedniego przedmiotu: dodatkowa warstwa *obscurity*). **Ryzyka:** **przekierowania portów (port forwarding)**, **UPnP/NAT-PMP** (aplikacje otwierają porty automatycznie – wyłączyć), **NAT slipstreaming**, utrata źródłowego adresu w logach (potrzebne logi translacji – analiza incydentów!), CGNAT (współdzielony adres), brak ochrony przed ruchem wychodzącym i phishingiem. **Zalecenia:** stanowa **zapora** niezależnie od NAT, minimalne reguły DNAT, wyłączony UPnP, logowanie translacji, IPv6 z zaporą (brak NAT ≠ brak ochrony).

## 4. HTTP/HTTPS – serwery WWW

**Funkcja:** transport stron i API (80/443). HTTP jawny, HTTPS (TLS) szyfrowany.

| Zagrożenie | Obrona |
| :--- | :--- |
| podsłuch i MITM (HTTP) | **HTTPS wszędzie, HSTS**, certyfikaty zarządzane (ACME), TLS 1.2/1.3 |
| ataki aplikacyjne (SQLi, XSS, CSRF…) | zob. temat 9, **WAF** |
| błędy konfiguracji: listing katalogów, domyślne strony, ujawnianie wersji, panele administracyjne | utwardzenie (`ServerTokens Prod`, wyłączenie `autoindex`, usunięcie domyślnych plików), restrykcje dostępu |
| uruchamianie z wysokimi uprawnieniami | dedykowane konto (`www-data`), chroot/kontener, SELinux/AppArmor (domena `httpd_t`) |
| DoS (Slowloris, HTTP flood) | limity połączeń, timeouty, CDN, WAF, reverse proxy |
| brakujące nagłówki | **CSP, X-Content-Type-Options, X-Frame-Options, Referrer-Policy, Permissions-Policy** |
| podatne moduły/CMS | aktualizacje, SCA |

Przykład: konfiguracja serwera przez plik konfiguracyjny (np. **Nginx** – wykład W2, slajd 27: port, lokalizacja zasobów, autoryzacja).

## 5. FTP, TFTP i transfer plików

**FTP** (20/21): **hasła i dane jawnym tekstem**, osobny kanał danych (tryb aktywny/pasywny – kłopoty z zaporą), **anonimowy dostęp**, ataki **FTP bounce** (PORT), brute force. **Obrona:** **SFTP (SSH, port 22)** lub **FTPS (TLS)**; wyłączenie konta anonimowego, chroot użytkowników, najmniejsze uprawnienia, limity, logowanie, ograniczenie zakresu portów pasywnych. **TFTP** – bez uwierzytelniania, tylko w izolowanej sieci zarządzania (np. bootowanie sieciowe, konfiguracje urządzeń).

## 6. Pozostałe usługi sieci lokalnej

| Usługa | Zagrożenia | Zabezpieczenia |
| :--- | :--- | :--- |
| **SSH** | brute force, słabe klucze, root | klucze, `PermitRootLogin no`, fail2ban, ograniczenie `AllowUsers` |
| **SMB/CIFS** (wykład W1, slajdy 66–67: udziały, UNC, `\\serwer\udział`) | **SMBv1** (EternalBlue, WannaCry), brute force, NTLM relay, otwarte udziały | wyłączyć SMBv1, **SMB signing/szyfrowanie**, segmentacja, najmniejsze uprawnienia do udziałów, brak SMB w Internecie |
| **RDP** | brute force, podatności (BlueKeep), ransomware | NLA, MFA, **VPN/brama RDP**, blokada konta |
| **SMTP/IMAP/POP3** | open relay, spoofing nadawcy, phishing | TLS, uwierzytelnianie, **SPF/DKIM/DMARC**, filtry |
| **SNMP** | domyślne „community" (`public`), v1/v2c jawne | **SNMPv3** (uwierzytelnianie i szyfrowanie), ACL |
| **NTP** | amplifikacja (monlist), manipulacja czasem | aktualizacje, `restrict`, autoryzowane źródła (ważne dla Kerberos i logów) |
| **LDAP/Active Directory** | enumeracja, brak szyfrowania | LDAPS, najmniejsze uprawnienia, segmentacja kontrolerów |
| **Telnet** | jawne dane | wyłączyć |
| **Syslog** (514/UDP) | brak szyfrowania | TLS, centralny log server |
| **Bazy danych** (MySQL 3306, PostgreSQL 5432) | wystawienie do sieci, domyślne hasła | tylko wewnętrznie, TLS, ograniczone konta |
| **mDNS, LLMNR, NetBIOS** | poisoning, kradzież hashy | wyłączyć niepotrzebne |

## Wspólne zasady zabezpieczania usług

1. **Inwentaryzacja** – wiedza, co nasłuchuje (`ss -tulpn`, `Get-NetTCPConnection`, Nmap).
2. **Wyłączenie zbędnych** (wykład W8: minimalizacja; W2: *wyłączenie nieużywanych usług*) i niezabezpieczonych protokołów (Telnet, FTP, TFTP, SNMPv1/2c, SMBv1).
3. **Szyfrowanie i uwierzytelnianie** każdej usługi (TLS/SSH), silne hasła/klucze, MFA.
4. **Zapora hostowa** (UFW/iptables/Defender Firewall) + sieciowa, segmentacja, domyślna odmowa.
5. **Najmniejsze uprawnienia usługi** (dedykowane konto, MAC, kontener).
6. **Aktualizacje i bezpieczna konfiguracja** (CIS Benchmarks, brak domyślnych haseł).
7. **Logowanie i monitoring** (logi usług – `/var/log`, IDS, SIEM), alerty.
8. **Ochrona dostępności** (rate limiting, redundancja).
9. **Zabezpieczenia L2 dla DHCP/ARP** (snooping, DAI) i DNSSEC dla DNS.

## Podsumowanie

- **DHCP:** zagrożenia rogue DHCP i starvation → **DHCP snooping**, port security; **DNS:** poisoning, amplifikacja, tunelowanie → **DNSSEC, wyłączenie otwartej rekursji, filtrowanie, DoT/DoH**; **NAT:** nie jest zaporą – ryzyka port forwarding/UPnP → zapora stanowa; **HTTP:** HTTPS+HSTS, nagłówki, WAF, utwardzony serwer; **FTP:** jawny → **SFTP/FTPS**, Telnet → **SSH**, SNMPv3, wyłączenie SMBv1.
- Dla każdej usługi: minimalna ekspozycja, szyfrowanie, uwierzytelnianie, najmniejsze uprawnienia, aktualizacje, logowanie.
