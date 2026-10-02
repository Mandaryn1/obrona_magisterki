# Audyt bezpieczeństwa, testy penetracyjne i ocena podatności w ochronie sieci dostępowych

> Opracowanie oparte głównie na kursie Cisco *Cyber Threat Management* (Moduł 2: ocena bezpieczeństwa, techniki testowania, narzędzia, testy penetracyjne; Moduł 4: testowanie podatności, zarządzanie podatnościami) oraz wykładzie o ryzyku (audyty); uzupełnienia – ***(uzupełnienie)***.

## Kontekst: po co testować i oceniać

Skuteczność rozwiązań bezpieczeństwa można sprawdzić **bez czekania na rzeczywiste zagrożenie** (kurs Cisco). **Bezpieczeństwo operacyjne** zaczyna się od planowania i wdrożenia; po uruchomieniu sieci dochodzi **ciągła konserwacja** i **okresowe testowanie**, bo nowe luki powstają stale (nowe usługi, zmiany, nowe podatności). Testy: **ręczne i zautomatyzowane**; personel musi znać systemy operacyjne, podstawy programowania, protokoły TCP/IP, luki i ich ograniczanie, zabezpieczanie urządzeń, zapory i IPS.

### ST&E (Security Test and Evaluation)

Po pełnym zintegrowaniu sieci przeprowadza się **test i ocenę bezpieczeństwa** – badanie środków ochronnych w sieci operacyjnej. Cele: **wykrycie błędów projektowych, wdrożeniowych i operacyjnych**; **ocena adekwatności mechanizmów** do egzekwowania polityki; **spójność dokumentacji z implementacją**. Powtarzane **okresowo i po każdej zmianie**; częściej dla systemów krytycznych.

## Trzy filary: ocena podatności, test penetracyjny, analiza ryzyka (kurs Cisco, Moduł 4)

| Termin | Opis | Narzędzia / wykonawcy |
| :--- | :--- | :--- |
| **Analiza ryzyka** | ocena ryzyka stwarzanego przez luki dla **konkretnej organizacji**: prawdopodobieństwo ataków, typy aktorów zagrożeń, wpływ udanych exploitów | konsultanci, ramy zarządzania ryzykiem |
| **Ocena podatności** (vulnerability assessment) | **skanowanie** serwerów WWW i sieci wewnętrznych w poszukiwaniu luk: nieznanych infekcji, słabości usług baz danych, brakujących poprawek, zbędnych portów itd. | OpenVAS, Microsoft Baseline Analyzer, **Nessus**, Qualys, **Nmap** |
| **Test penetracyjny** | **autoryzowane symulowane ataki** do sprawdzenia siły zabezpieczeń; nie tylko potwierdza luki, ale **wykorzystuje je**, aby określić potencjalny wpływ | Metasploit, CORE Impact, etyczni hakerzy |

**Ocena podatności ≠ test penetracyjny:** pierwsza **identyfikuje** potencjalne problemy (szeroko, głównie automatycznie, bez wykorzystania); drugi **wykorzystuje** podatności (głębiej, ręcznie), by pokazać realny skutek ataku.

## 1. Ocena podatności (skanowanie)

### Skanery podatności (kurs Cisco, Moduł 2)

**Skaner podatności** ocenia komputery, systemy, sieci lub aplikacje pod kątem słabych punktów i **automatyzuje audyt**, tworząc **listę priorytetów**. Szuka m.in.:

- haseł domyślnych lub wspólnych,
- brakujących aktualizacji,
- otwartych portów,
- błędnych konfiguracji systemów i oprogramowania,
- aktywnych adresów IP, w tym **nieoczekiwanych urządzeń**.

Funkcje: audyty zgodności, dostarczanie poprawek, błędne konfiguracje, obsługa urządzeń mobilnych i bezprzewodowych, śledzenie malware, identyfikacja wrażliwych danych. Narzędzia: **Nessus, Retina, Core Impact, GFI LanGuard**; wybór: dokładność, niezawodność, skalowalność, raportowanie; opcje programowe lub chmurowe.

### Rodzaje skanowania

| Kryterium | Typy |
| :--- | :--- |
| **Cel** | **sieciowe** (hosty, otwarte porty, użytkownicy i grupy, znane luki), **aplikacji** (przez kod źródłowy, od wewnątrz bez uruchamiania), **aplikacji internetowych** |
| **Wpływ** | **inwazyjne** (próbują wykorzystać luki, mogą spowodować awarię celu) vs **nieinwazyjne** |
| **Poświadczenia** | **uwierzytelnione** (nazwa i hasło – więcej informacji, **mniej** fałszywych alarmów) vs **bez poświadczeń** (punkt widzenia osoby postronnej) |

**Fałszywy alarm (false positive)** – wykrycie nieistniejącej luki; **wynik fałszywie ujemny (false negative)** – niewykrycie istniejącej. Uwierzytelnione skanery dają mniej obu.

### Rodzaje testów sieciowych (kurs Cisco)

