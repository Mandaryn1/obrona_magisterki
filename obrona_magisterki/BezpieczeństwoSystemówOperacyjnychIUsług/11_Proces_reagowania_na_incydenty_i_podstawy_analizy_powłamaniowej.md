# Proces reagowania na incydenty oraz podstawy analizy powłamaniowej

> Wykład: MBK1 (slajdy 49–50: audyt dostępu, zarządzanie incydentami, przykład z blokadą IP), W1 (Podgląd zdarzeń, netstat), W2 (logi, procesy, rootkity, kopie i punkty przywracania), W8 (slajd 31: analiza malware w piaskownicy; Fail2Ban), W5 web (slajd 18: SIEM i automatyczne blokady), kurs Cisco (SIEM/SOAR, cykl PDCA audytu). Fazy reagowania, forensics, aspekty prawne – głównie ***(uzupełnienie)***.

## Pojęcia

- **Zdarzenie (event)** – dowolna obserwowalna zmiana w systemie lub sieci; **incydent bezpieczeństwa** – zdarzenie (lub seria) naruszające lub zagrażające **poufności, integralności lub dostępności** albo polityce bezpieczeństwa.
- **Reagowanie na incydenty (Incident Response, IR)** – zorganizowany proces wykrycia, analizy, ograniczenia skutków, usunięcia przyczyny, odtworzenia i wyciągnięcia wniosków.
- **Analiza powłamaniowa (post-incident analysis / forensics)** – badanie, **co się stało, jak, kiedy, kto, jaki jest zakres i skutki**, wraz z zabezpieczeniem **dowodów**.

Wykład (MBK1, slajd 50): *nawet najlepsze mechanizmy bezpieczeństwa nie zapobiegną wszystkim incydentom; kluczem jest **szybkie wykrycie, odpowiednia reakcja i wyciągnięcie wniosków***.

## Obowiązki organizacji (wykład, slajd 50)

1. **rejestr prób naruszenia** – dokumentowanie incydentów: **kto, kiedy, skąd, co próbował zrobić**,
2. **automatyczne powiadamianie** administratorów o podejrzanej aktywności,
3. **procedury reakcji** dla różnych typów incydentów (z góry określone kroki),
4. **post-mortem** – analiza po opanowaniu: przyczyny i działania naprawcze.

**Przykład z wykładu:** wielokrotne nieudane logowania z tego samego IP → system **automatycznie blokuje IP**, loguje zdarzenie i powiadamia administratora; po incydencie – analiza, czy atak był celowany, aktualizacja polityk. Kurs (SIEM/SOAR): niskopoziomowe zdarzenia obsługuje automatyzacja bez udziału człowieka.

## Fazy reagowania (NIST SP 800-61 / SANS) *(uzupełnienie)*

```
 1. PRZYGOTOWANIE → 2. WYKRYCIE I ANALIZA → 3. OGRANICZANIE, USUNIĘCIE, ODTWORZENIE → 4. DZIAŁANIA PO INCYDENCIE
        ▲                                                                                              │
        └──────────────────────── wnioski, poprawa procedur i zabezpieczeń ◀─────────────────────────┘
```

Alternatywnie SANS **PICERL**: Preparation, Identification, Containment, Eradication, Recovery, Lessons learned. *(NIST opublikował w 2025 r. wersję 3 SP 800-61, dostosowaną do CSF 2.0 – sprawdź aktualny układ; sens faz pozostaje.)*

### 1. Przygotowanie

- **polityka i plan IR**, role i odpowiedzialności (zespół **CSIRT/IRT**, kierownik incydentu, IT, prawny, PR, zarząd), kontakty (w tym **CSIRT NASK/GOV/MON**),
- **narzędzia:** logowanie i **SIEM**, EDR, kopie zapasowe, środowisko do analizy (piaskownica – wykład W8, slajd 31), media do akwizycji dowodów, **playbooki**,
- **inwentarz aktywów**, **baseline**, synchronizacja czasu (NTP), kopie zapasowe i **punkty przywracania** (W2, laboratorium), szkolenia, **ćwiczenia (tabletop, red/blue team)**,
- odpowiednia konfiguracja audytu (SACL, auditd) *przed* incydentem.

