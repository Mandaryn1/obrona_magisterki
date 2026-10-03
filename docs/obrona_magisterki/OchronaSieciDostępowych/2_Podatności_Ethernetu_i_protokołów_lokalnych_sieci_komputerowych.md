# Podatności technologii Ethernet oraz protokołów wykorzystywanych w lokalnych sieciach komputerowych

Podatności wynikają głównie z tego, że protokoły LAN powstały w czasach, gdy sieć uważano za zaufaną. Brakuje w nich uwierzytelniania i szyfrowania.

**Technologia Ethernet (L1/L2):**

- brak **uwierzytelniania i szyfrowania** ramek (można podsłuchiwać i podszywać się),
- **adres MAC łatwo podrobić** (MAC spoofing),
- ograniczona pamięć **tablicy CAM** przełącznika, co umożliwia **MAC flooding** (przełącznik zaczyna działać jak hub),
- domyślnie **brak kontroli dostępu do portu** (każdy podpięty host dostaje dostęp),
- ataki na **VLAN-y** (VLAN 1, VLAN hopping) oraz **pętle L2**,
- ruch w medium współdzielonym (Wi-Fi) łatwo przechwycić.

**Protokoły L2 i pomocnicze:**

- **ARP:** brak uwierzytelniania, więc możliwe **ARP spoofing/poisoning** i MITM,
- **STP:** fałszywe BPDU pozwala przejąć rolę root bridge,
- **DTP, CDP, VTP:** ujawniają informacje lub umożliwiają zmianę konfiguracji.

**Protokoły L3/L4:**

- **DHCP:** brak uwierzytelniania, możliwy **fałszywy serwer** i starvation,
- **IP:** łatwy **spoofing** adresu źródłowego,
- **ICMP:** flood, smurf, przekierowania, tunelowanie,
- **IPv6:** fałszywe Router Advertisement i Neighbor Discovery,
- **protokoły routingu** (RIP, OSPF) i **HSRP:** bez uwierzytelniania można wstrzykiwać fałszywe trasy,
- **TCP/UDP:** SYN flood, przejęcie sesji, amplifikacja w UDP.

**Usługi i protokoły aplikacyjne:**

- **Telnet, FTP, HTTP:** dane i hasła jawnym tekstem,
- **SMBv1:** podatność EternalBlue (WannaCry), **NTLM relay**,
- **SNMP v1/v2c:** jawne i domyślne community,
- **DNS:** cache poisoning i amplifikacja,
- **LLMNR/NBNS:** podszywanie i kradzież skrótów haseł,
- **RDP:** brute force.

**Obrona:** port security, DAI, DHCP snooping, BPDU Guard, **802.1X**, wyłączenie DTP, szyfrowanie (SSH, HTTPS, SNMPv3, MACsec), uwierzytelnianie protokołów routingu, ACL i uRPF, segmentacja VLAN, wyłączenie nieużywanych usług i protokołów.

### Ogólne przyczyny podatności protokołów

1. **Historyczne założenie zaufania** (sieci zamknięte),
2. **brak uwierzytelniania i szyfrowania** w protokołach infrastrukturalnych,
3. **bezpieczne ustawienia nie są domyślne** (VLAN 1, DTP, CDP, STP bez ochrony, domyślne hasła),
4. **błędy konfiguracji** i niezałatane urządzenia,
5. **kompatybilność wsteczna** (SMBv1, SSLv3, TLS 1.0, SNMPv2c).

### Metody wykrywania podatności protokołów

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