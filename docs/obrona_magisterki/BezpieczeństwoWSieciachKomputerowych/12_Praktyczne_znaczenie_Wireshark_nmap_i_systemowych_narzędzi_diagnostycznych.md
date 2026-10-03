# Praktyczne znaczenie narzędzi: Wireshark, nmap oraz systemowe narzędzia diagnostyczne

Narzędzia te służą do **sprawdzania, co faktycznie dzieje się w sieci**. Dają administratorowi i analitykowi realny obraz, a nie tylko założenia z dokumentacji. Są używane do diagnostyki, audytu i wykrywania ataków.

**Wireshark (i tcpdump)** to **analizator pakietów** (sniffer).

- Przechwytuje ruch i dekoduje protokoły na wszystkich warstwach.
- **Zastosowania:** diagnostyka problemów sieciowych, wykrywanie niezaszyfrowanych danych (np. Telnet, FTP, HTTP z hasłami), analiza ataków (skanowanie, ARP spoofing, DoS), analiza powłamaniowa. Przydatne są filtry, „Follow Stream" i statystyki.
- Wymaga dostępu do ruchu (port SPAN/TAP) i **zgody**, bo podsłuch bez uprawnień jest nielegalny.

**Nmap** to **skaner sieci i usług**.

- **Zastosowania:** wykrywanie aktywnych hostów, **otwartych portów i usług** (skany SYN, connect, UDP), rozpoznawanie wersji usług i systemu operacyjnego, skrypty NSE do wykrywania podatności.
- **Praktyka:** inwentaryzacja sieci, **audyt własnej ekspozycji** (które usługi są otwarte), weryfikacja reguł zapory, etap rozpoznania w testach penetracyjnych. Aktywne skanowanie jest wykrywane przez IDS/IPS.

**Systemowe narzędzia diagnostyczne:**

- **Linux:** `ip`, `ss`, `tcpdump`, `iptables`/`nft`, `ping`, `traceroute`, logi w `/var/log`.
- **Windows:** `ipconfig`, `netstat -ano`, `ping`, `tracert`, `nslookup`, Podgląd zdarzeń, PowerShell.
- **Zastosowania:** sprawdzanie konfiguracji i łączności, **lista otwartych portów i połączeń z przypisanymi procesami** (czy malware nie nasłuchuje na porcie), diagnostyka DNS i routingu, przegląd logów.

**Znaczenie praktyczne w skrócie:**

- szybka diagnoza i lokalizacja usterek,
- wykrywanie błędów konfiguracji i niezabezpieczonych protokołów,
- identyfikacja ataków i podejrzanej aktywności,
- weryfikacja skuteczności zabezpieczeń,
- wsparcie audytów i analizy incydentów.

Narzędzia łączy się w scenariuszu audytu. Nmap wykrywa usługi, Wireshark pokazuje, czy dane idą szyfrowane, a narzędzia systemowe i logi potwierdzają, co dzieje się na hostach. Używa się ich **wyłącznie w uprawnionym zakresie**, w ramach polityki bezpieczeństwa organizacji.

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