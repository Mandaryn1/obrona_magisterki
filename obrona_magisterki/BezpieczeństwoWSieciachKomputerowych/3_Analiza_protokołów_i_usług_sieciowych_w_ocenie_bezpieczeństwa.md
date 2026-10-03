# Analiza protokołów i usług sieciowych w ocenie bezpieczeństwa sieci

## Dlaczego analizować protokoły i usługi

Analiza protokołów i usług polega na sprawdzeniu, jakie protokoły i usługi działają w sieci i czy są bezpieczne. Większość protokołów sieciowych powstała, gdy zakładano **zaufanie do uczestników sieci** – bez uwierzytelniania i szyfrowania. Każda **aktywna usługa i otwarty port** zwiększa powierzchnię ataku. 

Ocena bezpieczeństwa polega na: 
1. wykryciu, jakie usługi i protokoły działają, 
2. zrozumieniu ich słabości, 
3. sprawdzeniu konfiguracji, wersji i poziomu szyfrowania, 
4. zastąpieniu protokołów niebezpiecznych bezpiecznymi.

## Metoda analizy

- **Inwentaryzacja**: które usługi i porty są otwarte (Nmap, netstat/ss).
- **Analiza ruchu**: jakie protokoły faktycznie przepływają i czy dane idą jawnym tekstem (Wireshark, tcpdump, NetFlow).
- **Ocena konfiguracji**: szyfrowanie, uwierzytelnianie, wersje, domyślne ustawienia.
- **Sprawdzenie znanych podatności** (CVE) i poziomu aktualizacji.
- **Rekomendacje**: wyłączenie, zastąpienie bezpieczniejszą wersją, ograniczenie dostępu.

## Przegląd protokołów i ich słabości

### Warstwa 2 i 3

| Protokół | Słabość | Ataki | Zabezpieczenie |
| :--- | :--- | :--- | :--- |
| **ARP** | brak uwierzytelniania odpowiedzi | **ARP poisoning/spoofing** (MITM) – wykład | DAI, statyczne wpisy, segmentacja |
| **DHCP** | brak uwierzytelniania serwera | **rogue DHCP**, starvation, podsunięcie bramy/DNS – wykład | DHCP snooping |
| **STP** | ufa BPDU | przejęcie roli root bridge | BPDU Guard, Root Guard |
| **CDP/LLDP** | ujawnia dane urządzeń | rozpoznanie sieci | wyłączenie na portach brzegowych |
| **IP** | adres źródłowy łatwy do sfałszowania | **IP spoofing**, amplifikacja – wykład | filtrowanie wejściowe, uRPF |
| **ICMP** | diagnostyczny, nadużywany | **ICMP flood, Ping of Death, tunelowanie ICMP** – wykład; smurf, redirect | ograniczanie i filtrowanie typów, rate limit |
| **RIP, OSPF, BGP** | domyślnie bez uwierzytelniania | fałszywe trasy, przejęcie prefiksów | uwierzytelnianie, filtry prefiksów, RPKI |

### Warstwa 4

| Protokół | Cecha | Ataki (wykład, slajd 30) |
| :--- | :--- | :--- |
| **TCP** | połączeniowy, **trójetapowe uzgadnianie** (SYN, SYN-ACK, ACK) | **SYN flood**, przejęcie sesji (**TCP hijacking** przez przewidzenie numerów sekwencyjnych), ACK storms; skanowanie portów |
| **UDP** | bezpołączeniowy, best-effort, bez uzgadniania | **UDP flood**, **ataki amplifikacyjne (DNS, NTP, memcached)**, spoofing źródła |

### Warstwa 7 (usługi)

| Usługa | Problem | Bezpieczniejsza alternatywa |
| :--- | :--- | :--- |
| **Telnet** | hasła i dane **jawnym tekstem** | **SSH** |
| **FTP** | jawne dane i hasła | **SFTP / FTPS** |
| **HTTP** | brak szyfrowania, podatny na MITM | **HTTPS (TLS 1.2/1.3)**, HSTS |
| **SMTP/POP3/IMAP (bez TLS)** | jawne, relay, spoofing nadawcy | TLS, **SPF, DKIM, DMARC** (wykład, slajd 34) |
| **DNS** | brak uwierzytelniania, UDP | **DNS poisoning/cache poisoning**, amplifikacja, tunelowanie DNS, → **DNSSEC**, filtrowanie DNS, DoT/DoH, ograniczenie rekursji |
| **SNMP v1/v2c** | „community string" jawnie, często domyślny (`public`) | **SNMPv3** (uwierzytelnienie + szyfrowanie) |
| **SMB v1** | luki (**EternalBlue, MS17-010** – WannaCry) | wyłączenie SMBv1, SMB 3 z szyfrowaniem |
| **RDP** | brute force, podatności, wystawienie do Internetu | VPN, NLA, MFA, bramy RDP |
| **SSH** | brute force, słabe klucze | klucze, wyłączenie logowania root i hasłem, fail2ban |
| **LDAP (bez TLS)** | jawny | LDAPS / StartTLS |
| **NTP** | amplifikacja (monlist) | aktualizacja, ograniczenia |
| **TLS/SSL** | stare wersje (SSLv3, TLS 1.0/1.1), słabe szyfry, wygasłe certyfikaty | TLS 1.2+, silne zestawy szyfrów, kontrola certyfikatów |
| **Bazy danych, usługi administracyjne** | wystawione na świat | segmentacja, zapora, brak publicznego dostępu |

## Co analizować w pojedynczym połączeniu

1. **Czy jest szyfrowane?**: HTTP vs HTTPS; w Wireshark treść jawna = ryzyko,
2. **Jakiej wersji protokołu użyto**: np. TLS 1.0, SMBv1, SNMPv2c,
3. **Czy strony są uwierzytelnione**: certyfikat serwera, MFA, kerberos,
4. **Czy ruch jest zgodny z oczekiwanym wzorcem**: nietypowe porty, wolumen, godziny,
5. **Czy usługa jest potrzebna** i **dostępna tylko dla właściwych segmentów**: reguły zapory.

## Wpływ szyfrowania na analizę

**Ruch szyfrowany (TLS)**: ponad 90% ruchu jest szyfrowane, co chroni dane, ale utrudnia inspekcję. Analizuje się wtedy metadane (np. fingerprint JA3) albo stosuje inspekcję TLS z uwzględnieniem prywatności.

Zasady:

- stosować tylko niezbędne usługi,
- zastępować protokoły niezabezpieczone wersjami szyfrowanymi,
- segmentować i filtrować ruch zaporą,
- aktualizować i monitorować.

## Podsumowanie

- Ocena bezpieczeństwa: **inwentaryzacja usług → analiza ruchu → przegląd konfiguracji i wersji → mapowanie podatności → rekomendacje**.
- Protokoły historyczne (Telnet, FTP, HTTP, SNMPv1/2c, SMBv1, ARP, DHCP, DNS) są **niebezpieczne z założenia** – zastępować bezpiecznymi (SSH, SFTP, HTTPS, SNMPv3, SMB 3) lub chronić mechanizmami sieciowymi.
- TCP/UDP/ICMP mają charakterystyczne ataki (SYN flood, amplifikacja, tunelowanie, hijacking) – znajomość protokołu pozwala pisać reguły zapory i sygnatury IDS.

---
[⬅️ Poprzedni temat](2_Rola_systemów_Windows_i_Linux_w_analizie_bezpieczeństwa_sieci.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Klasyfikacja_najważniejszych_rodzajów_ataków_sieciowych.md)