| Test | Opis |
| :--- | :--- |
| **Testy penetracyjne** | symulacja ataków złośliwych źródeł; może obejmować socjotechnikę i dostęp do siedziby |
| **Skanowanie sieci** | ping, skan portów TCP, rodzaje zasobów; czasem nazwy użytkowników, grupy i udziały |
| **Skanowanie podatności** | błędne konfiguracje, puste/domyślne hasła, cele DoS; czasem próba „wywrócenia" systemu |
| **Łamanie haseł** | wykrywanie słabych haseł (np. L0phtCrack) – podstawa polityki haseł |
| **Przegląd logów** | filtrowanie dużych plików dziennika pod kątem nietypowej aktywności |
| **Kontrolery integralności** | wykrywanie i raportowanie zmian w systemie (pliki, logowania) – Tripwire |
| **Wykrywanie wirusów** | antywirus/antymalware |
| *starsze* | **war-dialing** (modemy), **war-driving** (Wi-Fi) – nadal warto uwzględnić |

**Rekonesans:** **aktywny** (bezpośrednia interakcja z systemami: skanowanie, narzędzia pentestowe) i **pasywny** (OSINT: wyszukiwanie w źródłach publicznych – serwisy społecznościowe, wycieki haseł w dark webie, strona firmy – *footprinting*). Testerzy „myślą jak aktorzy zagrożeń", by znaleźć luki, zanim zrobią to prawdziwi atakujący.

### Wykorzystanie wyników testów (kurs Cisco)

- określenie **działań łagodzących** dla słabych punktów,
- **punkt odniesienia** do śledzenia postępów (baseline),
- ocena stanu wdrożenia wymagań bezpieczeństwa,
- **analiza kosztów i korzyści** poprawy bezpieczeństwa,
- wsparcie ocen ryzyka, certyfikacji i autoryzacji (C&A),
- baza działań naprawczych.

## 2. Testy penetracyjne

**Test penetracyjny** symuluje metody atakującego (z **zgodą organizacji**) w celu uzyskania nieautoryzowanego dostępu do sieci i skompromitowania systemów – pokazuje, **jak dobrze organizacja zniosłaby prawdziwy atak**. Technika **etycznego hackingu**; główny cel: **znaleźć i naprawić luki zanim zrobią to cyberprzestępcy**.

### Rodzaje według wiedzy testera

| Typ | Wiedza | Cechy |
| :--- | :--- | :--- |
| **Black box** | brak wiedzy o wnętrzu systemu; perspektywa zwykłego użytkownika/zewnętrznego atakującego | **najmniej czasochłonny i najtańszy** |
| **Gray box** | ograniczona wiedza (częściowo znane środowisko) | kompromis |
| **White box** | pełna wiedza o działaniu systemu; naśladuje atak insidera lub kogoś, kto zdobył informacje wcześniej | **najbardziej czasochłonny i najdroższy** |

### Cztery fazy testu penetracyjnego (kurs Cisco)

1. **Planowanie** – ustalenie zasad przeprowadzania testu (zakres, zgody, harmonogram, reguły zaangażowania),
2. **Odkrywanie** – rozpoznanie celu: **pasywne** (*foot printing*, publiczne źródła) i **aktywne** (skanowanie portów),
3. **Atak** – uzyskanie dostępu na podstawie zebranych informacji, **eskalacja uprawnień**, **ruch boczny**, instalacja narzędzi lub backdoora (**persistence**), a następnie **posprzątanie śladów**,
4. **Raportowanie** – szczegółowa dokumentacja: zidentyfikowane podatności, podjęte działania, wyniki.

### Ćwiczenia zespołów (kurs Cisco)

Dłuższe od pentestu:

- **Zespół czerwony (red team)** – przeciwnik atakujący i pozostający niezauważonym,
- **Zespół niebieski (blue team)** – obrońcy,
- **Zespół biały (white team)** – neutralny, definiuje cele i zasady, sędzia (wiedza o zarządzaniu i zgodności),
- **Zespół fioletowy (purple team)** – współpraca czerwonych i niebieskich w celu wykrycia słabości i poprawy kontroli.

### Zasady etyczno-prawne *(uzupełnienie)*

Pisemna **zgoda i zakres**, ochrona danych uzyskanych w teście, unikanie szkód (kopie zapasowe, okna serwisowe), poufność wyników; nieautoryzowane testy są przestępstwem (np. art. 267 k.k.).

## 3. Narzędzia testowania (kurs Cisco, Moduł 2)

| Narzędzie | Zadanie |
| :--- | :--- |
| **Nmap/Zenmap** | wykrywanie komputerów i usług, mapa sieci |
| **SuperScan** | skaner portów TCP/UDP dla Windows |
| **SIEM** | raportowanie w czasie rzeczywistym i analiza długoterminowa |
| **GFI LANguard** | skaner sieci i bezpieczeństwa |
| **Tripwire** | weryfikacja konfiguracji względem polityk i standardów |
| **Nessus** | skanowanie podatności (zdalny dostęp, błędne konfiguracje, DoS stosu TCP/IP) |
| **L0phtCrack** | audyt i odzyskiwanie haseł |
| **Metasploit** | informacje o lukach, testy penetracyjne, sygnatury IDS |
| **Wireshark** | analiza ruchu (np. Telnet vs SSH) |

