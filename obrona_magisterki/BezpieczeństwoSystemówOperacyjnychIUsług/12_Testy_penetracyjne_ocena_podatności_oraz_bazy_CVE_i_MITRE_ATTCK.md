# Testy penetracyjne, ocena podatności oraz bazy CVE i MITRE ATT&CK

> Wykład: **W3** (wstęp do etycznego hackingu, metodyki, laboratorium), **W4** (planowanie i zakres), **W5** (rozpoznanie, skanowanie, analiza wyników, CVE/CWE/CAPEC/CVSS), W2 (Kali Linux, Nmap), kurs Cisco z przedmiotu *Ochrona sieci dostępowych* (CVSS 3.1, NVD, fazy testu). Pełniejszy opis procesu testowania – folder *Bezpieczeństwo sieci teleinformatycznych*, temat 10; tu – **aspekt systemów operacyjnych i usług** oraz bazy wiedzy. Uzupełnienia – ***(uzupełnienie)***.

## 1. Etyczny hacking i test penetracyjny (W3)

**Etyczny haker** działa jak atakujący, aby ocenić stan bezpieczeństwa sieci/systemu, **zidentyfikować i ewentualnie wykorzystać luki** i ustalić, czy kompromitacja jest możliwa. Różnica etyczny/nieetyczny: **intencja i zgoda**; **zakres testu (scope)** – co wolno testować – jest kluczowy. Używa tych samych narzędzi co atakujący, ale **unika działań destrukcyjnych**, odpowiedzialnie zgłasza luki.

**Po co (W3):** sama obecność zapór, IPS, antywirusa, VPN, WAF **nie wystarcza – trzeba sprawdzić, czy działają**, i robić to **regularnie**, bo systemy i powierzchnia ataku się zmieniają.

**Aktorzy zagrożeń (W3):** zorganizowana przestępczość (zysk), hacktywiści, podmioty państwowe, insiderzy.

### Metodyki i standardy (W3, slajdy 13–14)

**MITRE ATT&CK**, **OWASP WSTG**, **NIST SP 800-115**, **OSSTMM**, **PTES** (7 faz: interakcje przedwstępne, zbieranie informacji, modelowanie zagrożeń, analiza podatności, eksploatacja, post-eksploatacja, raportowanie), **ISSAF**.

### Typy testów (W3, W4)

Perspektywa: **unknown-environment** (dawniej black-box), **partially known** (gray-box), **known-environment** (white-box). Środowiska: **infrastruktura sieciowa**, **aplikacje**, **chmura**, **fizyczne**, **socjotechniczne**.

### Planowanie (W4)

**Reguły zaangażowania (RoE)**, pisemna zgoda, **SOW/MSA/NDA/SLA**, lista docelowa (allow list), kontakty awaryjne, zgodność z przepisami (**PCI DSS, RODO, HIPAA**…), **scope creep**, bezpieczny transfer wyników (SCP/SFTP, PGP), ochrona poufności, ograniczenia narzędzi (nieinwazyjne w produkcji); lab testowy przed użyciem narzędzi u klienta (**Kali/Parrot**, VM, snapshoty – W3 slajdy 15–25).

### Fazy

**Planowanie → odkrywanie → atak → raportowanie** (Cisco/NIST), rozszerzone w PTES; **red/blue/white/purple team**.

## 2. Zbieranie informacji (W5)

**Rozpoznanie jest pierwszym krokiem cyberataku**:

- **pasywne** – bez interakcji z celem: **DNS, Whois, OSINT, Recon-ng, Shodan, certyfikaty i Certificate Transparency (crt.sh), wycieki haseł (h8mail), metadane (ExifTool), Google dorks/GHDB, Wayback Machine, repozytoria kodu**,
- **aktywne** – sondy do celu: **Nmap** (skany SYN `-sS`, connect `-sT`, UDP `-sU`, FIN `-sF`, `-sn`, `-sV`, `-sC`, `-O`, `-T0…-T5`), enumeracja hostów, **użytkowników, grup, udziałów SMB**, stron WWW (Nikto, `http-enum`), usług, **Scapy** (tworzenie pakietów), **Wireshark/tcpdump** (podsłuch).

**Dla systemów operacyjnych** – co zwykle sprawdza się w ocenie hosta *(uzupełnienie, na poziomie koncepcyjnym)*: poziom poprawek i wersje usług, otwarte porty i usługi, **domyślne/słabe hasła**, błędy konfiguracji (udziały SMB, RDP, SMBv1, SNMP `public`, anonimowe FTP, SSH z hasłem i rootem), **uprawnienia** (pliki SUID, `sudo -l`, słabe uprawnienia usług, niecytowane ścieżki usług Windows), polityka haseł i lockout, szyfrowanie dysku, logowanie i audyt, AV/EDR; zgodność z **CIS Benchmarks** (OpenSCAP, Lynis).

