# Rodzaje firewalli oraz zasady tworzenia polityk i reguł bezpieczeństwa

> Opracowanie oparte na wykładzie *Zapory sieciowe i systemy wykrywania włamań* (slajdy 3–9, 24–25, 29, 31–32); uzupełnienia – ***(uzupełnienie)***.

## Czym jest zapora (firewall)

**Zapora sieciowa** to **bariera kontrolująca przepływ ruchu sieciowego według zdefiniowanych reguł bezpieczeństwa**, działająca na różnych warstwach modelu OSI (wykład, slajd 3). Oddziela strefy o różnym poziomie zaufania (Internet, DMZ, LAN, serwery) i egzekwuje politykę. Wykład: zapory i IDS/IPS stanowią **podstawowe warstwy zabezpieczeń infrastruktury**.

## Historia (wykład, slajd 4)

- **lata 80.** – proste filtry pakietów (nagłówki warstwy sieciowej),
- **lata 90.** – **zapory stanowe** monitorujące stan połączeń, rozwój IDS,
- **lata 2000.** – zapory **aplikacyjne** i platformy **UTM**,
- **obecnie** – **NGFW** z AI, deep packet inspection i threat intelligence.

## Rodzaje zapór

### 1. Filtracja pakietów (packet filtering) – wykład, slajd 6

| | |
| :--- | :--- |
| **Zasada** | analiza **nagłówka każdego pakietu** niezależnie; porównanie z regułami (warstwy 3–4) |
| **Kryteria** | adresy IP źródłowe i docelowe, **porty TCP/UDP**, typ protokołu (TCP, UDP, ICMP), **flagi TCP** |
| **Zalety** | wysoka wydajność, niska latencja, prosta konfiguracja |
| **Wady** | **brak analizy zawartości**, podatność na ataki aplikacyjne, brak pamięci stanu (trudna obsługa ruchu powrotnego), nie wykrywa złożonych zagrożeń |
| **Przykłady** | ACL na routerach, proste reguły systemowe |

### 2. Zapory stanowe (stateful inspection) – wykład, slajd 7

| | |
| :--- | :--- |
| **Zasada** | **tabela stanów** aktywnych sesji (fazy nawiązywania i kończenia połączeń TCP); decyzja zależy od **kontekstu połączenia** |
| **Dynamiczne reguły** | automatyczne tworzenie tymczasowych reguł dla **odpowiedzi** na zatwierdzone żądania (brak ręcznych reguł powrotnych) |
| **Zalety** | lepsza ochrona niż samo filtrowanie, odrzuca pakiety niezgodne ze stanem (np. nieoczekiwane ACK), wygodna konfiguracja |
| **Wady** | więcej zasobów (tabela stanów, podatność na wyczerpanie – SYN flood), nadal ograniczona analiza aplikacyjna |
| **Przykłady (wykład)** | **Cisco PIX, Check Point FireWall-1, iptables z modułem conntrack**, Windows Defender Firewall, pfSense |

### 3. Zapory na poziomie obwodu (circuit-level gateway) *(uzupełnienie)*

Kontrolują **ustanowienie sesji** (np. TCP handshake) i przekazują strumień bez analizy treści (np. SOCKS). Skuteczne w weryfikacji legalności sesji, nie treści.

### 4. Zapory aplikacyjne i proxy (Application Layer Firewalls) – wykład, slajd 8

| | |
| :--- | :--- |
| **Zasada** | **głęboka inspekcja ruchu na poziomie protokołów aplikacyjnych (warstwa 7)**; często **pośrednik (proxy)** – klient łączy się z zaporą, zapora z serwerem |
| **Możliwości (wykład)** | inspekcja **HTTP/HTTPS**, polecenia **FTP i SMTP**, zapytania **DNS**, protokoły baz danych |
| **Wykrywane zagrożenia** | **SQL Injection, XSS, command injection, directory traversal** |
| **Przykłady (wykład)** | **ModSecurity (WAF), F5 BIG-IP ASM, Cloudflare WAF** |
| **Zalety** | zrozumienie protokołu, blokowanie ataków aplikacyjnych, ukrywanie serwerów |
| **Wady** | większe opóźnienia i koszt, wymaga rozpoznania każdego protokołu |

**WAF (Web Application Firewall)** – wyspecjalizowana zapora aplikacji WWW (OWASP Top 10): wykrywa wzorce SQL (UNION, SELECT, DROP), tagi `<script>`, niebezpieczne znaki (; | &), normalizuje ścieżki (wykład, slajd 31).

### 5. Zapory nowej generacji (NGFW) – wykład, slajd 9

Łączą funkcje **tradycyjnego firewalla** z zaawansowanymi:

- **IDS/IPS** (wykrywanie i blokowanie sygnatur ataków),
- **kontrola aplikacji** – identyfikacja i zarządzanie ruchem aplikacyjnym **niezależnie od portu i protokołu**,
- **antywirus/antymalware** na poziomie sieci,
- **inspekcja SSL/TLS** (analiza ruchu szyfrowanego),
- **głęboka inspekcja pakietów**, filtrowanie URL, **świadomość użytkownika i tożsamości**, threat intelligence, sandboxing.

Producenci (wykład): **Palo Alto Networks, Fortinet FortiGate, Cisco Firepower**.

### 6. UTM – Unified Threat Management – wykład, slajd 32

Jedno urządzenie integrujące: zaporę stanową, **IDS/IPS**, bramę antywirusową, **VPN**, filtrowanie treści WWW, kontrolę aplikacji i QoS, zabezpieczenie poczty (antyspam/antyphishing), **DLP**. Zalety: jeden panel, niższy TCO dla MŚP, łatwa integracja; wady: **pojedynczy punkt awarii**, ograniczona skalowalność, spadek wydajności przy pełnej inspekcji, vendor lock-in. Przykłady: FortiGate, SonicWall, WatchGuard, Sophos XG.

### 7. Zapory hostowe (host-based) i rozproszone *(uzupełnienie)*

Na pojedynczym hoście (Windows Defender Firewall z profilami domenowy/prywatny/publiczny zarządzanymi GPO, iptables/nftables, firewalld, ufw – wykład, slajd 25); **mikrosegmentacja** i zapory w chmurze (**Security Groups, NSG**, zapory kontenerowe).

### Poziom wykonania

| Typ | Przykłady (wykład, slajd 25) |
| :--- | :--- |
| **sprzętowe/korporacyjne** | **Cisco ASA** (inspekcja stanowa, VPN, klastrowanie i HA; z modułem Firepower – IPS i ochrona przed malware), Palo Alto, Fortinet, Check Point |
| **open source/programowe** | **pfSense** (oparty na FreeBSD; VLAN, VPN, load balancing, captive portal; pakiety Snort, Suricata, pfBlockerNG), **iptables/nftables** (nftables – nowoczesny następca: uproszczona składnia, wyższa wydajność) |
| **wbudowane w system** | **Windows Defender Firewall** |

## Porównanie

| Typ | Warstwa | Wiedza o stanie | Treść aplikacji | Wydajność | Ochrona |
| :--- | :-: | :-: | :-: | :-: | :--- |
| Filtr pakietów | 3–4 | nie | nie | bardzo wysoka | podstawowa |
| Stanowa | 3–4 | tak | nie | wysoka | dobra |
| Proxy/aplikacyjna (WAF) | 7 | tak | tak | średnia | wysoka dla aplikacji |
| **NGFW** | 3–7 | tak | tak (DPI, aplikacje, tożsamość) | średnia | **kompleksowa** |
| UTM | 3–7 | tak | tak | średnia | kompleksowa (MŚP) |

## Polityka bezpieczeństwa a reguły zapory

**Polityka bezpieczeństwa sieci** (dokument wysokiego poziomu: co jest dozwolone, między jakimi strefami, dla kogo) jest **źródłem** reguł zapory. Reguły to jej techniczna realizacja. Wykład: problemy w praktyce to **fragmentacja polityk** (setki reguł NGFW i tysiące sygnatur IPS) i potrzeba **zarządzania cyklem życia polityk**, automatycznej kontroli zgodności i scentralizowanych platform (np. **Panorama, FortiManager** – slajd 29).

### Elementy reguły

| Pole | Opis |
| :--- | :--- |
| **Strefa/interfejs źródłowy i docelowy** | skąd/dokąd (Internet, DMZ, LAN, Serwery) |
| **Adres źródłowy/docelowy** | hosty, podsieci, **obiekty i grupy obiektów**, FQDN |
| **Usługa/port/protokół** lub **aplikacja** | np. TCP 443, `ssl`, `ms-rdp` |
| **Użytkownik/grupa** | integracja z AD/ISE (NGFW) |
| **Akcja** | **allow, deny/drop, reject**, inspect (z profilem IPS/AV/URL) |
| **Profile bezpieczeństwa** | IPS, antymalware, filtr URL, DLP |
| **Logowanie** | log początku/końca sesji, alerty |
| **Harmonogram** | okna czasowe |
| **Komentarz/właściciel/ticket** | uzasadnienie, odpowiedzialny, data wygaśnięcia |

### Zasady tworzenia polityk i reguł