### 2. Wykrywanie i analiza

**Źródła wykryć:** alerty IDS/IPS/EDR/AV, SIEM (korelacja), **logi** (Windows: Event Log **4624/4625/4670/4663**, 4688, 7045; Linux: `auth.log`/`secure`, auditd, `journalctl`), anomalie procesów i połączeń (`netstat`, `ps`, `ss`), zgłoszenia użytkowników, threat intelligence (IOC), skargi zewnętrzne.

**Triage i klasyfikacja:** czy to rzeczywisty incydent (odrzucenie fałszywych alarmów), **typ** (malware/ransomware, nieautoryzowany dostęp, DoS, wyciek danych, phishing, insider), **dotkliwość i priorytet** (krytyczność zasobu, zakres, wpływ na biznes i dane osobowe), **zakres** (które hosty, konta, dane), oś czasu. Dokumentacja od początku (**kto, co, kiedy, jak**).

### 3. Ograniczanie (containment), usunięcie (eradication), odtworzenie (recovery)

| Etap | Działania |
| :--- | :--- |
| **ograniczanie** | **izolacja** hosta/segmentu (odłączenie od sieci, kwarantanna, VLAN, blokada na zaporze), **blokada IP/kont**, wyłączenie konta/usługi, zmiana haseł i kluczy, zatrzymanie procesu; *krótkoterminowo* i *długoterminowo*; **uwaga: wyłączenie zasilania niszczy dane ulotne (RAM)** – decyzja zależy od celu (zachować dowody vs szybko zatrzymać) |
| **usunięcie przyczyny** | usunięcie malware i mechanizmów utrwalenia (autostart, usługi, zadania cron/Task Scheduler, konta), **załatanie podatności**, zamknięcie wektora wejścia; przy rootkicie jądra – **reinstalacja** (wykład W2, slajd 50) |
| **odtworzenie** | przywrócenie z **zaufanych kopii** (zweryfikowanych), ponowne uruchomienie usług, **zwiększony monitoring**, weryfikacja integralności, stopniowe przywracanie |

### 4. Działania po incydencie (post-mortem / lessons learned)

spotkanie w ciągu dni, **analiza przyczyn źródłowych (RCA)**, ocena skuteczności reakcji (MTTD/MTTR), aktualizacja polityk, reguł detekcji, utwardzenia i szkoleń, raport dla kierownictwa, (jeśli wymagane) **zgłoszenia do organów**.

## Analiza powłamaniowa (forensics) *(uzupełnienie)*

### Zasady

- **zachować dowody** i ich **integralność** – praca na **kopiach (obrazach)**, **sumy kontrolne** (SHA-256), **write blocker** dla dysków, nie modyfikować oryginału,
- **łańcuch dowodowy (chain of custody)** – kto, kiedy, co miał pod kontrolą (podstawa wartości dowodowej),
- **kolejność ulotności (order of volatility, RFC 3227):** rejestry/cache → **pamięć RAM** → stan sieci i procesów → dyski → logi zdalne → nośniki archiwalne (zbierać od najbardziej ulotnych),
- dokumentować każdy krok, minimalizować ingerencję, zasada **jedna analiza = jeden dokument**.

### Co zbierać i analizować

| Źródło | Windows | Linux |
| :--- | :--- | :--- |
| **pamięć (RAM)** | zrzut pamięci (WinPmem, DumpIt), analiza **Volatility** (procesy, połączenia, wstrzyknięty kod, hasła) | LiME, Volatility |
| **procesy i połączenia** | `tasklist`, `netstat -ano`, Process Explorer, Sysinternals | `ps auxf`, `ss -tulpn`, `lsof` |
| **logi** | **Security.evtx** (4624/4625/4672/4688/4720 – nowe konto/7045 – nowa usługa/1102 – wyczyszczenie logu), PowerShell logs, **Sysmon** | `/var/log/auth.log`/`secure`, `wtmp`/`btmp` (`last`, `lastb`), `journalctl`, **auditd** |
| **utrwalenie** | rejestr `Run/RunOnce`, usługi, Harmonogram zadań, WMI, foldery Startup | `cron`, `systemd` units, `~/.bashrc`, `authorized_keys`, `/etc/rc.local`, SUID |
| **system plików** | **MFT**, znaczniki czasu, **ADS**, Prefetch, Amcache/Shimcache, Shadow Copies, kosz | znaczniki czasu (mtime/ctime/atime), `find -newer`, journal |
| **konta i uprawnienia** | lokalne konta i grupy, Kerberos | `/etc/passwd`, `/etc/shadow`, `sudoers` |
| **sieć** | pcap (Wireshark), NetFlow, logi zapory/proxy/DNS | j.w. |

