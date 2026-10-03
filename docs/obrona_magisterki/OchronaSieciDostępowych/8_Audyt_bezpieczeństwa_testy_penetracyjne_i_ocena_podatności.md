# Audyt bezpieczeństwa, testy penetracyjne i ocena podatności w ochronie sieci dostępowych

Audyt, testy penetracyjne i ocena podatności to trzy powiązane, ale różne metody **weryfikacji skuteczności zabezpieczeń**. Samo wdrożenie zabezpieczeń nie wystarcza, trzeba sprawdzić, czy działają, i robić to **regularnie**, bo sieć i zagrożenia się zmieniają.

## Audyt bezpieczeństwa

**Audyt bezpieczeństwa** to systematyczna ocena zgodności zabezpieczeń z politykami, standardami (ISO 27001, CIS, NIS2) i dobrymi praktykami. Obejmuje przegląd dokumentacji, konfiguracji, uprawnień i logów oraz procedur. Zalicza się do niego **ST&E (testy i ocena bezpieczeństwa)**: wykrywanie błędów projektowych, wdrożeniowych i operacyjnych, ocena adekwatności mechanizmów i zgodności dokumentacji z rzeczywistością. Powtarza się go okresowo i po zmianach.

### Rodzaje

| Typ | Opis |
| :--- | :--- |
| **Wewnętrzny** | własny personel, bieżąca ocena zgodności z politykami |
| **Zewnętrzny** | niezależni audytorzy; obiektywna ocena, certyfikacja |
| **Zgodności** | spełnienie wymogów prawnych (RODO, ustawa o KSC/NIS2) i standardów |
| **Testy penetracyjne** | jako forma audytu technicznego |

### Proces audytu – cykl PDCA

1. **Planowanie** – zakres, cele, kryteria, metodyka, harmonogram, zasoby,
2. **Wykonanie** – zbieranie dowodów, wywiady, analiza dokumentacji, testowanie kontroli,
3. **Raportowanie** – dokumentacja ustaleń, niezgodności, rekomendacje, raport dla kierownictwa,
4. **Działania korygujące** – wdrożenie rekomendacji, weryfikacja skuteczności, aktualizacja polityk.

## Testy penetracyjne

**Test penetracyjny (pentest)** to autoryzowana **symulacja ataku**, w której tester próbuje **wykorzystać** podatności, żeby pokazać realny wpływ. Etapy to **planowanie** (zakres, zgoda), **odkrywanie** (rozpoznanie pasywne i aktywne), **atak** (eksploatacja, eskalacja uprawnień) i **raportowanie**. Podział według wiedzy testera: **black, gray i white box**. Ćwiczenia zespołów: red team (atakuje), blue team (broni), white team (sędziuje), purple team (współpraca).

### Rodzaje według wiedzy testera

| Typ | Wiedza | Cechy |
| :--- | :--- | :--- |
| **Black box** | brak wiedzy o wnętrzu systemu; perspektywa zwykłego użytkownika/zewnętrznego atakującego | **najmniej czasochłonny i najtańszy** |
| **Gray box** | ograniczona wiedza (częściowo znane środowisko) | kompromis |
| **White box** | pełna wiedza o działaniu systemu; naśladuje atak insidera lub kogoś, kto zdobył informacje wcześniej | **najbardziej czasochłonny i najdroższy** |

### Cztery fazy testu penetracyjnego

1. **Planowanie** – ustalenie zasad przeprowadzania testu (zakres, zgody, harmonogram, reguły zaangażowania),
2. **Odkrywanie** – rozpoznanie celu: **pasywne** (*foot printing*, publiczne źródła) i **aktywne** (skanowanie portów),
3. **Atak** – uzyskanie dostępu na podstawie zebranych informacji, **eskalacja uprawnień**, **ruch boczny**, instalacja narzędzi lub backdoora (**persistence**), a następnie **posprzątanie śladów**,
4. **Raportowanie** – szczegółowa dokumentacja: zidentyfikowane podatności, podjęte działania, wyniki.

## Zarządzanie podatnościami

**Ocena podatności (vulnerability assessment)** polega na **identyfikacji** słabych punktów, zwykle automatycznej, skanerami (Nessus, OpenVAS, Retina, GFI LANguard):

- skanowanie z uwierzytelnianiem daje mniej fałszywych wyników niż bez,
- skany mogą być sieciowe, aplikacyjne i webowe, inwazyjne lub nieinwazyjne,
- trzeba weryfikować wyniki (fałszywe alarmy).

### Cykl życia

| Etap | Treść |
| :--- | :--- |
| **Odkryj** | zinwentaryzuj zasoby, szczegóły hostów (system, usługi), opracuj linię bazową, regularne zautomatyzowane skanowanie |
| **Ustalanie priorytetów aktywów** | grupy/jednostki biznesowe, wartość biznesowa wg krytyczności |
| **Ocena** | profil ryzyka wg krytyczności aktywów, podatności, zagrożeń, klasyfikacji |
| **Raport** | poziom ryzyka biznesowego, plan bezpieczeństwa, monitoring, znane luki |
| **Środek zaradczy** | priorytety wg ryzyka biznesowego, eliminacja w kolejności ryzyka |
| **Weryfikacja** | kolejne audyty potwierdzające usunięcie |

## Różnice

- **ocena podatności:** szeroka, głównie automatyczna, wykrywa (lista słabości),
- **pentest:** głębszy, w dużej mierze ręczny, wykorzystuje luki i pokazuje skutki,
- **audyt:** sprawdza zgodność procesów, polityk i konfiguracji z wymaganiami.

**Ochrona sieci dostępowych:** testy obejmują m.in. przełączniki i VLAN-y (port security, DAI, DHCP snooping), **802.1X/NAC**, sieci Wi-Fi (rogue AP, łamanie WPA), urządzenia końcowe, IoT i zabezpieczenia haseł.

**Zarządzanie podatnościami (cykl):** odkrycie → priorytetyzacja → ocena → raport → środek zaradczy → weryfikacja.

**Zasady:** pisemna zgoda i jasny zakres, ostrożność w środowisku produkcyjnym, raport z rekomendacjami i terminami, retest po naprawach. Test to ocena **w danym momencie**, więc powtarza się go cyklicznie.

## Podsumowanie

- **Ocena podatności** (skanery: Nessus, OpenVAS, Nmap – **identyfikacja**), **test penetracyjny** (autoryzowane symulowane ataki – **wykorzystanie**; black/gray/white box; fazy: planowanie, odkrywanie, atak, raportowanie), **analiza ryzyka** (znaczenie dla organizacji) i **audyt** (wewnętrzny/zewnętrzny/zgodności, cykl PDCA) uzupełniają się.
- Wyniki służą do **działań naprawczych, baseline'u, priorytetyzacji i oceny kosztów**; testy powtarza się okresowo i po zmianach (ST&E).
- Ćwiczenia: **red, blue, white, purple team**; skanowanie **uwierzytelnione** daje mniej fałszywych alarmów, **inwazyjne** niesie ryzyko awarii.

---
[⬅️ Poprzedni temat](7_Rola_wielowarstwowej_ochrony_w_zabezpieczaniu_lokalnych_sieci_komputerowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](9_Źródła_struktura_i_ocena_alertów_bezpieczeństwa_w_lokalnych_sieciach_komputerowych.md)