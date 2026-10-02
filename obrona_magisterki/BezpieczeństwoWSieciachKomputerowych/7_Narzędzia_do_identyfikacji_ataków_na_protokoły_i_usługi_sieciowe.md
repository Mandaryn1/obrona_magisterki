# Narzędzia służące do identyfikacji ataków na protokoły i usługi sieciowe

> Opracowanie oparte głównie na wykładzie *Zapory sieciowe i systemy wykrywania włamań* (slajdy 10–19, 26, 33); uzupełnienia oznaczone ***(uzupełnienie)***.

## Podział narzędzi

| Grupa | Zadanie | Przykłady |
| :--- | :--- | :--- |
| **Analizatory pakietów** | przechwytywanie i dekodowanie ruchu | **Wireshark, tcpdump**, tshark |
| **IDS/IPS sieciowe (NIDS/NIPS)** | wykrywanie/blokowanie ataków w ruchu | **Snort, Suricata**, Cisco Firepower |
| **Monitory bezpieczeństwa sieci (NSM)** | metadane i logi połączeń, threat hunting | **Zeek (Bro)**, Security Onion |
| **IDS/IPS hostowe (HIDS/HIPS)** | integralność plików, logi, procesy | **OSSEC / Wazuh**, Sysmon, auditd |
| **Analiza przepływów** | statystyka ruchu, anomalie | **NetFlow, sFlow, IPFIX** (ntop, Plixer Scrutinizer, SolarWinds NTA) |
| **Zapory i WAF** | blokowanie i logi | iptables/nftables, pfSense, Cisco ASA, **ModSecurity**, NGFW (Palo Alto, FortiGate) |
| **SIEM / SOAR** | korelacja logów, alarmy, automatyzacja | Splunk, ELK/Elastic, Wazuh, QRadar, Sentinel |
| **Skanery portów i podatności** | rozpoznanie, ocena | **nmap, masscan**, Nessus, OpenVAS, Nikto |
| **Narzędzia wykrywania spoofingu L2** *(uzup.)* | ARP/DHCP | arpwatch, XArp, DHCP snooping (przełącznik) |
| **Narzędzia ofensywne (testy)** *(uzup.)* | symulacja ataków | Metasploit, hping3, Ettercap, Yersinia, Caldera |
| **Analiza malware** | badanie próbek | Cuckoo, Any.Run, Ghidra, Volatility (wykład, slajd 22) |

## Analizatory pakietów

### Wireshark

- GUI, dekodowanie **setek protokołów**, filtry **przechwytywania** (BPF) i **wyświetlania**, śledzenie strumieni TCP (*Follow TCP Stream*), statystyki (Conversations, I/O Graph, Expert Info).
- **Wykrywanie ataków:** duplikaty ARP, wiele SYN bez ACK (SYN flood/skanowanie), podejrzane odpowiedzi DNS, ruch jawny (Telnet, FTP, HTTP), anomalie ICMP.
- Przydatne filtry: `arp.duplicate-address-detected`, `tcp.flags.syn==1 && tcp.flags.ack==0`, `dns.flags.response==1`, `http.request.method=="POST"`, `icmp`, `dhcp`.

### tcpdump

- CLI (Unix/Linux; biblioteka **libpcap**), idealny do zdalnej diagnostyki przez SSH; wynik zapisywany do `.pcap` i analizowany w Wireshark.
- Przykłady: `tcpdump -i eth0 -nn 'tcp[tcpflags] & tcp-syn != 0'`, `tcpdump -w capture.pcap host 10.0.0.5 and port 443`.

## Systemy IDS/IPS

### Rodzaje (wykład, slajdy 11, 15)

| | **NIDS / NIPS** | **HIDS / HIPS** |
| :--- | :--- | :--- |
| Miejsce | kluczowe segmenty; IDS na **SPAN/TAP** (kopia ruchu); IPS **inline** | agent na hoście |
| Widzi | ruch sieciowy | logi, integralność plików, procesy, pamięć |
| Przykłady | **Snort, Suricata**, Cisco Firepower | **OSSEC, Wazuh**, Trend Micro Deep Security, McAfee HIPS |
| Uwagi | IPS: możliwy SPOF | nie widzi ruchu innych hostów; słabo z szyfrowanym ruchem sieciowym |

### Metody wykrywania (wykład, slajd 12)

| | **Sygnaturowe** | **Anomalii** | **Hybrydowe** |
| :--- | :--- | :--- | :--- |
| Zasada | porównanie z bazą znanych wzorców | odchylenia od profilu normalnego ruchu (ML, statystyka) | połączenie obu |
| Zalety | wysoka precyzja, mało fałszywych alarmów | wykrywa **nowe/zero-day** | optymalne pokrycie |
| Wady | **bezradne wobec zero-day**, wymaga aktualizacji sygnatur | więcej **fałszywych alarmów**, okres uczenia | złożoność |

**Zasada:** „*IDS wykrywa – IPS reaguje*" (wykład, slajd 14): wykrycie → analiza → blokowanie → rejestracja w logach.

### Snort / Suricata (wykład, slajd 26)

