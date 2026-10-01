# Metody oceny jakości interfejsu – klasyfikacja, typy metod

## Po co oceniać interfejs

Cel: wykryć problemy użyteczności i dostępności, zmierzyć jakość interfejsu, porównać warianty, uzasadnić zmiany. Badanie powinno być prowadzone cyklicznie (na prototypie i na gotowym produkcie – zob. UCD w materiałach dodatkowych).

## Klasyfikacja metod

```
                     metody oceny jakości interfejsu
                    ┌──────────────┴──────────────┐
              AUTOMATYCZNE                      MANUALNE
        (narzędzia komputerowe)         (ocena wykonywana przez człowieka)
                                      ┌─────────┴─────────┐
                              Z UDZIAŁEM                 BEZ UDZIAŁU
                              UŻYTKOWNIKÓW               UŻYTKOWNIKÓW
                              (ocena od użytkowników)    (ocena od ekspertów)
```

| Rodzaj | Opis |
| :--- | :--- |
| **Automatyczne** (*automated*) | procedurę oceny wykonują **narzędzia komputerowe** w całości lub w znacznej części; możliwe, jeśli istnieją **poddające się algorytmizacji wzorce–standardy** (np. walidatory kodu, WAVE – kontrola zgodności z WCAG) |
| **Manualne** (*manual*) | wykonywane **ręcznie przez człowieka**; mogą być wspomagane komputerowo, ale **główna część oceny (pozyskanie wiedzy)** należy do osoby oceniającej |

Manualne dzielą się na:

- **z udziałem użytkownika** – ocena pochodzi od **grupy użytkowników** (uczestników oceny),
- **bez udziału użytkownika** – ocena pochodzi od **ekspertów** w sprawach interfejsu oprogramowania.

## Typy metod oceny jakości

| Typ | Opis | Cechy |
| :--- | :--- | :--- |
| **Testowanie** (*testing*) | wykonywanie **zaplanowanych (w scenariuszach) interakcji** uczestników z interfejsem | etapy: plan badań (scenariusze, ich weryfikacja) → pozyskanie uczestników → realizacja z **obserwacją i pomiarem** → opracowanie wyników → wnioski; możliwe w normalnej eksploatacji (obserwacja/rejestracja działań) |
| **Inspekcja** (*inspection*) | przegląd interfejsu z użyciem **list kontrolnych, kryteriów lub analizy heurystycznej** | wykonują **eksperci**; cel: identyfikacja **potencjalnych problemów** |
| **Wywiad** (*survey*) | pozyskanie informacji o jakości (lub problemach) od użytkowników: **wywiady, ankiety, kwestionariusze** | uczestnicy: doświadczeni użytkownicy; możliwy w normalnej eksploatacji |
| **Modelowanie analityczne** (*analytical modeling*) | **prognozy jakości** przez budowę i użycie **modeli interakcji** użytkownika z interfejsem | formalne modele opisu interfejsu; **bardzo pracochłonne, rzadko stosowane** |
| **Symulacja** (*simulation*) | **modele komputerowe** interakcji do symulowania typowych działań użytkownika i zbierania danych | wymaga specjalistycznego oprogramowania; **rzadko stosowana** (duże koszty modeli) |

*(uzupełnienie)* Przykłady modelowania analitycznego: **GOMS/KLM** (przewidywanie czasu wykonania zadania), prawo **Fittsa** (czas dojścia do celu).

## Metryki oceny jakości interfejsu

**Metryka** – miara jakości; ocena **jakościowa i ilościowa**. Oceny jakościowe mapuje się na ilościowe: **binarne (0/1)**, skale **pięciostopniowe** lub **procentowe (0–100%)** (skale % – wynik analiz statystycznych wielu wypowiedzi, ankiet itp.).

### Klasyfikacja metryk

| Grupa | Opis | Przykłady |
| :--- | :--- | :--- |
| **Wydajnościowe** (*performance*) | skuteczność wykonywania zadań | **sukces zadania** (task success), **czas zadania** (time on task), **stopa błędów** (errors), **przyswajalność** (learnability – zmiana produktywności w trakcie pracy) |
| **Bazujące na problemach** (*issue-based*) | liczba i rodzaj wykrytych problemów | częstotliwość unikalnych problemów; unikalne problemy **na uczestnika**; **% uczestników** doświadczających problemu; **kategoryzacja** problemów (wg zadań/kategorii) |
| **Bazujące na ocenach użytkowników** (*self-reported*) | subiektywne oceny | ocena **po zadaniu** (post-task), **po sesji** (post-session, np. SUS), ocena **specyficznego aspektu** (np. dostosowanie do osób z niepełnosprawnością ruchową/wzrokową) |
| **Behawioralne i fizjologiczne** | zachowanie i reakcje organizmu | ruchy gałek ocznych (eyetracking), średnica źrenic, wyrazy twarzy, tętno, EDA/GSR |

