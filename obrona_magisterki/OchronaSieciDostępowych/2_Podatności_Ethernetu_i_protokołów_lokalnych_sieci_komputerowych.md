# Podatności technologii Ethernet oraz protokołów wykorzystywanych w lokalnych sieciach komputerowych

> Temat nie jest pokryty dostarczonymi materiałami (poprzedni przedmiot dotykał ich jedynie w obszarze ataków L2/L3). Opracowanie z własnej wiedzy ***(uzupełnienie)***.

## Podatności samej technologii Ethernet

| Cecha / podatność | Opis | Skutek |
| :--- | :--- | :--- |
| **Brak uwierzytelniania i szyfrowania w L2** | ramka Ethernet nie ma mechanizmów uwierzytelniania nadawcy ani poufności | podsłuch, spoofing |
| **Adres MAC łatwy do sfałszowania** | MAC jest tylko etykietą ustawialną programowo | **MAC spoofing**, obejście filtrów MAC |
| **Medium współdzielone / rozgłoszeniowe** | w hubach i Wi-Fi każdy widzi ruch; domeny rozgłoszeniowe | sniffing, burze rozgłoszeniowe |
| **Tablica CAM przełącznika o skończonym rozmiarze** | po przepełnieniu przełącznik działa jak hub (*fail-open*) | **MAC flooding** → podsłuch |
| **Brak kontroli dostępu do portu domyślnie** | każde urządzenie podpięte do portu zyskuje dostęp | rogue devices |
| **Domyślne VLAN 1 / native VLAN** | współdzielone, niebezpieczne ustawienia domyślne | **VLAN hopping** |
| **Zaufanie w obrębie domeny rozgłoszeniowej** | brak ochrony przed ruchem bocznym w segmencie | ataki wewnętrzne |
| **Dostęp fizyczny** | gniazda, kable, porty SPAN | podpięcie, podsłuch |
| **Pętle L2** | bez STP burza rozgłoszeń | DoS |
| **Brak ochrony przed zakłóceniami i podsłuchem fizycznym** (kable miedziane, TAP) | podsłuch na kablu bez wykrycia | wyciek |

## Podatności protokołów LAN

### Warstwa 2

| Protokół | Słabość | Atak | Obrona |
| :--- | :--- | :--- | :--- |
| **ARP** | bezstanowy, brak uwierzytelniania; przyjmuje niezamawiane odpowiedzi | **ARP spoofing/poisoning** → MITM, DoS | **DAI**, statyczne wpisy, segmentacja |
| **STP/RSTP** | ufa każdemu BPDU | przejęcie roli root bridge, MITM, DoS | **BPDU Guard, Root Guard** |
| **VLAN / 802.1Q, DTP** | negocjacja trunku, native VLAN | **switch spoofing**, **double tagging** | `switchport mode access`, wyłączenie DTP, osobny native VLAN |
| **CDP/LLDP** | ujawnianie modelu, wersji, adresów | rozpoznanie (*reconnaissance*) | wyłączenie na portach użytkowników |
| **VTP** | rozgłaszanie konfiguracji VLAN | zmiana/usunięcie VLAN w sieci | tryb transparent, hasło VTP |
| **Port Mirroring (SPAN)** | kopia ruchu | nieuprawniony podsłuch | kontrola dostępu do zarządzania |

### Warstwa 3 i 4

| Protokół | Słabość | Atak |
| :--- | :--- | :--- |
| **DHCP** | brak uwierzytelniania serwera i klienta | **rogue DHCP** (fałszywa brama/DNS), **starvation** |
| **ICMP** | diagnostyka, redirect | flood, smurf, **ICMP redirect**, tunelowanie |
| **IP** | źródło łatwe do sfałszowania | **IP spoofing**, amplifikacja |
| **IPv6 (ND/RA)** | nieuwierzytelnione Router Advertisement | **rogue RA**, MITM w IPv6 (domyślnie włączone w systemach) |
| **RIP/OSPF/EIGRP** | domyślnie bez uwierzytelnienia | fałszywe trasy |
| **HSRP/VRRP** | domyślnie bez uwierzytelnienia | przejęcie roli bramy |
| **TCP** | uzgadnianie 3-etapowe, przewidywalne numery sekwencyjne | **SYN flood**, hijacking |
| **UDP** | bez połączenia | flood, amplifikacja (DNS, NTP, memcached) |

