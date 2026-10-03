# Narzędzia wykorzystywane do monitorowania i analizy ruchu w lokalnych sieciach komputerowych

Narzędzia do monitorowania i analizy ruchu w LAN można podzielić według tego, co robią.

**1. Sniffery (analiza pakietów):**

- **Wireshark** (analiza z interfejsem graficznym) i **tcpdump** (wiersz poleceń) przechwytują ruch i dekodują protokoły.
- **Zastosowania:** diagnostyka, forensics, wykrywanie ataków L2 i niezaszyfrowanych danych, porównanie np. Telnetu (czytelny) i SSH (zaszyfrowany).
- Wymagają dostępu do ruchu: **port SPAN lub TAP** na przełączniku.

**2. Skanery sieci:**

- **Nmap/Zenmap** wykrywają hosty, porty, usługi, wersje i system operacyjny (OS fingerprinting), także w warstwie 3. Podobnie **SuperScan** w Windows.
- Służą do inwentaryzacji i audytu własnej sieci.

**3. Narzędzia wiersza poleceń:**

- `ipconfig`/`ip`, `ping`, `arp`, `tracert`/`traceroute`, `nslookup`, `netstat`/`ss`, `nbtstat`, netcat, hping.
- Szybka diagnostyka łączności, tablicy ARP, DNS i otwartych połączeń.

**4. Monitoring przepływów:**

- **NetFlow/sFlow/IPFIX** zbierają **metadane przepływów** (kto z kim, ile danych). Dobrze skalują się i działają także przy ruchu szyfrowanym. Pomagają wykrywać anomalie i DDoS.

**5. IDS/IPS i NSM (Network Security Monitoring):**

- **Snort i Suricata** (wykrywanie i blokowanie ataków) oraz **Zeek** (logi metadanych, np. `conn.log` i `dns.log`).
- **HIDS:** OSSEC/Wazuh na hostach.

**6. SIEM i SOAR:**

- Gromadzą logi z wielu źródeł, **korelują zdarzenia**, generują alerty i przechowują dane do analizy powłamaniowej. SOAR automatyzuje reakcję.

**7. Skanery podatności i narzędzia uzupełniające:**

- **Nessus, Retina, GFI LANguard** (wykrywanie podatności),
- **Tripwire** (kontrola integralności plików), **Metasploit** (testy penetracyjne),
- **Fing**, **GlassWire** (proste narzędzia do wykrywania urządzeń i ruchu).

**Wniosek:** każde narzędzie pokazuje inny wycinek. Sniffer daje szczegóły pakietów, NetFlow ogólny obraz ruchu, IDS wykrywa ataki, a SIEM łączy dane w całość. Używa się ich w ramach polityki bezpieczeństwa, za zgodą administratora sieci.

| Kategoria | Zadanie | Przykłady |
| :--- | :--- | :--- |
| **Analizatory pakietów (sniffery)** | przechwytywanie i dekodowanie ruchu | Wireshark, tcpdump, tshark |
| **Skanery sieci i portów** | odkrywanie hostów, portów, usług, systemów | **Nmap/Zenmap**, **SuperScan**, masscan, hping, netcat |
| **Analiza przepływów** | statystyka ruchu, wykrywanie anomalii | **NetFlow, sFlow, IPFIX** (ntop, Scrutinizer) |
| **IDS/IPS i NSM** | wykrywanie ataków | Snort, Suricata, Zeek |
| **SIEM / SOAR** | agregacja, korelacja, automatyzacja | Splunk, QRadar, Sentinel, ELK, Wazuh |
| **Skanery podatności** | podatności, konfiguracje | Nessus, OpenVAS, Retina, Core Impact, GFI LANguard, Qualys |
| **Kontrola integralności** | wykrywanie zmian | Tripwire, OSSEC |
| **Systemy zarządzania siecią** | dostępność, obciążenie | SNMP: Zabbix, Nagios, PRTG, LibreNMS |
| **Kontrola dostępu / profilowanie** | kto jest w sieci | NAC, **Cisco ISE**, 802.1X |
| **Narzędzia dla użytkowników domowych/SOHO** | lista urządzeń, alerty | **Fing**, **GlassWire** |
| **Narzędzia CLI** | diagnostyka | ipconfig/ifconfig, ping, arp, tracert, nslookup, netstat, nbtstat |

### Dobór narzędzia do zadania

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

## Podsumowanie

- Podstawowy zestaw: **sniffery** (Wireshark, tcpdump), **skanery** (Nmap/Zenmap, SuperScan), **analiza przepływów** (NetFlow/sFlow), **IDS/IPS i NSM**, **SIEM/SOAR**, **skanery podatności**, **kontrola integralności** (Tripwire), narzędzia **CLI** (ping, arp, netstat, tracert, nslookup, netcat, hping).
- Dobór zależy od pytania (zawartość ruchu, inwentaryzacja, anomalie, ataki, korelacja, podatności).
- Stosować legalnie i etycznie; łączyć narzędzia w ramach monitoringu i procesu reagowania.

---
[⬅️ Poprzedni temat](3_Znaczenie_monitorowania_sieci_lokalnej_w_wykrywaniu_i_analizie_incydentów.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Zasady_projektowania_bezpiecznej_infrastruktury_sieci_lokalnej_i_zabezpieczanie_urządzeń.md)