- **Snort** – najpopularniejszy open-source NIDS/NIPS; wykrywanie sygnaturowe z językiem reguł, preprocesory normalizujące protokoły, tryby IDS i inline IPS; reguły Community i VRT.
- **Suricata** – nowoczesny IDS/IPS, **wielowątkowy**, identyfikacja protokołów, inspekcja TLS, skrypty Lua, **zgodny z regułami Snorta**.
- **Zeek (Bro)** – monitor bezpieczeństwa z językiem skryptowym; logi metadanych: `conn.log`, `http.log`, `dns.log`; threat hunting i forensics, integracja z SIEM.

Przykładowa reguła Snort *(uzupełnienie)* – próby brute force SSH:

```text
alert tcp any any -> $HOME_NET 22 (msg:"Possible SSH brute force"; flags:S;
  detection_filter:track by_src, count 5, seconds 60; sid:1000010; rev:1;)
```

### Co wykrywają IDS/IPS (wykład, slajd 17)

| Atak | Sposób wykrycia |
| :--- | :--- |
| **Brute force** | wiele nieudanych logowań z jednego IP; IPS blokuje po przekroczeniu progu |
| **Skanowanie portów** (nmap, masscan) | charakterystyczne wzorce skanowania (wiele portów/SYN) |
| **Exploity znanych luk** (EternalBlue MS17-010, Shellshock, Heartbleed) | sygnatury payloadów |
| **DDoS** | anomalie wolumenu i rodzaju ruchu, rate limiting |
| **SQL Injection** | WAF/IPS – wzorce w zapytaniach |
| **Command & Control (C2)** | podejrzane domeny i wzorce komunikacji botnetów |

## Analiza przepływów i metadanych

**NetFlow / sFlow** (wykład, slajd 33) – eksport metadanych o ruchu z routerów i przełączników (źródło/cel, porty, bajty, czas) – bez zawartości pakietów. Zastosowanie: wykrywanie anomalii (DDoS, skanowanie, eksfiltracja), planowanie pojemności, rozliczenia. Skalowalne dla dużych sieci i szyfrowanego ruchu.

## Zapory i WAF jako źródło wykryć

- Logi zapór (odrzucone/dopuszczone połączenia, skanowanie), **zapory stanowe** (iptables+conntrack), **NGFW** z DPI i IPS (wykład, slajd 9),
- **WAF** (ModSecurity, F5 ASM, Cloudflare WAF): SQLi, XSS, command injection, path traversal (wykład, slajdy 8, 31).

## SIEM, SOC, SOAR

- **SIEM** – centralizacja logów i korelacja zdarzeń (wykład, slajd 16); **SOAR** – automatyzacja reakcji; **EDR/XDR** – ochrona punktów końcowych; **Threat Intelligence** – aktualizacja sygnatur i IoC.
- Zespół **SOC** (24/7): triage alertów, weryfikacja fałszywych alarmów, reagowanie wg playbooków (wykład, slajd 28).

## Skanery i narzędzia oceny

- **nmap/masscan** – wykrywanie hostów/usług (obrona: inwentaryzacja; wykrywane przez IDS jako atak rozpoznawczy),
- **Nessus/OpenVAS** – podatności; **Nikto** – serwery WWW; **testssl.sh** – konfiguracja TLS,
- **Metasploit, Caldera, red/purple team** – sprawdzanie skuteczności detekcji (wykład, slajd 19).

## Ograniczenia narzędzi

| Problem | Opis | Rozwiązanie |
| :--- | :--- | :--- |
| **Szyfrowanie** (TLS 1.3, >90% ruchu) | IDS/IPS nie widzą treści (wykład, slajd 27) | inspekcja TLS (MITM proxy, CA), **JA3/metadane**, analiza na hoście |
| **Fałszywe alarmy / alert fatigue** | tysiące alertów dziennie (wykład, slajd 29) | strojenie reguł, whitelisting, SOAR, priorytetyzacja AI |
| **Wydajność** | >10 Gb/s wymaga sprzętu/klastrowania | load balancing przed sensorami, FPGA/ASIC |
| **SPOF (IPS inline)** | awaria zatrzymuje ruch | bypass, redundancja |
| **Zero-day** | brak sygnatur | detekcja anomalii, sandboxing |
| **Ominięcie przez fragmentację/obfuskację** | | normalizacja protokołów |

## Dobre praktyki (wykład, slajdy 18–19)

analiza wymagań → dobór rozwiązania → projekt architektury (punkty monitorowania, HA) → konfiguracja reguł → strojenie → monitoring przez SOC; ciągłe dostrajanie, integracja z SIEM/SOAR/threat intelligence, szkolenia (MITRE ATT&CK), okresowe testy penetracyjne i symulacje (red/purple team).

## Podsumowanie

- Do identyfikacji ataków służą: **analizatory pakietów** (Wireshark, tcpdump), **NIDS/NIPS** (Snort, Suricata), **NSM** (Zeek), **HIDS/HIPS** (OSSEC/Wazuh), **analiza przepływów** (NetFlow/sFlow), **zapory/WAF**, **SIEM/SOAR**, skanery.
- Metody detekcji: **sygnaturowa, anomalii, hybrydowa**; **IDS wykrywa, IPS blokuje**.
- Główne wyzwania: szyfrowanie, fałszywe alarmy, wydajność, zero-day.

---
[⬅️ Poprzedni temat](6_Metody_detekcji_i_obrony_przed_atakami_w_warstwie_III_modelu_OSI.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Monitorowanie_ruchu_sieciowego_w_wykrywaniu_nieprawidłowości_i_incydentów.md)