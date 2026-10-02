# Etapy i metody testowania bezpieczeństwa sieci teleinformatycznych

> Opracowanie oparte głównie na **W3** (wstęp do etycznego hackingu i metodyk), **W4** (planowanie i zakres testów), **W5** (rozpoznanie, skanowanie podatności, analiza wyników), uzupełnione kursem Cisco *Cyber Threat Management* (z przedmiotu *Ochrona sieci dostępowych*: ST&E, cztery fazy, black/gray/white box, red/blue/white/purple team). Uzupełnienia – ***(uzupełnienie)***.

## Po co testować bezpieczeństwo sieci (W3)

Ktoś odpowiedzialny za sieć chce **znaleźć możliwe ścieżki włamania, zanim zrobią to napastnicy**. Firewalle, IPS, antywirusy, VPN, WAF – **nie wystarczy ich wdrożyć, trzeba sprawdzić, czy działają**, i robić to **regularnie**, bo sieci i **powierzchnia ataku zmieniają się** (nowa ekspozycja → ponowna ocena). Test odpowiada na pytania: *czy obrona wytrzyma? co chronimy i jaka jest wartość danych?*

**Etyczny haker (W3):** działa jak atakujący, ale **za zgodą** – *„zakres testu (scope)" jest kluczowy*; odpowiedzialnie zgłasza luki; używa tych samych narzędzi co napastnicy, unikając destrukcyjnych działań. Kluczowa różnica etyczny/nieetyczny: **intencja i uprawnienie**.

# CZĘŚĆ A. Metodyki i standardy (W3)

Stosowanie znanej metodyki zapewnia **systematyczność, powtarzalność, rozliczalność i obronę wyników** oraz zapobiega „scope creep".

| Metodyka | Opis |
| :--- | :--- |
| **MITRE ATT&CK** | baza **taktyk, technik i procedur (TTP)** przeciwnika; macierze: Enterprise, Network, Cloud, ICS, Mobile; od OSINT po post-eksploatację; używana przez pentesterów, red team, zespoły IR i threat hunting |
| **OWASP Web Security Testing Guide (WSTG)** | najbardziej szczegółowy przewodnik testowania aplikacji WWW (XSS, XXE, CSRF, SQLi…) |
| **NIST SP 800-115** | wytyczne planowania i prowadzenia testów bezpieczeństwa informacji; standard branżowy; zastąpił SP 800-42 |
| **OSSTMM** (ISECOM) | powtarzalna metodyka: metryki bezpieczeństwa operacyjnego, analiza zaufania, **testy ludzi, fizyczne, bezprzewodowe, telekomunikacyjne, sieci danych**, zgodność, raport STAR |
| **PTES** (Penetration Testing Execution Standard) | **7 faz:** interakcje przedwstępne → zbieranie informacji → modelowanie zagrożeń → analiza podatności → eksploatacja → post-eksploatacja → raportowanie |
| **ISSAF** | fazy: zbieranie informacji, mapowanie sieci, identyfikacja podatności, penetracja, uzyskanie dostępu i eskalacja uprawnień, dalsza enumeracja, kompromitacja zdalnych użytkowników/lokalizacji, utrzymanie dostępu, zacieranie śladów |

## Środowiska testów (W3)

| Środowisko | Zakres |
| :--- | :--- |
| **Testy infrastruktury sieciowej** | przełączniki, routery, zapory, serwery AAA, IPS; często także infrastruktura bezprzewodowa |
| **Testy aplikacji** | błędy konfiguracji, walidacja danych, wstrzykiwanie, logika; wraz z bazą danych |
| **Testy w chmurze** | zależne od modelu SaaS/PaaS/IaaS; odpowiedzialność klienta i dostawcy ustalona w umowie |
| **Testy fizyczne** | włamanie do obiektu – ocena perymetru, strażników, bram |
| **Testy socjotechniczne** | ocena zachowania ludzi (telefon, e-mail, WWW, SMS) – cel: ocena i poprawa programu świadomości, **nie wskazywanie osób**; narzędzie **SET** |

## Perspektywa testera (W3, W4)

| Typ | Wiedza | Cechy |
| :--- | :--- | :--- |
| **Unknown-environment** (dawniej black-box) | minimum (domeny/IP) – perspektywa zewnętrznego atakującego | najmniej czasochłonny i najtańszy (kurs Cisco); bez wiedzy personelu sieci |
| **Partially known** (gray-box) | częściowa (np. poświadczenia, brak pełnej dokumentacji) | kompromis |
| **Known-environment** (white-box) | pełna (diagramy, adresy, konfiguracje, poświadczenia, kod źródłowy) | najpełniejsza identyfikacja luk; najdroższy |

