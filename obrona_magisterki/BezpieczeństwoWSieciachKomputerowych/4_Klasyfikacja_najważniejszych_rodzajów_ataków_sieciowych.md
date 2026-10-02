# Klasyfikacja najważniejszych rodzajów ataków sieciowych

Ataki można klasyfikować według wielu kryteriów; poniżej najczęściej stosowane. Egzaminacyjnie najlepiej przedstawić **klasyfikację według naruszanej własności (CIA)** – tak jak w wykładzie – i uzupełnić ją podziałem według warstw OSI oraz sposobu działania.

## 1. Według naruszanej własności bezpieczeństwa (wykład, W1, slajd 34)

| Atak na… | Cel | Przykłady |
| :--- | :--- | :--- |
| **dostępność** | uniemożliwić korzystanie z usługi | **DoS** (przeciążenie pojedynczego systemu), **DDoS** (z wielu źródeł, botnet), **ping flood, SYN flood**, UDP flood, ataki amplifikacyjne (DNS, NTP, memcached) |
| **poufność** | przechwycić informacje | **packet sniffing** (przechwytywanie ruchu), **Man-in-the-Middle**, **eavesdropping** (podsłuch transmisji), przechwycenie haseł, skanowanie |
| **integralność** | zmodyfikować lub sfałszować dane | **IP spoofing**, **DNS poisoning**, **ARP poisoning**, modyfikacja pakietów, wstrzykiwanie |

*(uzupełnienie)* Atak na **autentyczność/uwierzytelnienie**: podszywanie się, **replay** (powtórzenie przechwyconych komunikatów), brute force, session hijacking.

## 2. Według sposobu działania

| Rodzaj | Opis | Przykłady |
| :--- | :--- | :--- |
| **Pasywne** | nasłuch bez ingerencji, trudne do wykrycia, naruszają poufność | sniffing, analiza ruchu, rozpoznanie pasywne (OSINT) |
| **Aktywne** | ingerencja w ruch lub systemy | spoofing, MITM z modyfikacją, DoS, wstrzykiwanie, skanowanie, exploity |

## 3. Według warstwy modelu OSI *(uzupełnienie + wykład)*

| Warstwa | Ataki |
| :--- | :--- |
| **1 – fizyczna** | podsłuch na kablu, zagłuszanie (jamming), sabotaż, rogue AP |
| **2 – łącza danych** | **MAC flooding**, **ARP spoofing/poisoning**, **rogue DHCP / DHCP starvation**, **ataki na STP**, **VLAN hopping**, MAC spoofing |
| **3 – sieciowa** | **IP spoofing**, ICMP flood, **smurf**, Ping of Death, ataki na routing (RIP/OSPF/BGP), fragmentacja |
| **4 – transportu** | **SYN flood**, **skanowanie portów**, przejęcie sesji TCP (**TCP hijacking**), UDP flood, ACK storms |
| **5–6 – sesji/prezentacji** | session hijacking/fixation, **SSL stripping**, downgrade TLS |
| **7 – aplikacji** | **SQL Injection, XSS, CSRF**, command injection, path traversal, buffer overflow, **DNS poisoning/tunneling**, HTTP flood, brute force, phishing |

Wykład (Zapory/IDS, slajd 30–31) wymienia: spoofing, sniffing, MITM (**ARP poisoning, fałszywe serwery DHCP/DNS**), session hijacking; na L7: SQLi, XSS, command injection, directory traversal.

## 4. Według etapu ataku (cykl życia, APT – wykład, slajd 37)

1. **Rozpoznanie** (reconnaissance: OSINT, skanowanie portów – nmap, masscan),
2. **Pierwsze naruszenie** (initial compromise: exploit, phishing, brute force),
3. **Utrwalenie pozycji** (foothold: backdoor, trojan, RAT),
4. **Eskalacja uprawnień**,
5. **Ruch boczny** (lateral movement),
6. **Eksfiltracja danych** / szkoda (ransomware, sabotaż).

*(uzupełnienie)* Odpowiednik: **Cyber Kill Chain** (Lockheed Martin) i matryca **MITRE ATT&CK**.

## 5. Według pochodzenia i motywacji sprawcy (wykład, slajdy 21–22)

