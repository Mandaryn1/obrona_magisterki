# Praktyczne znaczenie narzędzi: Wireshark, nmap oraz systemowe narzędzia diagnostyczne

> Wykład wymienia Wireshark, tcpdump, Zeek, NetFlow (*Zapory/IDS*, slajd 33), nmap/masscan jako narzędzia skanowania (slajd 17), Wireshark w analizie dynamicznej malware (slajd 22) oraz polecenia logów Windows/Linux (MBK1, slajd 49). Szczegóły użycia – ***(uzupełnienie)***.

## Zastosowanie w obronie i w atakach

Narzędzia są **neutralne** – używają ich zarówno **obrońcy** (administratorzy, SOC, audytorzy, pentesterzy), jak i atakujący. Obrońca **musi znać** te same techniki, aby wykrywać ataki i weryfikować własną sieć. Uwaga prawna: skanowanie i przechwytywanie ruchu **wymaga zgody właściciela** sieci; nieuprawnione działania mogą naruszać prawo (np. art. 267 k.k. – nieuprawniony dostęp do systemu), regulaminy i RODO.

## 1. Wireshark

**Analizator protokołów** z GUI (wykład: *najpopularniejszy analizator pakietów; dekodowanie setek protokołów, filtrowanie ruchu, śledzenie strumieni TCP, statystyki*). Przechwytuje ruch z interfejsu (Npcap/libpcap) lub czyta pliki `.pcap/.pcapng` (także z tcpdump).

### Praktyczne znaczenie

| Zastosowanie | Przykład |
| :--- | :--- |
| **Diagnostyka sieci** | opóźnienia, retransmisje TCP, błędne uzgadnianie, problemy DNS/DHCP, MTU |
| **Analiza incydentów (forensics)** | rekonstrukcja ataku z nagrania ruchu, wyciąganie plików i haseł z ruchu jawnego |
| **Wykrywanie ataków** | ARP spoofing (`arp.duplicate-address-detected`), SYN flood/skanowanie (`tcp.flags.syn==1 && tcp.flags.ack==0`), rogue DHCP (`dhcp`), tunelowanie DNS (długie `dns.qry.name`), C2 beaconing |
| **Audyt bezpieczeństwa protokołów** | czy ruch jest szyfrowany (HTTP vs HTTPS, Telnet, FTP, SNMPv2c) |
| **Analiza malware** (wykład, slajd 22) | ruch sieciowy próbki w sandboxie |
| **Nauka protokołów** | wgląd w nagłówki i handshaki |

### Przydatne funkcje

- **filtry przechwytywania** (BPF, np. `host 10.0.0.5 and port 443`) i **filtry wyświetlania** (`ip.addr==10.0.0.5`, `tcp.port==445`, `http.request`, `dns`, `icmp`, `ssl.handshake`),
- **Follow TCP/HTTP Stream**, **Statistics → Conversations/Protocol Hierarchy/I/O Graphs**, **Expert Information** (anomalie), eksport obiektów (HTTP, SMB),
- deszyfracja TLS po podaniu klucza sesji (SSLKEYLOGFILE) – w środowisku testowym.

### Ograniczenia

- szyfrowany ruch (TLS 1.3) – widoczne tylko metadane,
- duże zbiory danych (lepiej tcpdump + analiza offline lub NetFlow/Zeek),
- w sieciach przełączanych potrzebny **SPAN/TAP**, aby widzieć cudzy ruch,
- wymaga zgody i ochrony prywatności.

## 2. nmap (Network Mapper)

**Skaner sieci**: wykrywa **hosty, otwarte porty, usługi i wersje, system operacyjny**; z silnikiem skryptów **NSE**.

### Podstawowe skanowania