Dobór zależy od czasu i budżetu (W4): przy znanym środowisku zakres może ograniczać się do znalezienia ścieżki do organizacji; przy nieznanym – szerszy (audyt konfiguracji wewnętrznej, skan stacji).

# CZĘŚĆ B. Etapy testu penetracyjnego

Wersja z kursu Cisco i NIST SP 800-115 (4 fazy) oraz rozszerzona PTES (7 faz):

| Faza (Cisco/NIST) | Faza (PTES) | Treść |
| :--- | :--- | :--- |
| **1. Planowanie** | Interakcje przedwstępne | ustalenie zasad, zakresu, zgód, harmonogramu |
| **2. Odkrywanie** | Zbieranie informacji + Modelowanie zagrożeń + Analiza podatności | rozpoznanie pasywne (*footprinting*) i aktywne (skanowanie portów), identyfikacja podatności |
| **3. Atak** | Eksploatacja + Post-eksploatacja | uzyskanie dostępu, **eskalacja uprawnień, ruch boczny**, instalacja narzędzi/backdoora (**persistence**), posprzątanie śladów |
| **4. Raportowanie** | Raportowanie | dokumentacja podatności, działań i wyników |

## Etap 1 – Planowanie i określanie zakresu (W4)

> *Planowanie i przygotowanie to jedna z najważniejszych (a może najważniejsza) faz; zły zakres → problemy z klientem, a nawet prawne.*

**Kluczowe elementy do ustalenia:** grupa docelowa raportu, **reguły zaangażowania (Rules of Engagement)**, ścieżka eskalacji i kanały komunikacji, zasoby i wymagania, budżet, zastrzeżenia (disclaimers), ograniczenia techniczne.

### Dokumenty i zagadnienia prawne (W4)

| Element | Treść |
| :--- | :--- |
| **Pisemna zgoda i autoryzacja** | podpis osoby upoważnionej; zgoda stron trzecich (ISP, dostawca chmury) |
| **Reguły zaangażowania (RoE)** | okno czasowe, lokalizacja, adresy źródłowe testów, dozwolone/zabronione rodzaje testów (np. brak socjotechniki; SQLi tylko w środowisku testowym), kontrole bezpieczeństwa, które mogą wykryć testy, sposób komunikacji |
| **SOW (Statement of Work)** | zakres, harmonogram, lokalizacja, wymagania, płatności |
| **MSA (Master Service Agreement)** | umowa ramowa dla powtarzalnych zleceń |
| **NDA** | jednostronna, dwustronna, wielostronna – poufność |
| **SLA** | oczekiwania co do jakości/terminu usługi |
| **Umowa (contract)** | konkretna, bez dwuznaczności; doradztwo prawne; ograniczenia dot. przenoszenia danych/PII poza kraj |
| **Poufność danych** | co robić z hasłami i danymi wrażliwymi, usunięcie po zakończeniu |
| **Zgodność i regulacje** | **PCI DSS** (wymaga testów, skanów ASV), **RODO/GDPR**, HIPAA, FedRAMP, GLBA, NY DFS 500.05 (testy penetracyjne wymagane) – wiele regulacji wymaga zewnętrznych testów; **ograniczenia lokalne** prawa (np. CFAA w USA; w Polsce art. 267 k.k.) |
| **Zakres (scope)** | lista docelowa (**allow list**): IP, FQDN (także subdomeny), SSID sieci bezprzewodowych, API (SOAP, REST/Swagger, GraphQL, WSDL), wewnętrzne/zewnętrzne, chmura (ograniczenia dostawcy); materiały: SDK, kod źródłowy, diagramy architektury |
| **Scope creep** | niekontrolowany wzrost zakresu – **zarządzanie zmianami i nowe SOW**, jasna komunikacja |
| **Walidacja zakresu** | pytania do klienta (odbiorca raportu, cel, uprawnienia decyzyjne), interesariusze, kontakt awaryjny, bezpieczny transfer (SCP/SFTP, PGP/S-MIME) |
| **Ograniczenia techniczne i narzędzi** | systemy poza zakresem (krytyczność), narzędzia zabronione |
| **Budżet/ROI** | test jest **oceną w punkcie czasowym (point-in-time)** i nie gwarantuje całkowitego bezpieczeństwa; muszą towarzyszyć **realistyczne plany naprawcze** i terminy |

**Etyka i profesjonalizm (W4):** sprawdzanie przeszłości zespołów, **natychmiastowe zgłaszanie wykrytej przestępczej aktywności** (jeśli prawdziwy napastnik już jest w sieci), ochrona poufności wyników, ubezpieczenie odpowiedzialności, ograniczanie inwazyjności narzędzi do zakresu, lab testowy przed użyciem narzędzi u klienta (W3: środowisko wirtualne, snapshoty, zamknięta sieć, Kali/Parrot).

