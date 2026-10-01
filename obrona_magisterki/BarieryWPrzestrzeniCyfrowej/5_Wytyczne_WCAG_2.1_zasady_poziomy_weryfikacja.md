# Wytyczne dostępności treści internetowych WCAG 2.1 – zasady, poziomy, weryfikacja

## Czym jest WCAG

**WCAG (Web Content Accessibility Guidelines)** – wytyczne dostępności treści internetowych, **standard W3C** (World Wide Web Consortium; inicjatywa WAI). **WCAG 2.1** (Rekomendacja W3C, 5 czerwca 2018) jest następcą WCAG 2.0; to zestaw rekomendacji dotyczących tworzenia treści internetowych dostępnych dla jak największej grupy użytkowników, szczególnie z niepełnosprawnościami.

- Oryginał: https://www.w3.org/TR/WCAG21/; polskie tłumaczenie: https://wcag21.lepszyweb.pl/.
- **WCAG 2.1 jest standardem minimalnym dostępności cyfrowej** w polskiej ustawie o dostępności cyfrowej stron internetowych i aplikacji mobilnych podmiotów publicznych (z 4.04.2019) – załącznik zawiera tabelę wytycznych i kryteriów z poziomami.
- *(uzupełnienie)* WCAG 2.1 jest **wstecznie zgodne** z 2.0 (dodaje kryteria dla urządzeń mobilnych, słabowidzących i osób z ograniczeniami poznawczymi). **WCAG 2.2** (Rekomendacja W3C z października 2023) dodaje kolejne kryteria i jest zgodne wstecznie z 2.1; w Polsce wiążącym minimum jest nadal 2.1 (do czasu zmiany przepisów).

## Struktura WCAG – trzy warstwy

```
 4 zasady  ─▶  13 wytycznych  ─▶  kryteria sukcesu (78)  ─▶  techniki (wystarczające / zalecane)
 (POUR)         (cele ogólne)      (testowalne, A/AA/AAA)
```

