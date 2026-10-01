# Techniki oceny jakości interfejsów z udziałem i bez udziału użytkowników

## Przegląd

| | Z udziałem użytkowników | Bez udziału użytkowników |
| :--- | :--- | :--- |
| **Źródło oceny** | grupa użytkowników / potencjalnych użytkowników | **eksperci** (analiza ekspercka) |
| **Techniki** | testowanie interfejsu (scenariusze), test Kruga, **okulografia**, śledzenie działań (clicktracking), zbieranie opinii (ankiety, wywiady), obserwacja, analiza dzienników | analiza ekspercka, **wędrówka poznawcza** (uproszczona i rozwinięta), **ocena heurystyczna**, **inspekcja standardów**, lista LUT, punkty WUP |

## TECHNIKI Z UDZIAŁEM UŻYTKOWNIKÓW

### 1. Testowanie interfejsu przez użytkowników (eksperyment aktywny)

**Definicja:** odpowiednio dobrana grupa użytkowników pracuje z oprogramowaniem, wykonując **zadane scenariusze** (lub bez scenariuszy); w trakcie/po eksperymencie zbierane są wyniki. Zwykle **techniki mieszane**: obserwacja w trakcie, **rejestracja działań i parametrów** (czas, kliknięcia, ścieżki), **ankietowanie po eksperymencie**.

- **Metryki:** sukces zadania, czas, liczba błędów, kliknięcia, korzystanie z pomocy itd. (temat 6).
- **Zalety:** **najbardziej efektywna** metoda; **dużo informacji**; wykrywa **niespodziewane** działania użytkowników; daje sugestie nowych funkcji; precyzyjne informacje.
- **Wady:** **niezręczność, nienaturalność** sytuacji dla użytkowników; **brak kontroli** nad przebiegiem obserwacji; **trudna do zorganizowania** (koszt, rekrutacja).

#### Organizacja eksperymentu

1. **Przygotowanie** (plan, scenariusze, sprzęt, oprogramowanie, miejsce),
2. **dobór i pozyskanie grupy badawczej**,
3. **przeprowadzenie** eksperymentu,
4. **analiza i uogólnienie** wyników,
5. **wnioski i rekomendacje**.

**Dobór grupy badawczej:** użytkownicy vs potencjalni użytkownicy (→ **persony**). Czynniki doboru: cel badań, doświadczenie z interfejsem/poprzednimi wersjami, **zaburzenia widzenia**, wiek i wykształcenie, pochodzenie społeczne i kulturowe, wymagana liczebność.

**Liczebność grupy (wg wykładu):** **5 osób wykrywa ok. 80%** problemów; **20 osób – praktycznie wszystkie**; „optymalnie" **5–8 osób**; dla różnych person – osobne grupy (po 5–8 osób); badania niekontrolowane (ankietowe) – większa liczba. *(uzupełnienie)* Model Nielsena–Landauera: odsetek wykrytych problemów $=1-(1-L)^n$, $L\approx31\%$ → 5 użytkowników ≈ 84%.

**Stres uczestników:** należy go unikać (chyba że jest celem badania). Źródła: trauma, poczucie bycia sprawdzanym/testowanym, porównywanym z innymi, rywalizacji, obserwowanym i oceniającym przez „mądrzejszych" badaczy. Przeciwdziałanie: zapewnienie **komfortu, prywatności, dyskrecji, szkolenia, tłumaczenia** celu badania (nie testujemy człowieka, tylko interfejs).

### 2. Test Kruga

W wykładzie wymieniony jako „**Test Kruga – przerwanie i przypomnienie**". *(uwaga)* Wykład nie rozwija tej techniki; najprawdopodobniej odnosi się do uproszczonych, tanich testów użyteczności z małą grupą użytkowników wg **Steve'a Kruga** (to moja interpretacja, nie treść wykładu). Jeśli prowadzący omawiał ją szerzej, uzupełnij z własnych notatek.

### 3. Okulografia (eyetracking)

Badanie **aktywności gałek ocznych** – co i jak postrzega użytkownik. Dostarcza informacji o **aktywności wzroku, nie o zrozumieniu** informacji; pokazuje, na czym użytkownik skupia wzrok, a które obszary pomija (szczegółowo w temacie 10).

### 4. Śledzenie działań użytkownika (clicktracking)