**Narzędzia:** Autopsy/The Sleuth Kit, FTK Imager, `dd`/`dc3dd` (obrazy dysków), Volatility, Wireshark, **Velociraptor**, KAPE, Plaso/log2timeline (**oś czasu**), YARA, ELK/SIEM.

### Schemat analizy

1. potwierdzenie i zakres (pierwsze wskaźniki: IOC – adresy IP, domeny, hashe, nazwy plików, klucze rejestru),
2. **oś czasu zdarzeń** (pierwsze wejście → eskalacja → ruch boczny → działania na danych),
3. **wektor początkowy** (phishing, podatność, słabe hasło/RDP, skradzione poświadczenia),
4. ustalenie **TTP** i mapowanie na **MITRE ATT&CK** (temat 12) oraz fazy łańcucha ataku (APT: rozpoznanie → pierwsze naruszenie → utrwalenie → eskalacja → ruch boczny → eksfiltracja),
5. **zakres szkód** (dane, konta, systemy), ocena czy dane osobowe wyciekły,
6. wyszukanie IOC w całej infrastrukturze (threat hunting),
7. rekomendacje i raport.

### Analiza malware

Statyczna (strings, hashe, PE/ELF) i **dynamiczna w izolowanym środowisku** (wykład W8, slajd 31: izolowana sieć, maszyny wirtualne, pełny monitoring wywołań systemowych, ruchu i zmian plików, **analiza behawioralna i wskazanie IoC**).

## Aspekty prawne i organizacyjne *(uzupełnienie)*

- **RODO:** zgłoszenie naruszenia ochrony danych osobowych organowi nadzorczemu (**UODO**) **w ciągu 72 h** od stwierdzenia, a gdy wysokie ryzyko – także osobom, których dane dotyczą; rejestr naruszeń,
- **NIS2/KSC:** wczesne ostrzeżenie **24 h**, zgłoszenie incydentu **72 h**, raport końcowy do miesiąca (do CSIRT; w Polsce nowelizacja KSC weszła w życie 3.04.2026),
- współpraca z organami ścigania (art. 267 k.k. i inne), dowody dopuszczalne, ochrona prywatności w trakcie analizy logów (RODO), komunikacja kryzysowa.

## Typowe scenariusze i reakcje

| Incydent | Reakcja |
| :--- | :--- |
| **brute force na SSH/RDP** (4625/`Failed password`) | blokada IP (fail2ban/zapora), zmiana haseł, MFA, ograniczenie ekspozycji (wykład) |
| **ransomware** | natychmiastowa izolacja, zachowanie próbki i RAM, **przywrócenie z kopii offline**, analiza wektora, zmiana poświadczeń domenowych |
| **rootkit** | analiza z zewnątrz (live USB), zrzut pamięci, reinstalacja |
| **wyciek danych** | zakres, powiadomienia (72 h), ograniczenie dostępu |
| **przejęte konto** | reset poświadczeń, unieważnienie sesji i tokenów, przegląd aktywności |

## Podsumowanie

- **Proces IR:** **przygotowanie → wykrywanie i analiza → ograniczanie, usuwanie, odtworzenie → działania po incydencie**; obowiązki: rejestr incydentów, automatyczne powiadomienia, procedury, post-mortem (wykład MBK1).
- **Forensics:** zabezpieczenie dowodów (kopie, hashe, łańcuch dowodowy, kolejność ulotności), analiza **logów, pamięci, procesów, utrwalenia, systemu plików**, **oś czasu**, IOC, zakres szkód; mapowanie na ATT&CK.
- Wymogi prawne: **RODO 72 h**, NIS2 24 h/72 h; wnioski → udoskonalenie zabezpieczeń.