## Etap 2 – Odkrywanie: rozpoznanie i skanowanie (W5)

*(Szczegóły technik – temat 1.)*

1. **Rozpoznanie pasywne** – DNS, Whois, OSINT, media społecznościowe, certyfikaty/CT, wycieki, metadane, Google dorks, archiwa, repozytoria kodu, Shodan, Recon-ng.
2. **Rozpoznanie aktywne** – enumeracja hostów, usług (Nmap: SYN, connect, UDP, FIN, ping sweep, `-T`), użytkowników, grup, udziałów, WWW; Scapy; sniffing (Wireshark, tcpdump).
3. **Skanowanie podatności** – typowy skaner w czterech krokach: **wykrywanie hostów i portów → identyfikacja oprogramowania i wersji → korelacja z bazą znanych luk → raport**.

### Rodzaje skanów podatności (W5, kurs Cisco)

| Rodzaj | Opis |
| :--- | :--- |
| **bez uwierzytelniania** | perspektywa zdalnego atakującego; widoczne tylko usługi sieciowe |
| **uwierzytelnione** | poświadczenia (SSH/admin): pełniejszy obraz powierzchni ataku, **mniej fałszywych alarmów** |
| **wykrywające (discovery)** | identyfikacja powierzchni ataku (porty, usługi, wersje) |
| **pełne (full)** | wszystkie opcje polityki (kategorie wtyczek: system, producent, protokół, zgodność, typ ataku) |
| **ukryte (stealth)** | minimalizacja szumu w środowisku produkcyjnym (np. SYN scan, rzadkie zapytania) |
| **pasywne** | analiza ruchu – topologia, usługi, wersje, bez sond |
| **zgodności (compliance)** | weryfikacja zgodności z politykami/regulacjami (np. HIPAA) |
| **inwazyjne/nieinwazyjne** | z próbą wykorzystania luk (ryzyko awarii) / bez |

**Uwarunkowania skanowania (W5):** najlepszy czas (produkcja!), znajomość **protokołów** (TCP i UDP), **topologia** (nie skanować przez WAN), **ograniczenia przepustowości**, **ograniczanie zapytań (throttling – mniej wątków)**, **kruche systemy** (np. drukarki, urządzenia przemysłowe).

**Narzędzia:** Nmap/Masscan, Nessus, OpenVAS/Greenbone (GVM), Nikto, Burp/ZAP (WWW), Metasploit, Wireshark, Scapy.

## Etap 3 – Atak (eksploatacja i post-eksploatacja)

