# Narzędzia wykorzystywane do monitorowania i analizy ruchu w lokalnych sieciach komputerowych

> Opracowanie oparte na kursie Cisco (Moduł 2: narzędzia, Nmap/Zenmap, SuperScan, sniffery, SIEM; Moduł 4: NetFlow, Wireshark) i wykładzie IoT (Fing, GlassWire); pozostałe – ***(uzupełnienie)***.

## Podział narzędzi

| Kategoria | Zadanie | Przykłady |
| :--- | :--- | :--- |
| **Analizatory pakietów (sniffery)** | przechwytywanie i dekodowanie ruchu | Wireshark, tcpdump, tshark |
| **Skanery sieci i portów** | odkrywanie hostów, portów, usług, systemów | **Nmap/Zenmap**, **SuperScan**, masscan, hping, netcat |
| **Analiza przepływów** | statystyka ruchu, wykrywanie anomalii | **NetFlow, sFlow, IPFIX** (ntop, Scrutinizer) |
| **IDS/IPS i NSM** | wykrywanie ataków | Snort, Suricata, Zeek |
| **SIEM / SOAR** | agregacja, korelacja, automatyzacja | Splunk, QRadar, Sentinel, ELK, Wazuh |
| **Skanery podatności** | podatności, konfiguracje | Nessus, OpenVAS, Retina, Core Impact, GFI LANguard, Qualys |
| **Kontrola integralności** | wykrywanie zmian | Tripwire, OSSEC |
| **Systemy zarządzania siecią** *(uzup.)* | dostępność, obciążenie | SNMP: Zabbix, Nagios, PRTG, LibreNMS |
| **Kontrola dostępu / profilowanie** | kto jest w sieci | NAC, **Cisco ISE**, 802.1X |
| **Narzędzia dla użytkowników domowych/SOHO** (wykład IoT) | lista urządzeń, alerty | **Fing**, **GlassWire** |
| **Narzędzia CLI** | diagnostyka | ipconfig/ifconfig, ping, arp, tracert, nslookup, netstat, nbtstat |

## 1. Analizatory pakietów (sniffery) – kurs Cisco

- **Analiza problemów sieciowych**, **wykrywanie prób włamań**, **izolacja wykorzystanych systemów**, **rejestrowanie ruchu**, **wykrywanie nadużyć**.
- Sniffing = badanie całego ruchu przechodzącego przez kartę sieciową, niezależnie od adresata; może dotyczyć całego ruchu lub konkretnego protokołu/usługi/ciągu (np. login i hasło). Wykorzystują go **administratorzy** (przepustowość, problemy) i **przestępcy**. **Bezpieczeństwo fizyczne** zapobiega wprowadzaniu snifferów.
- **Laboratorium Cisco:** *Użyj Wiresharka do porównania ruchu Telnet i SSH* – Telnet jawnie widoczny, SSH zaszyfrowany.
- **Wireshark** – GUI, dekodowanie setek protokołów, filtry przechwytywania/wyświetlania, *Follow Stream*, statystyki. **tcpdump** – CLI (libpcap), zdalna diagnostyka przez SSH, zapis do `.pcap`.
- W sieci przełączanej wymaga **SPAN/mirror** lub **TAP**.

## 2. Nmap / Zenmap

**Nmap** – publicznie dostępny skaner niskiego poziomu do **mapowania sieci i rekonesansu**; **Zenmap** – wersja graficzna. Funkcje (kurs Cisco):

- **klasyczne skanowanie portów TCP/UDP** (wiele usług na jednym hoście),
- **przeczesywanie portów** (ta sama usługa na wielu hostach),
- **ukryte skanowanie** TCP/UDP (trudniejsze do wykrycia przez host lub IPS),
- **zdalna identyfikacja systemu operacyjnego** (*OS fingerprinting*),
- **skanowanie protokołów** (warstwa 3: np. GRE, OSPF),
- użycie **hostów wabików** do zamaskowania źródła skanu; działa na UNIX/Linux/Windows/OS X; nie ma funkcji warstwy aplikacji (w sensie klasycznego skanera podatności).

Typowe polecenia: `nmap -sn 10.0.0.0/24` (kto żyje), `nmap -sS -p- host`, `-sV`, `-O`, `-A`, `--script vuln`.

## 3. SuperScan

Skaner portów dla Windows (wymaga uprawnień administratora). Wykrywa otwarte porty **TCP/UDP**, usługi, ma **regulowaną szybkość**, nieograniczone zakresy IP, **TCP SYN i UDP**, wykrywanie hostów wieloma metodami ICMP, **przechwytywanie banerów**, wyliczanie hostów Windows, raporty HTML, narzędzia ping/traceroute/whois. Kurs zauważa, że lista narzędzi jest częściowo przestarzała – ma dać wiedzę o **rodzajach** narzędzi.

## 4. Narzędzia diagnostyczne wiersza poleceń (kurs Cisco, Moduł 2)