## 3. Skanowanie podatności (W5, slajdy 81–97)

Skaner (Nessus, OpenVAS/Greenbone, Nikto): **4 kroki** – wykrywanie hostów i portów (Nmap) → identyfikacja oprogramowania i wersji → dopasowanie do znanych podatności → raport.

| Rodzaj skanu | Opis |
| :--- | :--- |
| **bez uwierzytelniania** | perspektywa zdalnego atakującego, tylko usługi widoczne w sieci |
| **uwierzytelniony** | poświadczenia (SSH/admin) – pełniejszy obraz (np. `netstat` z uprawnieniami root pokazuje PID/programy), **mniej fałszywych alarmów** |
| **wykrywający (discovery)** | powierzchnia ataku |
| **pełny** | wszystkie opcje polityki (kategorie wtyczek) |
| **ukryty (stealth)** | minimalny szum w produkcji |
| **pasywny** | analiza ruchu |
| **zgodności** | zgodność z politykami/regulacjami (HIPAA, PCI) |

**Uwarunkowania (W5):** czas skanowania, znajomość protokołów (TCP **i UDP**), topologia, przepustowość, **ograniczanie zapytań**, **kruche systemy** (drukarki). **Główny problem – fałszywe alarmy**; wyniki wymagają weryfikacji.

## 4. Analiza wyników i priorytetyzacja (W5, slajdy 99–105)

Przeprowadzenie skanu jest najłatwiejsze; **główna praca to analiza**. Wyniki uwierzytelnione wiarygodniejsze niż zdalne; **najlepszy sposób weryfikacji – wykorzystanie podatności** (moduł w Metasploit zwykle = poważna luka). Pytania priorytetyzacji: znaczenie luki, **liczba systemów**, sposób wykrycia, **wartość i krytyczność urządzenia**, wektor ataku w danym środowisku, obejście/środek zaradczy. **Procedura:** najpierw luki o najwyższym zagrożeniu i największym prawdopodobieństwie; **aktywna kompromitacja – natychmiastowe zgłoszenie**; najpierw chronić systemy krytyczne.

## 5. Bazy wiedzy o podatnościach i atakach (W5, slajdy 100–102; kurs Cisco)

| Zasób | Opis |
| :--- | :--- |
| **CVE** (Common Vulnerabilities and Exposures) | słownik **unikalnych identyfikatorów** znanych podatności; format **`CVE-ROK-NUMER`** (rok + ≥ 4 cyfry), np. **CVE-2017-0144** (EternalBlue/SMBv1, MS17-010 – WannaCry), **CVE-2014-0160** (Heartbleed), **CVE-2014-6271** (Shellshock), **CVE-2021-44228** (Log4Shell); utworzony w **1999** (MITRE); identyfikatory przydzielają **CNA** |
| **NVD** (National Vulnerability Database, **NIST**) | baza oparta na CVE: **oceny CVSS**, szczegóły techniczne, dotknięte produkty, odnośniki |
| **CVSS** (Common Vulnerability Scoring System) | otwarty standard oceny dotkliwości (FIRST); **0–10**; grupy **Base/Temporal/Environmental**; wektor, np. `CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:L/I:L/A:N` = **3,8 (niski)**; skala: 0,1–3,9 niski, 4,0–6,9 średni, 7,0–8,9 wysoki, **9,0–10 krytyczny**; zdalne wykonanie kodu bez uwierzytelnienia `AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H` = **9,8** |
| **CWE** (Common Weakness Enumeration) | katalog **typów słabości** (np. CWE-89 SQL Injection, CWE-79 XSS, CWE-787 zapis poza buforem) |
| **CAPEC** (Common Attack Pattern Enumeration and Classification) | katalog **wzorców ataków** (MITRE) – słownik znanych ataków |
| **US-CERT/CISA, CERT/CC, JPCERT, CSIRT NASK** | biuletyny i koordynacja ujawniania |
| *(uzup.)* **CISA KEV** | katalog podatności **aktywnie wykorzystywanych** (priorytet naprawy) |
| *(uzup.)* **EPSS** | prawdopodobieństwo wykorzystania (uzupełnia CVSS) |
| *(uzup.)* biuletyny producentów (Microsoft **Patch Tuesday**, Red Hat/Ubuntu USN) | poprawki |

**Powiązanie:** **CWE** (słabość w kodzie) → **CVE** (konkretna podatność w produkcie) → **CVSS** (dotkliwość) → **CAPEC/ATT&CK** (jak się ją wykorzystuje). Priorytet = CVSS × krytyczność zasobu × aktywność exploita (KEV/EPSS) × ekspozycja.

## 6. MITRE ATT&CK