| Polecenie | Znaczenie |
| :--- | :--- |
| `nmap -sn 10.0.0.0/24` | **ping sweep** – które hosty żyją (bez skanowania portów) |
| `nmap -sS -p- 10.0.0.5` | **SYN scan** (półotwarty, „stealth") wszystkich portów |
| `nmap -sT` | skan pełnym połączeniem (bez uprawnień root) |
| `nmap -sU --top-ports 100` | skan UDP |
| `nmap -sV` | wykrywanie **wersji usług** |
| `nmap -O` | wykrywanie **systemu operacyjnego** |
| `nmap -A` | skan agresywny (OS, wersje, skrypty, traceroute) |
| `nmap --script vuln` | skrypty NSE do wykrywania podatności |
| `nmap -Pn` | pomijanie wykrywania hosta (gdy ICMP blokowany) |
| `-T0…-T5`, `-oA wynik` | tempo skanowania; zapis wyników |

### Praktyczne znaczenie

| Zastosowanie | Opis |
| :--- | :--- |
| **Inwentaryzacja** | jakie urządzenia i usługi działają w sieci (także „shadow IT", rogue devices) |
| **Audyt powierzchni ataku** | czy nie ma niepotrzebnych otwartych portów/usług (Telnet, SMB, RDP, bazy) |
| **Weryfikacja reguł zapory** | czy zapora naprawdę blokuje (skan z zewnątrz i z segmentów) |
| **Ocena podatności** | stare wersje usług, domyślne konfiguracje, skrypty NSE |
| **Test penetracyjny** (etap rozpoznania) | mapowanie celu |
| **Dla atakującego** | wykrywanie celów i usług; stąd **IDS/IPS wykrywa skanowanie** (wykład, slajd 17) |

**Reakcja obrońcy:** wykrywanie skanów (IDS, zapora, NetFlow), ograniczanie ekspozycji usług, **rate limiting**, blokada źródła; skanowanie *własnej* sieci regularnie (przed atakującym).

## 3. Systemowe narzędzia diagnostyczne

Dostępne „od ręki" na każdym systemie – podstawa **diagnostyki i wstępnej analizy incydentu** bez instalowania dodatków.

| Cel | Linux | Windows |
| :--- | :--- | :--- |
| **Konfiguracja interfejsów, IP** | `ip a`, `ifconfig`, `ip r` | `ipconfig /all`, `route print` |
| **Dostępność hosta** | `ping` | `ping` |
| **Trasa pakietów** | `traceroute`, `mtr` | `tracert`, `pathping` |
| **Tablica ARP** | `ip neigh`, `arp -a` | `arp -a` |
| **Otwarte porty i połączenia (z procesami)** | `ss -tulpn`, `netstat -tulpn`, `lsof -i` | `netstat -ano`, `Get-NetTCPConnection` |
| **DNS** | `dig`, `nslookup`, `host` | `nslookup`, `Resolve-DnsName` |
| **Połączenie z portem / test usługi** | `nc` (netcat), `curl`, `telnet` | `Test-NetConnection`, `curl` |
| **Przechwytywanie ruchu** | `tcpdump` | `pktmon`, Wireshark/Npcap |
| **Zapora** | `iptables -L -n -v`, `nft list ruleset`, `ufw status` | `netsh advfirewall show allprofiles`, `Get-NetFirewallRule` |
| **Logi** | `/var/log/auth.log`, `journalctl`, `auditd` | Podgląd zdarzeń: **4624, 4625, 4670, 4663** |
| **Procesy i usługi** | `ps`, `top`, `systemctl` | `tasklist`, Menedżer zadań, `Get-Process` |

### Praktyczne przykłady użycia

- **Wykrycie ARP spoofingu:** `arp -a` – dwa różne IP (w tym brama) z **tym samym adresem MAC**; potwierdzenie w Wireshark (duplikaty ARP).
- **Podejrzane połączenie wychodzące:** `netstat -ano` (Windows) / `ss -tunp` (Linux) → PID → proces → analiza, izolacja.
- **Brute force SSH:** `grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr` → lista źródłowych IP → blokada (fail2ban).
- **Nieudane logowania Windows:** `Get-EventLog -LogName Security -InstanceId 4625 -Newest 20`.
- **Problem z siecią:** `ping` (L3) → `traceroute` (gdzie ginie) → `dig` (DNS) → `nc -vz host 443` (port) → `tcpdump` (co faktycznie płynie).
- **Test reguły zapory:** `Test-NetConnection host -Port 3389` lub `nc -vz host 22`.

## Współpraca narzędzi w scenariuszu audytowym

```
 1. nmap -sn / -sS       → inwentaryzacja hostów i portów
 2. nmap -sV --script vuln → usługi i podatności
 3. Wireshark/tcpdump      → czy ruch jest szyfrowany, jakie protokoły płyną
 4. ss/netstat, logi       → procesy odpowiadające za usługi i logowania
 5. raport + rekomendacje  → utwardzenie, reguły zapory, aktualizacje
```

## Podsumowanie

- **Wireshark** – głęboka analiza ruchu (diagnostyka, forensics, wykrywanie ataków i niezaszyfrowanych protokołów); **tcpdump** – zdalne/skryptowe przechwytywanie.
- **nmap** – inwentaryzacja hostów i usług, audyt powierzchni ataku, weryfikacja zapory; wykrywany przez IDS jako rozpoznanie.
- **Narzędzia systemowe** (ping, traceroute, ip/ipconfig, ss/netstat, arp, dig/nslookup, logi) – szybka diagnostyka i wstępna analiza incydentu bez dodatkowego oprogramowania.
- Używać **wyłącznie za zgodą właściciela sieci**; łączyć narzędzia z monitoringiem (SIEM, NetFlow, IDS).

---
[⬅️ Poprzedni temat](11_Podstawowe_działania_administratora_w_zakresie_zabezpieczania_infrastruktury_sieciowej.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../OchronaSieciDostępowych/OchronaSieciDostępowych_tytul.md)