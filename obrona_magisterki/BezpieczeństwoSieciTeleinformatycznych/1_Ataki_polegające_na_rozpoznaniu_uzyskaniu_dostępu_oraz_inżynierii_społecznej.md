# Ataki polegające na rozpoznaniu, uzyskaniu dostępu oraz inżynierii społecznej

> Opracowanie oparte na W5 (rozpoznanie pasywne i aktywne), W3 (socjotechnika, ATT&CK, aktorzy zagrożeń), wykładzie o zaporach/IDS (slajdy 17, 30, 34, 35) i poprzednim wykładzie o mechanizmach bezpieczeństwa (APT, socjotechnika); uzupełnienia – ***(uzupełnienie)***.

## Ujęcie: cykl życia ataku

Atak na sieć teleinformatyczną to zwykle proces wieloetapowy. Trzy grupy z tematu odpowiadają jego początkowym etapom:

```
 1. ROZPOZNANIE ─▶ 2. UZYSKANIE DOSTĘPU ─▶ 3. UTRWALENIE / ESKALACJA ─▶ 4. RUCH BOCZNY ─▶ 5. CEL (eksfiltracja, szyfrowanie, sabotaż)
        ▲                    ▲
        └── inżynieria społeczna (może wspierać etap 1 i 2) ──┘
```

Wykład (APT): *rozpoznanie → pierwsze naruszenie → utrwalenie pozycji → eskalacja uprawnień → ruch boczny → eksfiltracja*. Odpowiedniki: **Cyber Kill Chain** (Lockheed Martin) i matryca **MITRE ATT&CK**, która (W3) zawiera taktyki i techniki adwersarzy – od zbierania informacji (OSINT) po eksploatację i post-eksploatację.

**Sprawcy (W3):** zorganizowana przestępczość (zysk), hacktywiści (ideologia), podmioty państwowe (cyberwojna, szpiegostwo), zagrożenia wewnętrzne (insiderzy – oszukani lub złośliwi).

# CZĘŚĆ A. Ataki polegające na rozpoznaniu (reconnaissance)

**Rozpoznanie jest zawsze pierwszym krokiem w cyberataku** (W5): aby odnieść sukces, atakujący musi zebrać informacje o celu. W testach penetracyjnych rozpoznanie obejmuje **skanowanie i enumerację**.

## Rozpoznanie pasywne i aktywne (W5)

| | **Pasywne** | **Aktywne** |
| :--- | :--- | :--- |
| Zasada | **bez bezpośredniej interakcji** z celem; zewnętrzne bazy danych, nasłuch ruchu | **wysyłanie sond** do sieci/systemów i analiza odpowiedzi |
| Wykrywalność | trudne lub niemożliwe do wykrycia przez cel | **wykrywalne** (logi, IDS/IPS, zapory) |
| Metody (W5) | enumeracja domen, inspekcja pakietów, **OSINT**, Recon-ng, podsłuch | enumeracja hostów, sieci, użytkowników, grup, udziałów, stron WWW, aplikacji, usług; tworzenie pakietów |
| Inwazyjność | brak | wyższa – ostrożność (zasięg, tempo, ryzyko awarii) |

## Techniki rozpoznania pasywnego (W5)

| Technika | Co daje atakującemu | Narzędzia |
| :--- | :--- | :--- |
| **Wyszukiwanie DNS** | adresy IP domeny i subdomen, serwery poczty (rekordy MX), struktura | DNSRecon, `nslookup`, `host`, `dig` |
| **Whois** | kontakty techniczne i administracyjne domeny, rejestrator | `whois` |
| **Aplikacje w chmurze a własne** | czyj to właściciel adresów IP (np. AWS hostujący domenę) | `host`, `whois` (OrgName) |
| **Media społecznościowe i oferty pracy** | pracownicy, role, kontakty kluczowe, **stosowane technologie** (z ogłoszeń); podstawa pod spear phishing/whaling; fałszywe ogłoszenia o pracę „na rozmowę" | scrapowanie LinkedIn, Facebook, Twitter |
| **Certyfikaty SSL/TLS** | nazwa wspólna, URI serwera, organizacja, OCSP/CRL, wady kryptograficzne | narzędzia SSL w Kali |
| **Przejrzystość certyfikatów (Certificate Transparency)** | **wyliczanie subdomen** | **crt.sh** |
| **Zrzuty haseł z naruszeń** | adresy e-mail i hasła z wcześniejszych wycieków (Pastebin, dark web, GitHub) | **h8mail**, WhatBreach |
| **Metadane plików** | autor, oprogramowanie, lokalizacja (Exif w zdjęciach), nazwy użytkowników w dokumentach | **ExifTool** |
| **Google hacking (dorks)** | poufne pliki, panele, logi z identyfikatorami sesji, serwery podatne | operatory `filetype:`, `inurl:`, `intext:`; **GHDB** |
| **Archiwa stron** | dawne wersje, ujawnione wcześniej informacje | Wayback Machine |
| **Publiczne repozytoria kodu** | kod, konfiguracje, **klucze i hasła**, architektura | GitHub, GitLab |
| **OSINT (platformy)** | zbiorcze gromadzenie danych | **Recon-ng** (moduły, API, raporty), SpiderFoot |
| **Shodan** | internetowa wyszukiwarka urządzeń: podatne hosty, IoT, **ICS**, bazy danych, urządzenia sieciowe (np. wystawiony protokół **Cisco Smart Install**) | shodan.io, CLI |
| **Podsłuch** | pasywne zbieranie ruchu (fizyczny lub bezprzewodowy dostęp) | Wireshark, tshark, tcpdump |