1. **Domyślna odmowa (default deny)** – wszystko zabronione, co nie jest jawnie dozwolone; na końcu reguła *cleanup/deny all* z logowaniem.
2. **Zasada najmniejszych uprawnień** – możliwie wąskie źródło, cel i usługa; **unikanie `any`**.
3. **Kolejność reguł (first match)** – reguły są oceniane od góry; szczególne przed ogólnymi; reguły antyspoofingowe i stealth na początku; **unikaj reguł „przesłoniętych" (shadowed)**.
4. **Reguły dwukierunkowe świadomie** – ruch **przychodzący (ingress)** i **wychodzący (egress)**; filtrowanie wychodzące ogranicza C2 i eksfiltrację.
5. **Segmentacja** – reguły między strefami zgodne z modelem zaufania; **stref DMZ** nie ma dostępu do LAN poza wymaganym.
6. **Anti-spoofing** – blokada adresów prywatnych i bogon na interfejsie zewnętrznym, własnych adresów z zewnątrz.
7. **Obiekty i grupy** zamiast pojedynczych adresów – czytelność i łatwość zmian.
8. **Dokumentacja i właściciele** – każda reguła z uzasadnieniem i osobą odpowiedzialną; **zarządzanie zmianami** (wniosek, zatwierdzenie, test, wdrożenie, rollback).
9. **Regularny przegląd i recertyfikacja reguł** – usuwanie nieużywanych (analiza liczników trafień), wygasających (tymczasowych), zbyt szerokich; **audyt zgodności** (wykład: automatyczne sprawdzanie zgodności).
10. **Logowanie i monitoring** – logi do SIEM, alerty na odrzucenia z krytycznych stref, analiza trendów.
11. **Warstwowość** – zapora to jedna z warstw; uzupełnia IPS, WAF, EDR, NAC, segmentacja (obrona w głąb).
12. **Test przed wdrożeniem** i po (skan, test penetracyjny – temat 10); tryb *monitor-only/learning* przy nowych regułach.
13. **Utwardzenie samej zapory** – aktualizacje (urządzenia brzegowe są częstym celem), zarządzanie z sieci zarządzania, MFA dla administratorów, kopie konfiguracji, HA.
14. **Zgodność z normami** – ISO 27001 (A.8.20–8.22: bezpieczeństwo sieci, segregacja), PCI DSS (zapory i segmentacja), NIS2.

### Typowe błędy

zbyt szerokie reguły (`any any permit`), brak reguły końcowej z logiem, nieudokumentowane i stare reguły, brak filtrowania wychodzącego, domyślne hasła do zarządzania, otwarte zarządzanie z Internetu, brak przeglądu po zmianach, niepewność co do kolejności i przesłonięć, fragmentacja polityk (wiele urządzeń).

## Przykłady

### Reguła ACL (Cisco) *(uzupełnienie)*

```text
ip access-list extended INTERNET-IN
 deny   ip 10.0.0.0 0.255.255.255 any          ! anti-spoofing
 deny   ip 192.168.0.0 0.0.255.255 any
 permit tcp any host 203.0.113.10 eq 443        ! tylko HTTPS do reverse proxy
 permit icmp any any packet-too-big
 deny   ip any any log                          ! cleanup z logowaniem
```

### Polityka stref (matryca) *(uzupełnienie)*

| Z \ Do | Internet | DMZ | LAN | Serwery | Zarządzanie |
| :--- | :-: | :-: | :-: | :-: | :-: |
| **Internet** | – | 443 (reverse proxy) | ✗ | ✗ | ✗ |
| **DMZ** | wybrane (aktualizacje) | – | ✗ | tylko aplikacja→baza | ✗ |
| **LAN** | przez proxy 80/443 | ✗ | – | wybrane usługi | ✗ |
| **Serwery** | wybrane (aktualizacje) | ✗ | odpowiedzi | – | ✗ |
| **Zarządzanie** | ✗ | SSH/HTTPS | SSH | SSH | – |

## Podsumowanie

- **Rodzaje:** filtrujące pakiety (L3–4), **stanowe** (tabela stanów, dynamiczne reguły), obwodowe, **aplikacyjne/proxy i WAF** (L7: SQLi, XSS…), **NGFW** (IPS, kontrola aplikacji, antymalware, inspekcja TLS, tożsamość), **UTM**, hostowe; wykonania: Cisco ASA, Palo Alto, Fortinet, pfSense, iptables/nftables, Windows Firewall.
- **Polityka → reguły:** domyślna odmowa, najmniejsze uprawnienia, kolejność first-match, ingress i egress, anti-spoofing, obiekty i dokumentacja, zarządzanie zmianami, regularny przegląd i audyt, logowanie do SIEM, utwardzenie i HA, zgodność z normami.
- Wyzwania: fragmentacja polityk (zarządzanie cyklem życia, Panorama/FortiManager), wydajność i inspekcja TLS.

---
[⬅️ Poprzedni temat](5_Technologie_uwierzytelniania_i_autoryzacji_RADIUS_TACACS_plus_oraz_ISE.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Szczegółowa_inspekcja_pakietów_oraz_systemy_IDS_i_IPS_zadania_i_ograniczenia.md)