| Polecenie | Funkcja |
| :--- | :--- |
| `ipconfig` / `ifconfig` | ustawienia TCP/IP (IP, maska, brama, DNS, MAC) |
| `ping` | łączność (ICMP) |
| `arp` | tablica MAC–IP; szybkie znalezienie MAC |
| `tracert` / `traceroute` | trasa pakietu, miejsce zawieszenia |
| `nslookup` / `dig` | zapytania DNS |
| `netstat` | porty nasłuchujące i aktywne połączenia |
| `nbtstat` | rozwiązywanie nazw NetBIOS (Windows) |
| `nmap` | audyt: hosty, systemy, usługi |
| `netcat` | połączenia TCP/UDP: skanowanie portów, monitorowanie, **przechwytywanie banerów**, kopiowanie plików |
| `hping` | analiza pakietów: skanowanie portów, wykrywanie ścieżek, fingerprinting OS, **testowanie zapór** |

## 5. Analiza przepływów – NetFlow, sFlow, IPFIX

- Eksport **metadanych** z routerów i przełączników (źródło, cel, porty, protokół, bajty, czas) – bez treści pakietów.
- Kurs Cisco (Moduł 4): **NetFlow i Wireshark** służą do **scharakteryzowania normalnej charakterystyki ruchu** (baseline).
- Zastosowania: **wykrywanie anomalii** (DDoS, skanowanie, eksfiltracja), planowanie pojemności, rozliczenia; skalowalne, działa też dla ruchu szyfrowanego.

## 6. IDS/IPS i monitory bezpieczeństwa sieci

- **NIDS** na SPAN/TAP, **NIPS** inline; **HIDS/HIPS** na hostach (*uzupełnienie – zob. poprzedni przedmiot*).
- **Snort/Suricata** – sygnatury; **Zeek** – metadane (`conn.log`, `dns.log`, `http.log`), threat hunting; **OSSEC/Wazuh** – integralność plików, logi.
- Metody: **sygnaturowa**, **anomalii**, **hybrydowa**.

## 7. SIEM, SOAR i Cisco ISE

SIEM: korelacja, agregacja, forensics, retencja (zob. temat 3). **Cisco ISE** (kurs Cisco): system profilowania użytkowników/urządzeń i kontroli dostępu (AAA, 802.1X, postura) – dostarcza SIEM-owi dane o użytkowniku, urządzeniu i zgodności.

## 8. Narzędzia kontroli integralności i konfiguracji (kurs Cisco, Moduł 2)

- **Kontrolery integralności** wykrywają i raportują zmiany w systemie (głównie plików; niektóre – logowania/wylogowania); **Tripwire** ocenia i weryfikuje konfiguracje względem polityk, standardów zgodności i dobrych praktyk.
- **L0phtCrack** – audyt i odzyskiwanie haseł (wykrywanie słabych haseł); **Metasploit** – informacje o podatnościach, testy penetracyjne, tworzenie sygnatur IDS.

## 9. Narzędzia „codzienne" dla małych sieci i IoT (wykład IoT)

**Fing** (lista urządzeń, powiadomienia o nowych), **GlassWire** (ruch aplikacji), Wireshark, dedykowane systemy monitoringu IoT; przegląd listy urządzeń w routerze; alerty o nowych urządzeniach.

## Dobór narzędzia do zadania

| Zadanie | Narzędzie |
| :--- | :--- |
| „Co płynie w sieci i czy jest szyfrowane?" | Wireshark, tcpdump |
| „Jakie hosty/usługi istnieją?" | Nmap, SuperScan |
| „Czy ruch odbiega od normy?" | NetFlow + baseline, NBA |
| „Czy ktoś nas atakuje?" | Snort/Suricata/Zeek |
| „Co się stało w ostatniej godzinie w całej firmie?" | SIEM |
| „Jakie podatności mamy?" | Nessus, OpenVAS |
| „Czy ktoś zmienił pliki/konfigurację?" | Tripwire, OSSEC |
| „Szybka diagnoza problemu" | ping, tracert, netstat, arp, nslookup |

## Zasady stosowania

- **zgoda właściciela** sieci (skanowanie, sniffing mogą być nielegalne bez autoryzacji),
- narzędzia są **neutralne** – atakujący używają tych samych (Nmap, sniffery, Metasploit); obrońca musi znać ich działanie i ślady w logach,
- uwaga na skanowanie **inwazyjne** (może uszkodzić cel),
- ochrona danych z przechwytywania (zawierają hasła i dane osobowe).

## Podsumowanie

- Podstawowy zestaw: **sniffery** (Wireshark, tcpdump), **skanery** (Nmap/Zenmap, SuperScan), **analiza przepływów** (NetFlow/sFlow), **IDS/IPS i NSM**, **SIEM/SOAR**, **skanery podatności**, **kontrola integralności** (Tripwire), narzędzia **CLI** (ping, arp, netstat, tracert, nslookup, netcat, hping).
- Dobór zależy od pytania (zawartość ruchu, inwentaryzacja, anomalie, ataki, korelacja, podatności).
- Stosować legalnie i etycznie; łączyć narzędzia w ramach monitoringu i procesu reagowania.

---
[⬅️ Poprzedni temat](3_Znaczenie_monitorowania_sieci_lokalnej_w_wykrywaniu_i_analizie_incydentów.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Zasady_projektowania_bezpiecznej_infrastruktury_sieci_lokalnej_i_zabezpieczanie_urządzeń.md)