*W5:* zasięg sieci bezprzewodowej firmy często wykracza poza granice budynku – atakujący ma możliwość podsłuchu bez wchodzenia do środka.

## Techniki rozpoznania aktywnego (W5)

### Skanowanie portów (Nmap)

| Skan | Działanie | Odpowiedzi (stan portu) | Uwagi |
| :--- | :--- | :--- | :--- |
| **SYN (`-sS`, półotwarty)** | wysyła SYN, nie kończy połączenia | SYN/ACK = otwarty; RST = zamknięty; brak odpowiedzi = filtrowany | mniej widoczny w logach |
| **TCP connect (`-sT`)** | pełne połączenie przez OS | j.w. | więcej szumu, **IDS częściej alarmuje**; gdy brak uprawnień |
| **UDP (`-sU`)** | pakiet UDP na port | dane = otwarty; ICMP port unreachable = zamknięty; cisza = otwarty/filtrowany | DNS, SNMP, DHCP |
| **TCP FIN (`-sF`)** | pakiet FIN | RST = zamknięty; cisza = otwarty/filtrowany | omijanie prostych filtrów |
| **wykrywanie hostów (`-sn`)** | ICMP (*ping sweep*) | host aktywny/nie | inwentaryzacja podsieci |
| **tempo `-T0…-T5`** | od „paranoid" (unikanie IDS) do „insane" | | T4/T5 mogą przeciążyć cel |

### Enumeracja (W5)

| Rodzaj | Cel / narzędzie |
| :--- | :--- |
| **hostów** | pierwsze zadanie; Nmap, Masscan (z ograniczeniem do adresów w zakresie testu) |
| **użytkowników** | pierwszy krok do łamania poświadczeń; SMB (TCP 445), `smb-enum-users` |
| **grup** | role autoryzacyjne; `smb-enum-groups` |
| **udziałów sieciowych** | powierzchnia ataku sieci wewnętrznej; `smb-enum-shares`, enum4linux, smbclient |
| **stron WWW/aplikacji** | NSE `http-enum`, **Nikto** (hałaśliwy) |
| **usług** | rozpoznanie usług i wersji; `-sV`, `-sC` |
| **przez tworzenie pakietów** | **Scapy** (Python) – własne pakiety ICMP/TCP SYN |
| **Skanery podatności** | Nessus, OpenVAS/GVM – w tle Nmap + korelacja z podatnościami |

## Rozpoznanie w sieci lokalnej *(uzupełnienie)*

skanowanie ARP (`arp-scan`), nasłuch rozgłoszeń (LLMNR, NBNS, mDNS), SNMP walk (domyślne community), fingerprinting systemów, mapowanie VLAN/CDP.

## Wykrywanie i obrona przed rozpoznaniem

- **IDS/IPS** wykrywają **skanowanie portów** charakterystycznymi wzorcami (wykład: *Nmap, masscan*) i **brute force**; reguły IPS blokują źródłowy adres po przekroczeniu progu,
- **zapora** z domyślną odmową, filtrowanie ICMP, **rate limiting**, ukrywanie baneru i wersji,
- **minimalna ekspozycja**: tylko niezbędne usługi w Internecie, regularne **skanowanie własnej ekspozycji** (Shodan, Nmap) i usuwanie wystawionych urządzeń (np. Smart Install),
- higiena informacji: **czyszczenie metadanych**, kontrola informacji w ogłoszeniach o pracę i mediach społecznościowych, przegląd repozytoriów (skanowanie sekretów), ochrona rekordów Whois (prywatność), kontrola wpisów w CT i DNS,
- **honeypoty** i deception, SIEM (korelacja skanów),
- monitoring wycieków (czy firmowe adresy e-mail występują w dumpach).