Rejestracja kliknięć, ruchów myszy, przewijania; **mapy kliknięć**, ścieżki nawigacji, czasy na stronie; możliwa w trakcie normalnej eksploatacji; technika ilościowa, nie wyjaśnia przyczyn.

### 5. Zbieranie opinii użytkowników (user feedback)

Dotyczy wersji **eksploatowanych** i **testowych (beta)**.

- **Metody:** automatyczne (formularze, oceny w aplikacji); **wywiad z użytkownikami**: **ankieta papierowa**, **elektroniczna**, e-maile, **fora dyskusyjne**, **wywiady zogniskowane** (grupowe i jednostkowe).
- **Zalety:** szybkość, mało kłopotliwe, niskie koszty, **proces ciągły**, inicjatywa wychodzi od użytkowników.
- **Wady:** **niereprezentatywna próbka**, mało konkretne odpowiedzi, czasem mylne sugestie, **subiektywizm**, sprzeczności wypowiedzi.
- Narzędzia kwestionariuszowe: **SUS** (temat 8).

### 6. Obserwacja działań użytkownika i analiza dzienników (techniki bierne)

- **Obserwacja** – badacz obserwuje pracę użytkownika (nie ingerując) i notuje zachowania; można rejestrować wideo.
- **Analiza dzienników/zapisów** (logów) – statystyka użycia funkcji, ścieżki, miejsca porzuceń, błędy; wymaga dużej próby.
- Obie są **technikami biernymi** – użytkownik pracuje naturalnie.

*(uzupełnienie)* Dodatkowo: **think-aloud** (myślenie na głos), **testy A/B**, **sortowanie kart** (struktura informacji), **RTA** (retrospektywny think-aloud przy eyetrackingu).

### Adnotacje i analiza zapisów

Zapisy audio-wideo, notatki, ankiety, zapisy eyetrackera analizuje się z użyciem **adnotacji** (opisy zdarzeń i działań z czasem i kodami) – szczegóły w materiałach dodatkowych.

## TECHNIKI BEZ UDZIAŁU UŻYTKOWNIKÓW (METODY EKSPERCKIE)

Wykonują **eksperci** (analiza ekspercka, *expert review*). Zalety: **szybkie i tańsze** od testów, możliwe na wczesnym etapie (np. na prototypie), duża liczba problemów, nie ma kłopotów z rekrutacją użytkowników. Wady: **subiektywizm** ekspertów, ryzyko niezgodności z rzeczywistymi zachowaniami użytkowników, **fałszywe alarmy**; nie zastępują testów z użytkownikami.

### 1. Uproszczona wędrówka poznawcza (Cognitive Walkthrough)

- **Cel:** ocena **procesu uczenia się** oraz **płynności realizacji zadań** z użyciem oprogramowania.
- **Metoda:** ekspert realizuje **scenariusz wykorzystania** i odpowiada na pytania badawcze, oceniając je w **skali Likerta 1–5**.
- **Pytania badawcze:**
  1. Czy użytkownik będzie wiedział, **jak osiągnąć** zamierzony efekt?
  2. Czy użytkownik **zauważy** elementy interfejsu pomocne w realizacji zadania?
  3. Czy użytkownik potrafi **powiązać** elementy interfejsu z akcjami, które musi wykonać?
  4. Czy użytkownik będzie **informowany o stanie** realizacji zadania (informacja zwrotna)?

### 2. Rozwinięta wędrówka poznawcza (Pluralistic Walkthrough)

Rozszerzenie uproszczonej – zespół ekspertów rozszerza się o **użytkowników, programistów i innych członków zespołu projektowego**; pozostałe działania jak w uproszczonej.

### 3. Ocena heurystyczna

Eksperci oceniają interfejs według standardowego zestawu zasad dobrego interfejsu (**heurystyk**) – szczegółowo w temacie 9.

### 4. Inspekcja standardów i rekomendacji (guidelines inspection)

Formalna kontrola interfejsu **pod kątem zgodności z wymaganiami** (np. **WCAG 2.1**, ISO, wytyczne platformy) za pomocą **listy kontrolnej**.