### Szczegółowe metryki testowania (przykłady z wykładu)

- czas ukończenia zadań; **% pomyślnie ukończonych zadań**; % zadań ukończonych poprawnie w limicie czasu,
- czas poświęcony na obsługę błędów; **% błędów** w ogólnej liczbie operacji,
- liczba używanych funkcji i poleceń; **częstotliwość używania pomocy**; czas spędzony na dokumentacji,
- liczba powtórzeń lub nieudanych prób użycia funkcji; liczba wprowadzeń użytkownika w błąd,
- liczba poprawnie i błędnie wywołanych funkcji w określonym czasie,
- liczba komend dostępnych, ale **nigdy nieużywanych**; liczba sytuacji szukania alternatywnych rozwiązań,
- liczba sytuacji **rozproszenia uwagi**, **utraty kontroli** nad systemem, zgłoszeń **frustracji**.

### Trzy wymiary użyteczności (ISO 9241-11) a metryki

- **skuteczność** → binarny wskaźnik realizacji zadania, % ukończonych zadań, liczba błędów,
- **efektywność** → czas wykonania, liczba kliknięć, długość ścieżki,
- **satysfakcja** → kwestionariusze (SUS), oceny po zadaniu, (pośrednio) reakcje źrenic.

## Globalne miary użyteczności

Pozwalają **porównywać** różne interfejsy lub warianty jedną liczbą (np. **WUP**, **SUS**; często **kombinacja wielu wskaźników**).

**Problemy przy łączeniu wskaźników:** cechy jakościowe i ilościowe; różne **jednostki** (%, s, pkt, szt.); różne **skale** (1–5 pkt, 0–102 s); różne **wymagania** (min./max.); różna **istotność (waga)** (co ważniejsze: % ukończonych zadań czy czas?).

**Przykład konstrukcji (wykład):**

$$K=\sum_{j=1}^{k}w_j\left(D_j\frac{K_j}{K_j^{max}}+(1-D_j)\frac{K_j^{min}}{K_j}\right),\qquad \sum_{j=1}^{k}w_j=1$$

- $K_j$ – wartość $j$-tego wskaźnika, $w_j$ – jego **waga**,
- $D_j=1$ dla wskaźnika **maksymalizowanego** (im więcej, tym lepiej), $D_j=0$ dla **minimalizowanego** (np. czas, błędy),
- wskaźnik znormalizowany do przedziału $(0,1]$ (normalizacja ilorazowa – zob. przedmiot *Metody wnioskowania wielokryterialnego*, temat 1),
- **problem wyznaczenia wag** – np. metodą **AHP** (tamże, temat 3).

**SUM (Single Usability Measure)** – jedna miara użyteczności będąca funkcją m.in.: **czasu realizacji zadań**, **satysfakcji użytkownika (1–5)**, **osiągnięcia rezultatu (0/1)**, **liczby błędów**, **liczby kliknięć** (https://measuringu.com/SUM/).

## Wybór metody – zalecenia

- Najlepiej łączyć metody (np. **inspekcja eksperta** na wczesnym etapie + **testy z użytkownikami** na prototypie + **ankieta SUS** po teście + eyetracking dla krytycznych ekranów).
- Dobór zależy od **etapu projektu**, **budżetu**, **dostępności użytkowników** i **celu** (znalezienie problemów vs porównanie wariantów).

## Podsumowanie

- Metody: **automatyczne** (narzędzia) i **manualne** (z udziałem lub bez udziału użytkowników).
- **5 typów:** testowanie, inspekcja, wywiad, modelowanie analityczne, symulacja (ostatnie dwa rzadko stosowane).
- **4 grupy metryk:** wydajnościowe, bazujące na problemach, bazujące na ocenach użytkowników, behawioralne/fizjologiczne.
- Globalne miary (WUP, SUS, SUM) łączą wiele wskaźników; wymagają normalizacji i wag.