# CZĘŚĆ B. Ataki polegające na uzyskaniu dostępu

Po rozpoznaniu atakujący **uzyskuje dostęp** (ang. *initial access / gaining access*) do sieci lub systemu. Wykład: *„pierwsze naruszenie bezpieczeństwa"*, następnie utrwalenie, eskalacja uprawnień i ruch boczny; W5: enumeracja użytkowników jest *pierwszym krokiem do złamania poświadczeń*, a skanery podatności wskazują słabe punkty do wykorzystania.

## Rodzaje

| Wektor | Opis | Przykłady |
| :--- | :--- | :--- |
| **Wykorzystanie podatności (exploit)** | luki w usługach i aplikacjach brzegowych | **EternalBlue/SMBv1** (WannaCry), **Shellshock, Heartbleed** (wykład, slajd 17), podatności VPN/firewalli/serwerów pocztowych, **zero-day** |
| **Ataki na hasła** | brute force, słownikowe, **password spraying**, **credential stuffing** (hasła z wycieków – W5), domyślne i słabe hasła | SSH, RDP, VPN, panele WWW (wykład: IPS blokuje po progu nieudanych prób) |
| **Ataki na aplikacje webowe** | SQL Injection, XSS, CSRF, command injection, path traversal, deserializacja | wykład, slajdy 31, 35 (W1) |
| **Przechwycenie ruchu i sesji** | **MITM** (ARP poisoning, fałszywe serwery DHCP/DNS), sniffing, **session hijacking** (przewidzenie numerów sekwencyjnych), SSL stripping | wykład, slajd 30 |
| **Ataki na usługi sieciowe i protokoły** | DNS poisoning, SMB relay, ataki na SNMP/Telnet | |
| **Phishing i socjotechnika** | najczęstszy sposób wejścia (część C) | |
| **Dostęp bezprzewodowy** | **rogue AP / evil twin**, łamanie WPA | |
| **Dostęp fizyczny** | podpięcie urządzenia, kradzież, tailgating, nośniki USB (*baiting*) | |
| **Ataki na łańcuch dostaw** | skompromitowana aktualizacja lub dostawca | **NotPetya** przez MeDoc (wykład, slajd 35) |
| **Przejęte konta i VPN** | użycie skradzionych poświadczeń do legalnego wejścia | brak MFA |
| **Błędy konfiguracji i ekspozycja** | otwarte porty, panele administracyjne, bazy w Internecie (Shodan) | |

## Po uzyskaniu dostępu (post-eksploatacja)

**Utrwalenie (persistence)** – backdoor, trojan RAT, konta, zadania zaplanowane; **eskalacja uprawnień**; **ruch boczny** (pass-the-hash, RDP, SMB); **eksfiltracja**; *zacieranie śladów* (W4/M2: faza ataku obejmuje persistence i posprzątanie).

## Obrona

- **zarządzanie podatnościami i poprawkami** (WannaCry – lekcja z wykładu), ograniczenie ekspozycji usług, **zapory**, **IPS** z sygnaturami exploitów,
- **silne uwierzytelnianie: MFA**, polityki haseł, blokada kont, Fail2Ban, rate limiting, CAPTCHA, zakaz haseł domyślnych,
- **WAF**, walidacja danych, parametryzowane zapytania,
- **szyfrowanie** (TLS, WPA3), certyfikaty, DAI/DHCP snooping, DNSSEC,
- **segmentacja i least privilege** (ograniczenie ruchu bocznego), EDR,
- **monitoring i SIEM**, honeypoty, reakcja na incydenty.

# CZĘŚĆ C. Inżynieria społeczna (social engineering)

**Definicja (wykład):** techniki **psychologicznej manipulacji** mające na celu uzyskanie poufnych informacji lub dostępu do systemów przez **wykorzystanie ludzkich słabości**. Wg W3: *większość włamań dziś zaczyna się od jakiegoś ataku socjotechnicznego* – rozmowy telefonicznej, e-maila, strony WWW, SMS-a.

## Typy ataków (wykład, slajdy 34 i W1:36)