Weryfikacja podatności przez **wykorzystanie** (W5: *„najlepszym sposobem weryfikacji wyników skanowania jest wykorzystanie podatności"*; moduł Metasploit oznacza zwykle poważną lukę), **eskalacja uprawnień**, **ruch boczny**, utrwalenie dostępu, dostęp do danych jako dowód wpływu; zasady: **tylko w ramach zakresu**, bez działań destrukcyjnych, natychmiastowe zgłoszenie aktywnej kompromitacji (W5), dokumentacja kroków, sprzątanie po teście.

## Etap 4 – Analiza wyników i raportowanie (W5, kurs Cisco)

- **Analiza wyników skanów** jest trudniejsza niż samo skanowanie; konieczna **weryfikacja i eliminacja fałszywych alarmów** (sprawdzenie wersji, CVE, ręczne potwierdzenie).
- **Źródła informacji o lukach (W5):** **CVE** (identyfikatory), **CWE** (typowe słabości), **CVSS** (ocena 0–10: grupy Base/Temporal/Environmental, wektor), **NVD (NIST)**, **CAPEC** (wzorce ataków), US-CERT, CERT/CC, JPCERT.
- **Priorytetyzacja (W5):** znaczenie luki, liczba dotkniętych systemów, sposób wykrycia (skaner/ręcznie), **wartość i krytyczność urządzenia**, wektor ataku w danym środowisku, **istnienie obejścia**; zasada: **najpierw systemy krytyczne i luki o najwyższym stopniu zagrożenia i prawdopodobieństwie**, następnie liczba systemów; aktywna kompromitacja – natychmiastowe zgłoszenie.
- **Raport** (W4): adresowany do właściwej grupy (CISO, ISM, CIO, zespoły techniczne), z ustaleniami, **dowodami, oceną ryzyka (CVSS), rekomendacjami i terminami naprawy**, zastrzeżeniami (stan na datę, brak gwarancji); streszczenie dla kierownictwa + część techniczna; **retest** po naprawach.

# CZĘŚĆ C. Metody testowania bezpieczeństwa sieci (przegląd)

Z kursu Cisco i W3–W5:

| Metoda | Opis |
| :--- | :--- |
| **Test penetracyjny** | symulacja ataku z wykorzystaniem luk (etyczny hacking) |
| **Skanowanie sieci** | ping, skan portów, odkrywanie usług (Nmap, SuperScan) |
| **Skanowanie podatności** | automatyczne wykrywanie luk i błędów konfiguracji |
| **Łamanie haseł** | wykrywanie słabych haseł (L0phtCrack, hashcat) |
| **Przegląd logów** | analiza logów zabezpieczeń, SIEM |
| **Kontrola integralności** | wykrywanie zmian (Tripwire) |
| **Wykrywanie wirusów/malware** | antymalware |
| **Test zapór i IPS** *(uzup.)* | weryfikacja reguł, omijanie filtrów (hping, Scapy, Nmap), test ewazji IPS |
| **Testy segmentacji** *(uzup.)* | czy ruch między strefami jest zgodny z polityką |
| **Testy sieci bezprzewodowych** | rogue AP, łamanie szyfrowania, deauth, MITM; *war-driving* (starsze) |
| **Testy DoS/odporności** *(uzup.)* | kontrolowane, w ustalonych oknach |
| **Testy VPN i dostępu zdalnego** *(uzup.)* | konfiguracja IPsec/SSL, uwierzytelnianie, ekspozycja |
| **Audyt konfiguracji** *(uzup.)* | zgodność z CIS Benchmarks |
| **Testy socjotechniczne i fizyczne** | phishing, vishing, tailgating, włamanie do obiektu |
| **ST&E (Security Test & Evaluation)** | ocena środków ochrony w sieci operacyjnej: wykrycie błędów projektu/wdrożenia/eksploatacji, adekwatność mechanizmów, spójność dokumentacji z implementacją; **okresowo i po zmianach** |
| **Ćwiczenia zespołów** | **red team** (atakuje, niezauważony), **blue team** (broni), **white team** (reguły, sędzia), **purple team** (współpraca) |
| **Bug bounty** *(uzup.)* | programy zgłaszania luk |

### Rodzaje ćwiczeń i procesów

- **Vulnerability assessment vs pentest:** pierwsza **identyfikuje** (szeroko, automatycznie), drugi **wykorzystuje** (głęboko, ręcznie) – kurs Cisco, Moduł 4.
- Testy **uwierzytelnione/nieuwierzytelnione**, **zewnętrzne/wewnętrzne** (z sieci Internet vs z wnętrza po podpięciu do LAN lub z przejętej stacji).

## Dobre praktyki

- **Pisemna zgoda i jasny zakres**, kontrola scope creep,
- **lab testowy** i znajomość narzędzi przed użyciem u klienta (W3),
- **ostrożność w produkcji** (czas, tempo, kruche systemy),
- **walidacja wyników** i eliminacja fałszywych alarmów,
- **bezpieczne przechowywanie danych** z testu i usunięcie po zakończeniu,
- **raport rozumiany przez odbiorców** + plan naprawczy i retest,
- **cykliczność** (np. roczny pentest, kwartalne skany, test po zmianach) i integracja z zarządzaniem ryzykiem i ISMS; **spełnienie wymogów regulacji** (PCI DSS, NIS2 – audyt, RODO).

## Podsumowanie

- **Cel:** znaleźć luki przed napastnikami i zweryfikować działanie zabezpieczeń; test jest **pomiarem w punkcie czasowym**, powtarzanym regularnie.
- **Metodyki:** **NIST SP 800-115, PTES (7 faz), OSSTMM, ISSAF, MITRE ATT&CK, OWASP WSTG**; środowiska: infrastruktura, aplikacje, chmura, fizyczne, socjotechniczne; perspektywy: **unknown/partially known/known environment**.
- **Etapy:** **planowanie** (zakres, RoE, SOW/MSA/NDA, zgody, zgodność) → **odkrywanie** (rozpoznanie pasywne i aktywne, skanowanie podatności) → **atak** (eksploatacja, eskalacja, ruch boczny, persistence) → **raportowanie** (analiza, CVSS/CVE/CWE, priorytety, rekomendacje, retest).
- **Metody:** pentest, skanowanie sieci i podatności, łamanie haseł, przegląd logów, kontrola integralności, testy zapór/IPS/VPN/segmentacji/Wi-Fi, socjotechnika, ST&E, ćwiczenia red/blue/white/purple team.