- **Lista kontrolna** – specjalny kwestionariusz: kilka–kilkanaście **monotematycznych sekcji** (wymagania główne) po kilka–kilkanaście pytań; odpowiedzi **binarne** (tak/nie, spełnia/nie spełnia) lub **skala** (1–5, 2–5).
- **Etapy:** wybór listy kontrolnej → **przegląd wstępny** oprogramowania, funkcji i sposobu obsługi → **inspekcja** → opracowanie wyników → raport.
- **Wyniki:** **% spełnienia wymagań głównych**; **uśredniony poziom** spełnienia (w skali) dla głównych wymagań i całej listy; **lista niespełnionych wymagań** z oceną istotności i propozycjami poprawy.

### 5. Lista LUT

**Heurystyka w formie listy kontrolnej**; struktura: **obszary → podobszary → pytania**. Przykładowe obszary: *nawigacja i struktura* (łatwość nawigacji, struktura informacji), *komunikaty, feedback, pomoc dla użytkownika* (komunikaty ogólne, komunikaty o błędach, informacje zwrotne i pomoc), *interfejs aplikacji* (layout, dobór barw – m.in. czy interfejs umożliwia korzystanie osobom z zaburzeniami widzenia barw), *treść podstron* (teksty, nazewnictwo, etykiety), *wprowadzanie danych* (formularze, dane).

**Skala oceny pytań (1–5):**

| Ocena | Znaczenie |
| :-: | :--- |
| **1** | **krytyczne** problemy użyteczności, uniemożliwiające korzystanie lub zniechęcające do korzystania z aplikacji |
| **2** | **poważne** problemy uniemożliwiające większości użytkowników realizację zadań |
| **3** | **drobne** problemy, które pojedynczo stanowią utrudnienie dla większości użytkowników, jednak ich nagromadzenie może wpłynąć na jakość pracy |
| **4** | zidentyfikowano **pojedyncze drobne** problemy mogące obniżyć jakość pracy (np. słaba czytelność tekstu) |
| **5** | **nie stwierdzono** problemów |

### 6. Punkty WUP (Web Usability Points)

Metryka oceny jakości **aplikacji webowej** oparta na liście LUT: średnia ocen pytań uśredniona po podobszarach i obszarach:

$$WUP=\frac{1}{n_a}\sum_{i=1}^{n_a}\frac{1}{s_i}\sum_{j=1}^{s_i}\frac{1}{q_{ij}}\sum_{k=1}^{q_{ij}}p_{ijk}$$

- $n_a$ – liczba obszarów, $s_i$ – liczba podobszarów w obszarze $i$, $q_{ij}$ – liczba pytań w podobszarze $j$ obszaru $i$, $p_{ijk}$ – ocena pytania $k$ (1–5),
- zakres **1…5** (**im więcej, tym lepsza jakość interfejsu**),
- umożliwia porównywanie wariantów (**globalna miara użyteczności**).

*Przykład:* dwa obszary: A (dwa podobszary o średnich 4 i 2) i B (jeden podobszar o średniej 3). Średnia A $=3$, B $=3$, $WUP=3$.

## Porównanie technik

| Cecha | Z udziałem użytkowników | Bez udziału użytkowników (eksperci) |
| :--- | :--- | :--- |
| Autentyczność wyników | **wysoka** (rzeczywiste zachowania) | umiarkowana (ocena „w imieniu" użytkownika) |
| Koszt i czas | wysoki (rekrutacja, sprzęt, analiza) | **niższy**, szybszy |
| Etap projektu | prototyp, produkt | **wczesny** (szkice, makiety) |
| Wykrywane problemy | nieoczekiwane, rzeczywiste | zgodność ze standardami, oczywiste błędy |
| Ryzyko | stres uczestników, nienaturalność | subiektywizm, fałszywe alarmy |
| Zalecenie | **uzupełniać się wzajemnie** | **uzupełniać się wzajemnie** |

## Podsumowanie

- **Z użytkownikami:** testowanie (scenariusze), test Kruga, **okulografia**, clicktracking, opinie (ankiety, wywiady), obserwacja, analiza logów.
- **Bez użytkowników (eksperci):** **wędrówka poznawcza** (uproszczona – 4 pytania w skali 1–5; rozwinięta – szerszy zespół), **ocena heurystyczna**, **inspekcja standardów** (listy kontrolne, WCAG), **lista LUT**, **WUP**.
- Testowanie z użytkownikami jest najbardziej efektywne, ale kosztowne – **metody eksperckie** wcześnie wychwytują część problemów i obniżają koszty późniejszych testów.