| Typ | Opis |
| :--- | :--- |
| **Phishing** | masowe e-maile i strony podszywające się pod legalne instytucje |
| **Spear phishing** | ukierunkowany na konkretne osoby lub zespoły, spersonalizowany (dane z rozpoznania pasywnego – media społecznościowe) |
| **Whaling** | wymierzony w **kadrę kierowniczą** |
| **Vishing** | phishing telefoniczny (rozmowa) |
| **Smishing** | phishing przez SMS |
| **Pretexting** | fałszywy scenariusz (np. podszycie się pod dział IT) w celu wzbudzenia zaufania |
| **Baiting** | „przynęta" – zainfekowany USB zostawiony w miejscu publicznym |
| **Tailgating** | wejście za uprawnioną osobą do strefy zabezpieczonej |
| *(uzupełnienie)* **BEC** (Business Email Compromise) | podszywanie się pod przełożonego/dostawcę w celu wyłudzenia przelewu |
| *(uzupełnienie)* **Quid pro quo, watering hole, deepfake voice/video, MFA fatigue, QR phishing (quishing)** | |

## Ataki socjotechniczne a testowanie (W3)

Test socjotechniczny często pomijany w zakresie, bo dotyczy **ludzi, nie technologii**, a kierownictwo bywa niechętne; jego celem jest **ocena programu świadomości bezpieczeństwa, a nie wskazanie osób, które „oblały"**. Narzędzie: **Social-Engineer Toolkit (SET)** (Dave Kennedy). Testy fizyczne – włamanie do obiektu – oceniają strażników, bramki, ogrodzenia.

## Obrona (wykład, slajd 34 i W1:55)

**Techniczne:** bramki bezpieczeństwa poczty z antyphishingiem; **SPF, DKIM, DMARC** (weryfikacja nadawcy); analiza linków i reputacji URL; **sandboxing załączników**; **IPS** blokujący znane domeny phishingowe; **filtrowanie DNS** (np. Cisco Umbrella); wykrywanie złośliwych makr na stacjach (**EDR**); MFA odporne na phishing (FIDO2).

**Organizacyjne – klucz:** **program świadomości bezpieczeństwa** dla wszystkich: regularne szkolenia, **symulowane kampanie phishingowe**, komunikacja polityk, **zachęcanie do zgłaszania** incydentów; procedury weryfikacji próśb o przelew/dane (drugi kanał), kontrola dostępu fizycznego (zasada „nie wpuszczaj za sobą"), nośniki USB zablokowane.

## Powiązania części A, B i C – przykład scenariusza

1. **Rozpoznanie pasywne:** z LinkedIn – nazwiska pracowników IT; z ofert pracy – używany VPN i system pocztowy; z crt.sh – subdomena `vpn.firma.pl`; z dumpów (h8mail) – hasła z innego serwisu.
2. **Socjotechnika:** spear phishing do administratora z linkiem do fałszywego portalu VPN.
3. **Dostęp:** logowanie skradzionymi poświadczeniami do VPN (brak MFA).
4. **Aktywne rozpoznanie wewnątrz:** skan SMB, enumeracja użytkowników, grup i udziałów.
5. **Ruch boczny i eksfiltracja.**

Przerwanie łańcucha: MFA (etap 3), filtry poczty i szkolenia (2), minimalizacja informacji publicznych (1), segmentacja i IDS wewnętrzny (4–5).

## Podsumowanie

- **Rozpoznanie** (pierwszy krok): **pasywne** (DNS, Whois, OSINT, certyfikaty i CT/crt.sh, wycieki, metadane, Google dorks, Wayback, GitHub, Shodan, Recon-ng) i **aktywne** (skany portów Nmap: SYN, connect, UDP, FIN; enumeracja hostów, użytkowników, grup, udziałów, WWW, usług; Scapy); aktywne – wykrywalne przez IDS/IPS.
- **Uzyskanie dostępu:** exploity podatności, ataki na hasła, ataki aplikacyjne, MITM i przejęcie sesji, dostęp fizyczny i bezprzewodowy, łańcuch dostaw, przejęte konta; potem utrwalenie, eskalacja, ruch boczny.
- **Inżynieria społeczna:** phishing, spear phishing, whaling, vishing, smishing, pretexting, baiting, tailgating; obrona – filtry poczty (SPF/DKIM/DMARC), sandbox, filtrowanie DNS, **szkolenia i symulacje**.
- Obrona warstwowa: minimalna ekspozycja, MFA, aktualizacje, IPS/IDS, segmentacja, monitoring, świadomość użytkowników.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Standardy_i_dobre_praktyki_bezpieczeństwa_ISO_27001_NIS2_i_inne_normy.md)