- **wewnętrzni** (insider – złośliwi lub nieświadomi),
- **zewnętrzni:** cyberprzestępcy (zysk), hacktywiści (ideologia), państwa (nation-state: sabotaż, szpiegostwo, APT), script kiddies, zorganizowane grupy przestępcze.

## 6. Według metody / rodzaju techniki

| Kategoria | Opis |
| :--- | :--- |
| **Malware** | wirusy, robaki, trojany (RAT), ransomware, spyware, rootkity, botnety, fileless (wykład, slajdy 21–23) |
| **Ataki na hasła** | brute force, słownikowe, password spraying, credential stuffing |
| **Socjotechnika** | phishing, spear phishing, whaling, vishing, smishing, pretexting, baiting, tailgating |
| **Ataki typu spoofing** | IP, MAC, ARP, DNS, e-mail (SPF/DKIM/DMARC) |
| **MITM / przechwycenie sesji** | ARP/DNS spoofing, rogue AP, SSL stripping, TCP/session hijacking |
| **Ataki DoS/DDoS** | wolumetryczne (łącze), protokołowe (zasoby: SYN flood), warstwy aplikacji (HTTP flood) |
| **Ataki łańcucha dostaw** | NotPetya przez aktualizację MeDoc (wykład, slajd 35) |
| **Zero-day** | exploit nieznanej podatności (wykład, slajd 23) |

### Klasyfikacja DDoS (uzupełnienie)

| Typ | Cel | Przykłady |
| :--- | :--- | :--- |
| wolumetryczny | wysycenie łącza | UDP flood, amplifikacja DNS/NTP/memcached |
| protokołowy | wyczerpanie zasobów urządzeń (stany, tablice) | SYN flood, ACK flood, ataki na fragmentację |
| warstwy aplikacji | zasoby serwera aplikacji | HTTP GET/POST flood, Slowloris |

## Przykłady historyczne (wykład, slajd 35)

| Incydent | Typ | Lekcja |
| :--- | :--- | :--- |
| **WannaCry** (2017, EternalBlue/SMBv1) | robak + ransomware, 230 tys. komputerów | łatanie, segmentacja, kopie offline |
| **NotPetya** (2017) | wiper udający ransomware, łańcuch dostaw | bezpieczeństwo łańcucha dostaw, kopie air-gapped |
| **Emotet** (2014–2021) | botnet / malware-as-a-service | ochrona poczty, EDR |
| **Zeus** (2007–2014) | trojan bankowy | MFA, monitoring transakcji |
| **Mirai** (2016, IoT) | botnet DDoS z urządzeń IoT | zmiana domyślnych haseł, aktualizacje IoT |

## Tabela zbiorcza

| Atak | CIA | Pasywny/aktywny | Warstwa | Obrona (skrót) |
| :--- | :-: | :-: | :-: | :--- |
| Sniffing | poufność | pasywny | 2–7 | szyfrowanie, segmentacja, port security |
| ARP poisoning | integralność/poufność | aktywny | 2 | DAI, DHCP snooping |
| IP spoofing | integralność | aktywny | 3 | uRPF, filtry wejścia/wyjścia |
| SYN flood | dostępność | aktywny | 4 | SYN cookies, zapora stanowa, rate limit |
| DDoS | dostępność | aktywny | 3–7 | scrubbing, CDN, load balancing, rate limiting |
| DNS poisoning | integralność | aktywny | 7 | DNSSEC, zabezpieczone resolvery |
| MITM | poufność/integralność | aktywny | 2–7 | TLS, certyfikaty, DAI |
| SQLi/XSS | wszystkie | aktywny | 7 | WAF, walidacja, parametryzacja |

## Podsumowanie

- Podstawowa klasyfikacja (wykład): ataki na **dostępność** (DoS/DDoS, flood), **poufność** (sniffing, MITM, podsłuch), **integralność** (IP spoofing, DNS i ARP poisoning).
- Dodatkowo: pasywne vs aktywne, według warstwy OSI, według etapu ataku (APT), według sprawcy, według techniki (malware, socjotechnika, hasła, aplikacje webowe).
- Znajomość klasyfikacji pozwala dobrać **warstwową obronę** i narzędzia detekcji.

---
[⬅️ Poprzedni temat](3_Analiza_protokołów_i_usług_sieciowych_w_ocenie_bezpieczeństwa.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Metody_detekcji_i_obrony_przed_atakami_w_warstwie_II_modelu_OSI.md)