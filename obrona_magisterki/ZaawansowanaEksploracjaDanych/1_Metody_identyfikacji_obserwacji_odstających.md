# Metody identyfikacji obserwacji odstających

## Czym jest obserwacja odstająca

**Obserwacja odstająca (outlier, anomalia)** to wartość skrajna, leżąca blisko granic zakresu danych albo sprzeczna z ogólnym trendem pozostałych danych.

Dlaczego jest to ważne:

- może być **błędem zapisu** (np. różne skale pomiarowe, literówka, błąd czujnika),
- nawet jeśli jest poprawnym pomiarem, **metody wrażliwe na takie obserwacje dają niestabilne wyniki** (średnia, odchylenie standardowe, regresja, PCA, k-średnich),
- czasem to właśnie anomalie są celem analizy (wykrywanie oszustw, awarii, włamań).

Postępowanie po wykryciu: **zweryfikować źródło** → usunąć (jeśli błąd), zastąpić stałą (np. winsoryzacja, mediana) albo zostawić i użyć metod odpornych.

## Przegląd znanych metod

### Metody graficzne

- **histogram** – słupek oddalony od reszty,
- **wykres skrzynkowy (box plot)** – punkty poza „wąsami”,
- **wykres rozrzutu** (dla 2 zmiennych) – punkty oddalone od głównej chmury.

### Metody statystyczne

- **reguła $1{,}5\cdot IQR$** (Tukey),
- **standaryzacja / z-score** (reguła 3 sigm),
- zmodyfikowany z-score oparty na medianie i MAD (odporny),
- testy statystyczne: Grubbsa, Dixona (dla rozkładu normalnego),
- **odległość Mahalanobisa** (przypadek wielowymiarowy, uwzględnia korelacje).

### Metody uczenia maszynowego

- **analiza skupień** – obserwacje daleko od centrów klastrów lub niezaklasyfikowane do żadnego (np. **DBSCAN** oznacza je jako szum),
- **k najbliższych sąsiadów** – duża odległość do k-tego sąsiada,
- **LOF (Local Outlier Factor)** – lokalna gęstość porównywana z sąsiadami,
- **Isolation Forest** – drzewa decyzyjne izolujące punkty; anomalie izolują się szybciej,
- **One-Class SVM**,
- analiza reszt modelu regresji (odległość Cooka).

> Metody z wykładu: histogram, wykres skrzynkowy, wykres rozrzutu, standaryzacja (z-score), analiza skupień, Isolation Forest.

## Omówienie: reguła $1{,}5\cdot IQR$ (wykres skrzynkowy)

### Idea

Opiera się na **kwartylach**, więc jest **odporna** na same obserwacje odstające (kwartyle prawie nie reagują na skrajne wartości, w przeciwieństwie do średniej i odchylenia standardowego).

### Wzory

- $Q_1$ – kwartyl dolny (25% obserwacji jest mniejszych), $Q_3$ – kwartyl górny (75%),
- $IQR = Q_3 - Q_1$ – rozstęp międzykwartylowy; w „pudełku” mieści się 50% środkowych obserwacji.

Granice (ogrodzenia, *fences*):

$$\text{dolna: } Q_1 - 1{,}5\cdot IQR \qquad \text{górna: } Q_3 + 1{,}5\cdot IQR$$

| Typ | Warunek |
| :--- | :--- |
| **obserwacja odstająca** | poza przedziałem $[Q_1 - 1{,}5 IQR,\; Q_3 + 1{,}5 IQR]$ (na wykresie: kółko) |
| **obserwacja ekstremalnie odstająca** | poza przedziałem $[Q_1 - 3 IQR,\; Q_3 + 3 IQR]$ (na wykresie: gwiazdka) |

Wąsy sięgają do najdalszej obserwacji **mieszczącej się** w granicach; mediana to linia wewnątrz pudełka.

### Przykład z wykładu

Dane (wiek): `1, 5, 6, 6, 6, 7, 7, 7, 8, 8, 8, 9, 9, 11, 13, 15, 30` ($n=17$)

- mediana $=8$, $Q_1 = 6$, $Q_3 = 9$, więc $IQR = 3$,
- granice zwykłe: $6-4{,}5 = 1{,}5$ i $9+4{,}5 = 13{,}5$ → odstające: **1** i **15**,
- granice ekstremalne: $6-9=-3$ i $9+9=18$ → ekstremalnie odstająca: **30**.

## Porównanie: z-score

$$z_i = \frac{x_i - \bar{x}}{s}$$

Obserwacja jest podejrzana, gdy $|z_i| > 3$ (przy założeniu rozkładu normalnego odległość ponad 3 odchyleń standardowych od średniej). Można też przyjąć „wskazany procent najbardziej oddalonych obserwacji”.

Dla tych samych danych: $\bar{x} = 9{,}18$, $s = 6{,}22$; wartość 30 ma $z = 3{,}35$ → odstająca; wartości 1 ($z=-1{,}32$) i 15 ($z=0{,}94$) **nie** zostają wykryte.

| | Reguła IQR | z-score |
| :--- | :--- | :--- |
| Oparta na | kwartylach (pozycyjne) | średniej i odchyleniu std. |
| Odporność na outliery | **wysoka** | niska (outlier zawyża $s$ i „maskuje” siebie) |
| Założenie o rozkładzie | brak | (w przybliżeniu) normalny |
| Przypadek wielowymiarowy | każda zmienna osobno | każda zmienna osobno |

## Wielowymiarowe wykrywanie anomalii

- **Wykres rozrzutu** dwóch zmiennych – na wykładzie dwie obserwacje odstające $(-1{,}35;\ 30)$ i $(3{,}35;\ 30)$.
- Obserwacja może nie być odstająca w żadnym pojedynczym wymiarze, a być odstająca w kombinacji zmiennych (stąd Mahalanobis, LOF, Isolation Forest).
- **Isolation Forest**: buduje losowe drzewa dzielące dane na losowych cechach i progach; punkty anomalne są „rzadkie i różne”, więc **wymagają mniej podziałów** do odizolowania (krótka ścieżka w drzewie = wysoka ocena anomalii).
- **Metody skupieniowe**: DBSCAN – punkt, który nie jest ani rdzeniowy, ani graniczny, to szum (outlier).

## Podsumowanie

- Outlier = wartość odbiegająca od reszty; może być błędem lub cenną informacją.
- Najprostsze narzędzia: wykres skrzynkowy ($1{,}5 IQR$, $3 IQR$), z-score ($|z|>3$).
- Metody oparte na kwartylach są odporne; średnia i odchylenie standardowe – nie.
- W wielu wymiarach: odległość Mahalanobisa, klastrowanie (DBSCAN), LOF, Isolation Forest.
- Zawsze: sprawdzić, czy to błąd, zanim usuniemy obserwację.