**ATT&CK** (Adversarial Tactics, Techniques & Common Knowledge) – **baza wiedzy o taktykach, technikach i procedurach (TTP) przeciwników** oparta na obserwacjach z rzeczywistych ataków (W3: używana przez pentesterów, red teamy, **zespoły reagowania na incydenty i threat hunting**; macierze **Enterprise, Network, Cloud, ICS, Mobile**; od zbierania informacji (OSINT) po eksploatację i post-eksploatację).

**Struktura:** **taktyka** (cel – *dlaczego*) → **technika** (*jak*) → **podtechnika** → procedury, grupy, oprogramowanie, środki zaradcze i detekcje.

### 14 taktyk Enterprise *(uzupełnienie)*

| ID | Taktyka | Przykład techniki |
| :-: | :--- | :--- |
| TA0043 | **Reconnaissance** (rozpoznanie) | zbieranie informacji o celu, skanowanie |
| TA0042 | **Resource Development** | przygotowanie infrastruktury, kont |
| TA0001 | **Initial Access** | phishing (T1566), exploit usługi publicznej (T1190), konta (T1078) |
| TA0002 | **Execution** | interpreter poleceń/skryptów (T1059) |
| TA0003 | **Persistence** | autostart (T1547), zadania zaplanowane (T1053), usługi |
| TA0004 | **Privilege Escalation** | exploity, nadużycie uprawnień |
| TA0005 | **Defense Evasion** | wyłączanie zabezpieczeń, obfuskacja |
| TA0006 | **Credential Access** | zrzut poświadczeń (T1003), brute force (T1110) |
| TA0007 | **Discovery** | enumeracja kont, sieci, procesów |
| TA0008 | **Lateral Movement** | usługi zdalne (T1021: RDP/SMB/SSH) |
| TA0009 | **Collection** | zbieranie danych |
| TA0011 | **Command and Control** | komunikacja z C2 (DNS, HTTPS) |
| TA0010 | **Exfiltration** | wyprowadzanie danych |
| TA0040 | **Impact** | szyfrowanie (ransomware), usuwanie, DoS |

**Zastosowania ATT&CK:** mapowanie incydentów (temat 11), **inżynieria detekcji** (które techniki widzimy, a które nie), planowanie testów **red/purple team**, ocena pokrycia zabezpieczeń, threat intelligence i raporty, szkolenie SOC (wykład: *szkolenia z ATT&CK*). Dla OS: **ATT&CK ma osobne techniki dla Windows i Linuksa** (np. T1547 autostart – rejestr `Run` vs `systemd`/`cron`; T1003 – LSASS vs `/etc/shadow`).

## 7. Narzędzia (W2–W5)

**Kali Linux/Parrot** (dystrybucje z narzędziami – W2, slajd 8; W3), **Nmap/Zenmap**, Masscan, **Nessus, OpenVAS/Greenbone**, **Nikto**, **Metasploit**, **Burp Suite/OWASP ZAP**, Wireshark/tcpdump, Scapy, **Recon-ng**, **Shodan**, SpiderFoot, h8mail, ExifTool, enum4linux, Hydra/hashcat (łamanie haseł, audyt polityk), **SET** (socjotechnika).

## 8. Raportowanie i ciągłość

Raport: streszczenie dla kierownictwa + część techniczna; **ustalenia z dowodami**, **ocena (CVSS)**, **rekomendacje, terminy, plan naprawczy**, zastrzeżenia (**test jest oceną w punkcie czasowym**); retest po naprawach. Powiązanie z **zarządzaniem podatnościami** (odkrywanie → priorytetyzacja → ocena → raport → naprawa → weryfikacja), regularność (np. roczny pentest, kwartalne skany), wymogi (PCI DSS, NIS2, ISO 27001).

## Podsumowanie

- **Test penetracyjny** = autoryzowana symulacja ataku (etyczny hacking); metodyki **PTES, NIST SP 800-115, OSSTMM, ISSAF, ATT&CK, OWASP WSTG**; fazy planowanie–odkrywanie–atak–raportowanie; ścisły **zakres i zgoda**.
- **Ocena podatności**: rozpoznanie pasywne/aktywne, skanery (uwierzytelnione/nieuwierzytelnione…), **weryfikacja fałszywych alarmów**, priorytetyzacja wg krytyczności i prawdopodobieństwa wykorzystania.
- **Bazy:** **CVE** (identyfikatory), **NVD** (oceny), **CVSS** (0–10), **CWE** (słabości), **CAPEC** (wzorce ataków), KEV/EPSS; **MITRE ATT&CK** – taktyki i techniki przeciwnika (14 taktyk Enterprise) – używane w testach, detekcji, threat huntingu i analizie incydentów.

---
[⬅️ Poprzedni temat](11_Proces_reagowania_na_incydenty_i_podstawy_analizy_powłamaniowej.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../PrzygotowanieIPublikowanieArtykułówNaukowych/PrzygotowanieIPublikowanieArtykułówNaukowych_tytul.md)