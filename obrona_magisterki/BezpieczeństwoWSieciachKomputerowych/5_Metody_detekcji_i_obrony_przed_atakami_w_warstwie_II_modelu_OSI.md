# Metody detekcji i obrony przed atakami w warstwie II modelu OSI

> **Uwaga:** dostarczone materiały wspominają tylko o ARP poisoning, fałszywych serwerach DHCP i przełącznikach SPAN/TAP (Zapory/IDS, slajdy 11, 30; W1, slajd 34). Poniższe opracowanie pochodzi głównie z własnej wiedzy ***(uzupełnienie)***.

## Specyfika warstwy 2

Warstwa łącza danych (Ethernet, przełączniki, ARP, VLAN, STP) zakłada **zaufanie w obrębie segmentu (domeny rozgłoszeniowej)**. Napastnik, który ma dostęp do portu przełącznika (kabel lub Wi-Fi), może atakować sąsiednie hosty **bez przechodzenia przez zaporę**. Zabezpieczenia L3/L4 (zapory) tych ataków nie widzą.

## Ataki L2, ich wykrywanie i obrona

### 1. MAC flooding (przepełnienie tablicy CAM)

- **Atak:** napastnik zalewa przełącznik ramkami z losowymi źródłowymi adresami MAC; po przepełnieniu tablicy CAM przełącznik działa jak hub (**fail-open**) i rozsyła ruch na wszystkie porty → **podsłuch** (macof, Yersinia).
- **Wykrywanie:** gwałtowny wzrost liczby adresów MAC na porcie; logi przełącznika (alerty port security); IDS.
- **Obrona:** **Port Security** (limit liczby MAC na port, tryb sticky, reakcja na naruszenie: *protect/restrict/shutdown*), storm control.

### 2. ARP spoofing / poisoning (MITM)

- **Atak:** ARP nie ma uwierzytelniania, więc napastnik wysyła fałszywe (także niezamawiane, *gratuitous*) odpowiedzi ARP, wiążąc **swój MAC z adresem IP bramy/ofiary** → ruch przechodzi przez niego (Ettercap, Bettercap, arpspoof). Skutki: podsłuch, modyfikacja, DoS, kradzież sesji.
- **Wykrywanie:** **ten sam MAC dla wielu adresów IP** w tablicy ARP (`arp -a`), zmiana MAC bramy, **duplikaty adresów IP**; **arpwatch**, XArp; Wireshark: `arp.duplicate-address-detected`, duża liczba odpowiedzi ARP bez zapytań; IDS (Snort/Suricata) z regułami ARP.
- **Obrona:** **DAI – Dynamic ARP Inspection** (weryfikacja pakietów ARP względem tabeli powiązań DHCP snooping), statyczne wpisy ARP dla krytycznych hostów (brama), segmentacja VLAN, szyfrowanie (TLS/SSH ogranicza skutki), 802.1X.

### 3. Fałszywy serwer DHCP (rogue DHCP) i DHCP starvation

- **Atak:** (a) **rogue DHCP** – napastnik oferuje klientom własną bramę i DNS (MITM, przekierowanie); (b) **starvation** – wyczerpanie puli adresów żądaniami z fałszywych MAC (potem rogue DHCP przejmuje klientów).
- **Wykrywanie:** nieznany serwer DHCP (Wireshark: `dhcp`/`bootp` Offer od nieznanego adresu); logi przełącznika; skanery wykrywające rogue DHCP.
- **Obrona:** **DHCP snooping** – porty **zaufane** (trusted: uplink, serwer DHCP) i **niezaufane** (klienci); blokada odpowiedzi DHCP z portów niezaufanych; budowa **tabeli powiązań** (MAC–IP–port–VLAN); limit żądań DHCP na port; port security.

### 4. Ataki na STP (Spanning Tree Protocol)

- **Atak:** napastnik wysyła BPDU z niższym priorytetem i **przejmuje rolę root bridge** → zmiana topologii, MITM, DoS (przekierowanie ruchu przez siebie).
- **Wykrywanie:** zmiana root bridge, nietypowe BPDU na portach dostępowych; logi.
- **Obrona:** **BPDU Guard** (wyłączenie portu brzegowego po otrzymaniu BPDU), **Root Guard** (zapobiega zmianie roota), PortFast tylko na portach brzegowych, jawne ustawienie priorytetu root, wyłączenie STP na portach klienckich tam, gdzie to bezpieczne.

### 5. VLAN hopping

- **Atak:** (a) **switch spoofing** – napastnik wymusza trunk przez DTP; (b) **double tagging** – podwójne znakowanie 802.1Q wykorzystujące native VLAN; dostęp do innych VLAN.
- **Wykrywanie:** nieoczekiwane trunki na portach dostępowych, ramki z podwójnym tagiem (Wireshark).
- **Obrona:** porty dostępowe jako `switchport mode access`, **wyłączenie DTP** (`switchport nonegotiate`), **niewykorzystywany native VLAN** (np. 999), nie używać VLAN 1 dla użytkowników, wyłączenie nieużywanych portów i przypisanie ich do VLAN „parking".