1. **Zasady** (obszary) – 4,
2. **Wytyczne** – 13 ogólnych celów (np. „tekst alternatywny dla treści nietekstowych"),
3. **Kryteria sukcesu** – **mierzalne** (testowalne) wymagania przypisane do poziomu zgodności; to na nich opiera się ocena i automatyczna weryfikacja,
4. (poza normą) **Techniki** i opisy błędów – jak spełnić kryterium.

Wytyczne WCAG 2.1 są **weryfikowalne i precyzyjne** (kryterium sukcesu – trzecia warstwa).

## Cztery zasady (POUR)

| Zasada | Pytanie | Wytyczne |
| :--- | :--- | :--- |
| **1. Postrzegalność** (Perceivable) | czy użytkownik może **odebrać** informację zmysłami? | 1.1 alternatywa w postaci tekstu; 1.2 dostępność mediów zmiennych w czasie; 1.3 możliwość adaptacji (zrozumiała prezentacja zawartości); 1.4 możliwość rozróżnienia (ułatwienie percepcji treści) |
| **2. Funkcjonalność** (Operable) | czy można **obsługiwać** interfejs? | 2.1 dostępność z klawiatury; 2.2 wystarczająca ilość czasu; 2.3 napady i reakcje fizyczne (brak migotania); 2.4 możliwość nawigacji; 2.5 metody wprowadzania (gesty, wskaźniki) |
| **3. Zrozumiałość** (Understandable) | czy informacja i obsługa są **zrozumiałe**? | 3.1 możliwość odczytania (język); 3.2 przewidywalność; 3.3 pomoc w wprowadzaniu informacji (błędy w formularzach) |
| **4. Kompatybilność** (Robust) | czy treść działa z różnymi **przeglądarkami i technologiami wspomagającymi**? | 4.1 kompatybilność (poprawny kod, nazwa, rola, wartość, komunikaty statusu) |

Wytyczne w ramach zasady 1 (z wykładu): **1.1** Alternatywa w postaci tekstu; **1.2** Dostępność mediów zmiennych w czasie (dynamicznych); **1.3** Możliwość adaptacji – odpowiednia (zrozumiała) prezentacja zawartości; **1.4** Możliwość rozróżnienia – ułatwienie percepcji treści.

### Kryteria sukcesu

- Opis osiągnięcia każdej z **13 wytycznych**, np. „wszelkie treści nietekstowe przedstawione użytkownikowi posiadają swoją tekstową alternatywę, która pełni tę samą funkcję, z wyjątkiem sytuacji opisanych w kryterium".
- Są **mierzalne** (testowalne) – służą do oceny zgodności.
- *(uzupełnienie)* W WCAG 2.1 jest **78 kryteriów** (30 na poziomie A, 20 na AA, 28 na AAA), w tym **17 nowych** w stosunku do 2.0.

## Poziomy zgodności

| Poziom | Znaczenie (wg wykładu) | Charakter |
| :---: | :--- | :--- |
| **A** | strona/aplikacja **musi** spełniać | minimalny; usuwa najpoważniejsze bariery |
| **AA** | strona/aplikacja **powinna** spełniać | standardowy cel; **wymagany przez prawo** (A + AA) |
| **AAA** | strona/aplikacja **może** spełniać | najwyższy; nie dla całych serwisów, tylko tam, gdzie to możliwe |

Zgodność jest **mierzalna** (kryteria spełnione/niespełnione). Zgodność na poziomie wyższym zakłada spełnienie niższych (AA = A + AA).

### Przykładowe kryteria *(uzupełnienie)*

| Kryterium | Poziom | Treść |
| :--- | :-: | :--- |
| 1.1.1 Treść nietekstowa | A | każdy element nietekstowy ma tekstową alternatywę (np. `alt`) |
| 1.2.2 / 1.2.5 | A / AA | napisy rozszerzone / audiodeskrypcja dla nagrań |
| 1.3.1 Informacje i relacje | A | struktura (nagłówki, listy, tabele, etykiety) wyrażona w kodzie |
| 1.4.1 Użycie koloru | A | kolor nie jest jedynym nośnikiem informacji |
| **1.4.3 Kontrast (minimalny)** | AA | tekst: **4,5 : 1** (duży tekst **3 : 1**); poziom AAA (1.4.6): 7 : 1 |
| 1.4.4 Zmiana rozmiaru tekstu | AA | powiększanie do 200% bez utraty treści |
| 1.4.10 Dopasowanie do ekranu (reflow) | AA | brak przewijania w dwóch kierunkach przy powiększeniu (*nowe w 2.1*) |
| 1.4.11 Kontrast elementów nietekstowych | AA | 3 : 1 dla elementów interfejsu (*nowe w 2.1*) |
| **2.1.1 Klawiatura** | A | pełna obsługa z klawiatury |
| 2.4.1 Możliwość pominięcia bloków | A | „przejdź do treści" |
| 2.4.7 Widoczny fokus | AA | widoczne wskazanie fokusu klawiatury |
| 2.5.1 Gesty wielopunktowe | A | alternatywa dla złożonych gestów (*2.1*) |
| 3.1.1 Język strony | A | zadeklarowany język (`lang`) |
| 3.3.1 / 3.3.2 | A | identyfikacja błędu / etykiety lub instrukcje pól |
| 4.1.2 Nazwa, rola, wartość | A | komponenty mają nazwy i role dla technologii wspomagających |
| 4.1.3 Komunikaty o statusie | AA | komunikaty ogłaszane czytnikom bez przenoszenia fokusu (*2.1*) |

Listy kontrolne (np. opracowana na Yale: „WCAG 2.1 lista kontrolna A/AA") wypisują najczęstsze błędy w formie pytań (np. „wszystkie grafiki mają atrybut `alt` w języku polskim; opis nie dłuższy niż ok. 125 znaków; złożona grafika ma opis pod nią") – **nie zawierają wszystkich heurystyk**, ale ułatwiają weryfikację.

## ARIA – Accessible Rich Internet Applications

- **Zbiór atrybutów** umożliwiających tworzenie aplikacji webowych (zwłaszcza z **AJAX/JavaScript**) bardziej przyjaznych osobom z niepełnosprawnościami: dostępna nawigacja, pomoc przy wprowadzaniu, przyjazne aktualizacje treści itp.
- Atrybuty można dodać do dowolnego języka znaczników, ale są przystosowane głównie do **HTML**.
- Atrybut **`role`** definiuje role obiektów (np. `article`, `alert`, `slider`, `button`); atrybuty **`aria-*`** dostarczają opisy formularzy, **długość paska postępu**, **stany** (aktywny/nieaktywny, rozwinięty/zwinięty) itp.
- *(uzupełnienie)* Zasada: **najpierw natywny HTML** (`<button>`, `<nav>`, `<label>`), ARIA tylko tam, gdzie HTML nie wystarcza; zła ARIA jest gorsza niż brak ARIA.

## Weryfikacja zgodności z WCAG

Kryteria WCAG 2.1 są **weryfikowalne i precyzyjne**, co umożliwia **automatyczne sprawdzanie**.

### Automatyczna weryfikacja (wg wykładu)

- **W3C Markup Validation Service** – poprawność kodu HTML/CSS,
- **WAVE** (https://wave.webaim.org/) – narzędzie WebAIM, nakłada na stronę ikony błędów i ostrzeżeń (brak `alt`, niski kontrast, puste linki, brak etykiet), pokazuje strukturę i elementy ARIA,
- *(uzupełnienie)* axe DevTools, Lighthouse, Pa11y, narzędzia do mierzenia kontrastu.

### Ograniczenia automatów

*(uzupełnienie)* Automaty wykrywają tylko **część** problemów (szacunkowo ok. 30–40%): mogą sprawdzić **obecność** atrybutu `alt`, ale nie ocenią, czy opis jest **sensowny**; nie ocenią logicznej kolejności czytania ani zrozumiałości treści.

### Pełna weryfikacja

1. **Test automatyczny** (WAVE/axe/walidator).
2. **Audyt ekspercki / inspekcja z listą kontrolną** WCAG (A/AA) – zob. temat 7.
3. **Test klawiaturą** (Tab, Enter, Esc, brak pułapek fokusu) i z **czytnikiem ekranu** (NVDA, VoiceOver); powiększenie do 200%; test kontrastu.
4. **Testy z użytkownikami** z niepełnosprawnościami.
5. **Deklaracja dostępności** i bieżący monitoring (po zmianach treści).

## Praktyka wdrażania w UE i Polsce

- **UE:** dyrektywa 2016/2102 (strony i aplikacje mobilne sektora publicznego: strony – od 09.2020, aplikacje – od 06.2021); EAA 2019/882 (od 28.06.2025; sektor prywatny).
- **Polska:** ustawa z 4.04.2019 o dostępności cyfrowej (WCAG 2.1 – minimum), ustawa o zapewnianiu dostępności osobom ze szczególnymi potrzebami; Polski Akt o Dostępności (od 28.06.2025) – zob. temat 1.

## Podsumowanie

- WCAG 2.1 (W3C): **4 zasady POUR** (postrzegalność, funkcjonalność, zrozumiałość, kompatybilność) → **13 wytycznych** → **kryteria sukcesu** (mierzalne).
- **Poziomy zgodności:** **A** (musi), **AA** (powinna – wymóg prawny), **AAA** (może).
- **ARIA** uzupełnia HTML o role, stany i właściwości dla dynamicznych aplikacji.
- Weryfikacja: narzędzia automatyczne (**WAVE**, W3C Validator) + listy kontrolne + testy z czytnikiem ekranu i z użytkownikami.
