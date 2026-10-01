# Metodyka SUS (System Usability Scale)

## Istota

**SUS (System Usability Scale)** – kwestionariuszowa metoda pomiaru **subiektywnego poziomu satysfakcji** użytkowników z systemu; jedna z najpopularniejszych **globalnych miar użyteczności**.

- Technika „**quick and dirty**" – szybka i tania ocena użyteczności przez użytkowników.
- Użytkownicy: (1) **realizują scenariusz** użycia systemu, (2) **wypełniają ankietę ewaluacyjną** (10 stwierdzeń).
- Skala **Likerta 1–5**: od „zdecydowanie się nie zgadzam" (1) do „zdecydowanie się zgadzam" (5).
- *(uzupełnienie)* Autor: **John Brooke** (1986). Niezależna od technologii: web, aplikacje desktopowe i mobilne, urządzenia, systemy ERP.
- Zaleta: mała liczba pytań, **wiarygodna już dla małych grup** (kilka–kilkanaście osób), wynik w jednej liczbie 0–100, porównywalność z innymi systemami.

## Ankieta SUS (10 stwierdzeń, wersja polska z wykładu)

| Nr | Stwierdzenie | Charakter |
| :-: | :--- | :-: |
| Q1 | Myślę, że chciałbym często używać tego systemu | pozytywne |
| Q2 | Uważam, że system jest niepotrzebnie zbyt skomplikowany | negatywne |
| Q3 | Uważam, że system jest łatwy w użyciu | pozytywne |
| Q4 | Myślę, że będę potrzebował pomocy specjalisty, aby móc w pełni używać tego systemu | negatywne |
| Q5 | Uważam, że funkcje systemu są dobrze zintegrowane | pozytywne |
| Q6 | Sądzę, że w systemie jest zbyt dużo niespójności | negatywne |
| Q7 | Oceniam, że większość osób bardzo szybko nauczy się używać tego systemu | pozytywne |
| Q8 | Uważam, że system jest bardzo niewygodny w użyciu | negatywne |
| Q9 | Czułem się pewnie korzystając z systemu | pozytywne |
| Q10 | Musiałbym sporo nauczyć się, zanim mógłbym zacząć swobodnie pracować z tym systemem | negatywne |

Pozycje **nieparzyste** są sformułowane pozytywnie, **parzyste** – negatywnie (aby ograniczyć odpowiadanie „na autopilocie").

## Obliczanie wyniku

Odpowiedź na pytanie $i$ oznaczamy $S_i\in\{1,\dots,5\}$.

$$SUS=\left(\sum_{i=1,3,5,7,9}(S_i-1)+\sum_{i=2,4,6,8,10}(5-S_i)\right)\cdot2{,}5$$

- pozycje **nieparzyste**: wkład $=S_i-1$,
- pozycje **parzyste**: wkład $=5-S_i$,
- każda pozycja daje 0–4 punkty; suma 0–40; mnożenie przez **2,5** daje **0–100** (to **nie** jest procent).

### Przykład

Odpowiedzi: Q1–Q10 $=(4,\,2,\,4,\,1,\,4,\,2,\,5,\,1,\,4,\,2)$.

- nieparzyste: $(4-1)+(4-1)+(4-1)+(5-1)+(4-1)=3+3+3+4+3=16$,
- parzyste: $(5-2)+(5-1)+(5-2)+(5-1)+(5-2)=3+4+3+4+3=17$,
- $SUS=(16+17)\cdot2{,}5=\mathbf{82{,}5}$.

Wynik systemu = **średnia z wyników wszystkich uczestników**.

## Interpretacja

- Średnia wartość SUS dla **500 różnych systemów** (wg wykładu) wynosi **68** – to punkt odniesienia: wynik powyżej 68 = lepiej niż przeciętnie.
- *(uzupełnienie)* Orientacyjna interpretacja wyników (Sauro, Lewis): ok. **>80** – bardzo dobra użyteczność (oceny A, „doskonały"), ok. **68–80** – dobra, **<68** – poniżej średniej, **<51** – słaba. Wynik SUS nie wskazuje, **co** jest nie tak – wskazuje tylko, **czy** i **jak bardzo** użyteczność jest dobra; do diagnozy potrzebne są testy, obserwacje, heurystyki.
- **Dwa czynniki:** SUS w rzeczywistości mierzy dwa czynniki: **użyteczność** (8 pozycji) oraz **możliwość nauczenia się** (2 pozycje – **Q4 i Q10**) – można je liczyć osobno.

## Zastosowanie w badaniu

1. Uczestnik wykonuje scenariusze zadań w systemie.
2. Natychmiast po sesji wypełnia SUS (bez długiego namysłu, pierwsze odpowiedzi; niepominięte pozycje).
3. Wyniki są liczone i uśredniane; porównywane między wersjami/systemami; raportowane w **raporcie z badań** (ISO 25062) jako miara satysfakcji.
4. Często łączony z metrykami wydajnościowymi (czas, sukces) i eyetrackingiem; wchodzi do **globalnych miar** (WUP, SUM) – zob. temat 6.

## SUS na tle innych miar subiektywnych

| Miara | Charakter |
| :--- | :--- |
| **SUS** | 10 pytań, jedna wartość 0–100, ocena po sesji |
| ocena po zadaniu (post-task) | po każdym zadaniu (np. 1 pytanie o trudność, SEQ) |
| **SUM** | jedna miara łącząca czas, satysfakcję, sukces, błędy, kliknięcia |
| **WUP** | ocena eksperta na podstawie listy kontrolnej (nie użytkowników) |

## Zalety i ograniczenia

| Zalety | Ograniczenia |
| :--- | :--- |
| prosta, szybka, tania, **bezpłatna** | mierzy **subiektywne** odczucie – nie wskazuje konkretnych problemów |
| działa dla różnych systemów i wielkości prób | ogólna – nie mierzy np. dostępności ani szczegółów (wymaga uzupełnienia) |
| wynik porównywalny z benchmarkiem (68) | wynik zależy od kontekstu, doświadczenia użytkowników i sposobu sesji |
| wiarygodność i szeroko potwierdzona | sformułowania negatywne mogą być mylące (błędne odpowiedzi) |

## Podsumowanie

- SUS = 10 pytań (Likert 1–5), na przemian pozytywne (nieparzyste) i negatywne (parzyste).
- Wzór: $SUS=\big(\sum_{nieparz.}(S_i-1)+\sum_{parz.}(5-S_i)\big)\cdot2{,}5$ → wynik **0–100**.
- Średnia dla 500 systemów = **68**; dwa czynniki: użyteczność (8 pozycji) i możliwość nauczenia (Q4, Q10).
- Szybka metoda oceny satysfakcji po wykonaniu scenariusza; uzupełnia metryki wydajnościowe i testy.