### 6. MAC spoofing / podszywanie

- **Atak:** zmiana adresu MAC (obejście filtrów MAC, przejęcie tożsamości).
- **Obrona:** **802.1X** (uwierzytelnianie urządzenia/użytkownika zamiast ufania MAC), port security + sticky, NAC; monitorowanie kolizji MAC.

### 7. Ujawnianie informacji (CDP/LLDP) i atak na protokoły zarządzania

- **Atak:** CDP/LLDP ujawnia model, wersję IOS, adresację → rozpoznanie.
- **Obrona:** wyłączenie CDP/LLDP na portach użytkowników, bezpieczne zarządzanie (SSH, VLAN zarządzający).

### 8. Ataki Wi-Fi (L2 w sieciach bezprzewodowych)

- **Rogue AP / evil twin**, deauth flood, przechwytywanie handshake'ów → **WPA2/WPA3-Enterprise (802.1X)**, WIDS/WIPS, ochrona ramek zarządzających (802.11w).

## Mechanizmy obronne – zestawienie

| Mechanizm | Chroni przed | Opis |
| :--- | :--- | :--- |
| **Port Security** | MAC flooding, podpinanie obcych urządzeń | limit MAC, sticky, tryb naruszenia |
| **DHCP Snooping** | rogue DHCP, starvation | porty trusted/untrusted, tabela powiązań |
| **DAI (Dynamic ARP Inspection)** | ARP spoofing | weryfikacja ARP względem tabeli snooping |
| **IP Source Guard** | IP spoofing w L2 | filtr ruchu wg powiązań MAC–IP |
| **BPDU Guard / Root Guard** | ataki STP | ochrona portów i topologii |
| **802.1X / NAC** | nieautoryzowane urządzenia, MAC spoofing | uwierzytelnianie przed dostępem do sieci |
| **VLAN i hardening trunków** | VLAN hopping | access/trunk jawnie, native VLAN, brak DTP |
| **Storm control** | burze rozgłoszeń | limity broadcast/multicast/unicast |
| **Private VLAN, segmentacja** | ruch boczny | izolacja hostów w VLAN |
| **Wyłączenie nieużywanych portów** | podpięcie się do sieci | shutdown + VLAN parking |
| **Ochrona fizyczna** | podpięcie do gniazd | zamknięte szafy, gniazda nieużywane wyłączone |

### Przykład konfiguracji (Cisco IOS) *(uzupełnienie)*

```text
! Port Security na porcie dostępowym
interface GigabitEthernet0/5
 switchport mode access
 switchport access vlan 10
 switchport nonegotiate
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation shutdown
 spanning-tree portfast
 spanning-tree bpduguard enable
!
! DHCP snooping + DAI + IP Source Guard
ip dhcp snooping
ip dhcp snooping vlan 10
ip arp inspection vlan 10
interface GigabitEthernet0/24      ! uplink / serwer DHCP
 ip dhcp snooping trust
 ip arp inspection trust
interface GigabitEthernet0/5
 ip verify source
!
! Trunk – native VLAN niewykorzystywany
interface GigabitEthernet0/23
 switchport mode trunk
 switchport trunk native vlan 999
 switchport nonegotiate
!
storm-control broadcast level 1.00
```

## Detekcja – podsumowanie narzędzi i sygnałów

| Źródło | Co obserwować |
| :--- | :--- |
| **Przełącznik (syslog/SNMP)** | naruszenia port security, zmiany STP, odrzucone pakiety DAI/DHCP snooping |
| **SPAN/TAP + NIDS** (Snort/Suricata, Zeek – wykład, slajdy 11, 26) | anomalie ARP, nowe serwery DHCP, ramki z podwójnym tagiem |
| **arpwatch / XArp** | zmiany powiązań IP–MAC |
| **Wireshark/tcpdump** | `arp.duplicate-address-detected`, `dhcp`, `stp`, `vlan` |
| **NAC / 802.1X (RADIUS)** | nieznane urządzenia, próby logowania |
| **SIEM** | korelacja logów przełączników z innymi zdarzeniami |

## Podsumowanie

- Ataki L2 (**MAC flooding, ARP spoofing, rogue DHCP/starvation, STP, VLAN hopping, MAC spoofing**) wykorzystują zaufanie w segmencie i omijają zapory.
- Obrona to **mechanizmy przełączników**: **Port Security, DHCP snooping, DAI, IP Source Guard, BPDU/Root Guard**, poprawna konfiguracja VLAN/trunków, **802.1X/NAC**, storm control, ochrona fizyczna.
- Detekcja: logi przełączników, **arpwatch**, analiza ruchu (Wireshark, NIDS na SPAN/TAP), SIEM.

---
[⬅️ Poprzedni temat](4_Klasyfikacja_najważniejszych_rodzajów_ataków_sieciowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_Metody_detekcji_i_obrony_przed_atakami_w_warstwie_III_modelu_OSI.md)