Uwaga kursu: narzędzia szybko ewoluują – lista ma pokazać rodzaje narzędzi.

## 4. Audyt bezpieczeństwa

### Rodzaje (wykład ryzyko)

| Typ | Opis |
| :--- | :--- |
| **Wewnętrzny** | własny personel, bieżąca ocena zgodności z politykami |
| **Zewnętrzny** | niezależni audytorzy; obiektywna ocena, certyfikacja (ISO) |
| **Zgodności** | spełnienie wymogów prawnych (RODO, ustawa o KSC/NIS2) i standardów (ISO 27001, PCI DSS) |
| **Testy penetracyjne** | jako forma audytu technicznego |

### Proces audytu – cykl PDCA (wykład ryzyko)

1. **Planowanie** – zakres, cele, kryteria, metodyka, harmonogram, zasoby,
2. **Wykonanie** – zbieranie dowodów, wywiady, analiza dokumentacji, testowanie kontroli,
3. **Raportowanie** – dokumentacja ustaleń, niezgodności, rekomendacje, raport dla kierownictwa,
4. **Działania korygujące** – wdrożenie rekomendacji, weryfikacja skuteczności, aktualizacja polityk.

Audyt obejmuje **przegląd polityk, procedur, konfiguracji i testy** (w sieciach dostępowych: konfiguracje przełączników i AP, zgodność z baseline'em, mechanizmy 802.1X, porty, uprawnienia, logi, inwentarz urządzeń, procedury).

Ważne: monitoring i audyty weryfikują skuteczność wdrożonych zabezpieczeń i **identyfikują obszary do poprawy**; dostarczają obiektywnych danych do decyzji o inwestycjach i priorytetach.

## 5. Zarządzanie podatnościami (kurs Cisco, Moduł 4)

Wg NIST – **proaktywne zapobieganie wykorzystywaniu luk** w IT; krótszy czas i niższe koszty niż reagowanie po exploicie.

### Cykl życia (sześć etapów)

| Etap | Treść |
| :--- | :--- |
| **Odkryj** | zinwentaryzuj zasoby, szczegóły hostów (system, usługi), opracuj linię bazową, regularne zautomatyzowane skanowanie |
| **Ustalanie priorytetów aktywów** | grupy/jednostki biznesowe, wartość biznesowa wg krytyczności |
| **Ocena** | profil ryzyka wg krytyczności aktywów, podatności, zagrożeń, klasyfikacji |
| **Raport** | poziom ryzyka biznesowego, plan bezpieczeństwa, monitoring, znane luki |
| **Środek zaradczy** | priorytety wg ryzyka biznesowego, eliminacja w kolejności ryzyka |
| **Weryfikacja** | kolejne audyty potwierdzające usunięcie |

Wymaga: identyfikacji luk z **biuletynów dostawców i CVE**, kompetencji w ocenie wpływu, skutecznego wdrażania poprawek (z oceną nieprzewidzianych skutków) i **testu**, czy podatność usunięto. Powiązane: **zarządzanie poprawkami** i **aktywami** (zob. temat 5), **CVSS** do priorytetyzacji (temat 9).

## Zastosowanie w sieciach dostępowych – przykładowy plan

| Częstotliwość | Działanie |
| :--- | :--- |
| ciągle | monitoring (SIEM, NetFlow), alerty o nowych urządzeniach |
| tydzień/miesiąc | skan podatności hostów i urządzeń sieciowych, przegląd poprawek |
| kwartał | audyt konfiguracji przełączników/AP względem baseline, przegląd uprawnień, test odtwarzania |
| rok | test penetracyjny (zewnętrzny i wewnętrzny, w tym test podpięcia do LAN i Wi-Fi), audyt zgodności |
| po zmianach | ponowny test (ST&E) |

## Podsumowanie

- **Ocena podatności** (skanery: Nessus, OpenVAS, Nmap – **identyfikacja**), **test penetracyjny** (autoryzowane symulowane ataki – **wykorzystanie**; black/gray/white box; fazy: planowanie, odkrywanie, atak, raportowanie), **analiza ryzyka** (znaczenie dla organizacji) i **audyt** (wewnętrzny/zewnętrzny/zgodności, cykl PDCA) uzupełniają się.
- Wyniki służą do **działań naprawczych, baseline'u, priorytetyzacji i oceny kosztów**; testy powtarza się okresowo i po zmianach (ST&E).
- Ćwiczenia: **red, blue, white, purple team**; skanowanie **uwierzytelnione** daje mniej fałszywych alarmów, **inwazyjne** niesie ryzyko awarii.

---
[⬅️ Poprzedni temat](7_Rola_wielowarstwowej_ochrony_w_zabezpieczaniu_lokalnych_sieci_komputerowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](9_Źródła_struktura_i_ocena_alertów_bezpieczeństwa_w_lokalnych_sieciach_komputerowych.md)