### Usługi lokalne i protokoły aplikacyjne

| Protokół | Podatność | Obrona |
| :--- | :--- | :--- |
| **DNS** | brak uwierzytelniania, cache poisoning, tunelowanie | DNSSEC, ograniczenie rekursji, filtrowanie |
| **NetBIOS/LLMNR/mDNS** | rozgłoszeniowe rozpoznawanie nazw – *poisoning* (Responder) i kradzież hashy NTLM | wyłączenie LLMNR/NBT-NS |
| **SMB (v1)** | EternalBlue (WannaCry), brak szyfrowania | wyłączenie SMBv1, SMB signing/szyfrowanie |
| **NTLM / SMB relay** | przekazywanie uwierzytelnienia | Kerberos, SMB signing, wyłączenie NTLM |
| **Telnet, FTP, HTTP, POP3/IMAP/SMTP bez TLS** | dane i hasła jawnym tekstem | SSH, SFTP/FTPS, HTTPS, TLS |
| **SNMP v1/v2c** | „community string" jawnie (np. `public`) | **SNMPv3** |
| **RDP** | brute force, podatności | NLA, MFA, VPN, ograniczenie ekspozycji |
| **TFTP, NTP bez ochrony** | brak uwierzytelniania, amplifikacja | ograniczenia, aktualizacje |
| **UPnP, WPS (w routerach)** | zdalne otwieranie portów, słaby WPS | wyłączenie |
| **802.1D / Wi-Fi WEP, WPA** | słabe szyfrowanie | WPA3 (temat 6) |

## Ogólne przyczyny podatności protokołów

1. **Historyczne założenie zaufania** (sieci zamknięte),
2. **brak uwierzytelniania i szyfrowania** w protokołach infrastrukturalnych,
3. **bezpieczne ustawienia nie są domyślne** (VLAN 1, DTP, CDP, STP bez ochrony, domyślne hasła),
4. **błędy konfiguracji** i niezałatane urządzenia,
5. **kompatybilność wsteczna** (SMBv1, SSLv3, TLS 1.0, SNMPv2c).

## Metody wykrywania podatności protokołów

- skanowanie sieci i usług (**nmap**, skanery podatności: Nessus, OpenVAS) – kurs Cisco, Moduł 2 i 4,
- analiza ruchu w **Wireshark** (jawne protokoły: Telnet vs SSH – laboratorium Cisco), przegląd konfiguracji przełączników,
- **audyt konfiguracji** względem CIS Benchmarks,
- monitoring przełączników (logi port security/DAI/DHCP snooping),
- testy penetracyjne (Yersinia, Ettercap, Responder – w kontrolowanych warunkach).

## Podsumowanie

- **Ethernet** nie zapewnia uwierzytelniania, poufności ani ochrony przed spoofingiem MAC i przepełnieniem CAM; środowisko LAN ufa urządzeniom w segmencie.
- **Protokoły LAN** (ARP, DHCP, STP, VLAN/DTP, CDP, ICMP, DNS, SMB, SNMP, Telnet/FTP/HTTP) mają znane słabości; źródłem jest **brak uwierzytelniania/szyfrowania** i **niebezpieczne ustawienia domyślne**.
- Obrona: **wyłączenie zbędnych protokołów**, mechanizmy przełączników (port security, DHCP snooping, DAI, BPDU Guard), **802.1X**, szyfrowane wersje protokołów (SSH, HTTPS, SNMPv3, SMB 3), segmentacja, audyt konfiguracji.

---
[⬅️ Poprzedni temat](1_Podstawowe_zagrożenia_w_lokalnych_sieciach_komputerowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](3_Znaczenie_monitorowania_sieci_lokalnej_w_wykrywaniu_i_analizie_incydentów.md)