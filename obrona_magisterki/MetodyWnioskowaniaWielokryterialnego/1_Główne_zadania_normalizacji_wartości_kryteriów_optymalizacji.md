# Główne zadania normalizacji wartości analizowanych kryteriów optymalizacji

## Czym jest normalizacja

**Normalizacja** (w polskiej literaturze także *unitaryzacja*, skalowanie) to **przekształcenie wartości kryteriów** na wspólną skalę, tak aby można je było ze sobą porównywać i łączyć (sumować, uśredniać, mierzyć odległości). Kryteria w macierzy decyzyjnej mają zwykle **różne jednostki i rzędy wielkości** (cena w złotych, waga w kg, ocena 1–5, czas w godzinach), więc bez normalizacji nie można ich sensownie zagregować.

## Główne zadania normalizacji

| Nr | Zadanie | Wyjaśnienie |
| :-: | :--- | :--- |
| 1 | **Sprowadzenie do wspólnej, bezwymiarowej skali** | usuwa wpływ jednostek miary; dodawanie złotych do kilogramów nie ma sensu, a po normalizacji tak |
| 2 | **Ujednolicenie kierunku preferencji** | stymulanty (więcej = lepiej) i destymulanty (mniej = lepiej) zamieniane tak, by dla wszystkich kryteriów „większa wartość = lepiej" (albo odwrotnie) |
| 3 | **Ograniczenie zakresu wartości do ustalonego przedziału** | najczęściej $[0,1]$ (lub 0–100); wartość znormalizowana to stopień spełnienia kryterium/użyteczność cząstkowa |
| 4 | **Wyrównanie wpływu kryteriów** | kryterium o dużych liczbowo wartościach (np. cena w tys. zł) nie zdominuje kryterium o małych (np. waga w kg) w sumie ważonej lub w odległościach (TOPSIS, punkt idealny); **wagi mają wyrażać ważność, a nie skalę liczbową** |
| 5 | **Umożliwienie stosowania wag i metod agregacji** | suma ważona, odległość od ideału, metody relacji przewyższania wymagają porównywalnych wartości |
| 6 | **Ujednolicenie typu danych** | kryteria jakościowe (opisowe, skala porządkowa) przekłada się na liczby, aby wprowadzić je do jednego modelu |
| 7 | **Poprawa stabilności numerycznej i zbieżności** | w optymalizacji (zwłaszcza gradientowej, w sieciach neuronowych) kryteria o skrajnie różnych skalach pogarszają działanie algorytmów |

Normalizacja **nie zmienia kolejności** wariantów w ramach jednego kryterium (przekształcenie monotoniczne), zmienia natomiast **proporcje** między kryteriami, a przez to potencjalnie końcowy ranking.

## Typy kryteriów a normalizacja

- **stymulanta** ($S$) – wzrost wartości jest korzystny,
- **destymulanta** ($D$) – wzrost wartości jest niekorzystny,
- **nominanta** – najlepsza jest wartość nominalna $x_{nom}$; często zamienia się ją na odległość $|x-x_{nom}|$ (destymulanta).

## Najczęściej stosowane metody

Oznaczenia: $x_{ij}$ – wartość $i$-tego wariantu według $j$-tego kryterium, $x_j^{\max}$, $x_j^{\min}$ – wartości skrajne w kolumnie.

| Metoda | Wzór (stymulanta) | Wzór (destymulanta) | Zakres | Uwagi |
| :--- | :--- | :--- | :-: | :--- |
| **Unitaryzacja zerowana** (min–max) | $z_{ij}=\dfrac{x_{ij}-x_j^{\min}}{x_j^{\max}-x_j^{\min}}$ | $z_{ij}=\dfrac{x_j^{\max}-x_{ij}}{x_j^{\max}-x_j^{\min}}$ | $[0,1]$ | najlepszy wariant = 1, najgorszy = 0; wrażliwa na wartości skrajne |
| **Ilorazowa (względem maksimum/minimum)** | $z_{ij}=\dfrac{x_{ij}}{x_j^{\max}}$ | $z_{ij}=\dfrac{x_j^{\min}}{x_{ij}}$ | $(0,1]$ | zachowuje proporcje; wymaga wartości dodatnich |
| **Względem sumy** | $z_{ij}=\dfrac{x_{ij}}{\sum_i x_{ij}}$ | $\dfrac{1/x_{ij}}{\sum_i 1/x_{ij}}$ | $[0,1]$, $\sum=1$ | udziały w całości (stosowana m.in. w AHP) |
| **Wektorowa (euklidesowa)** | $z_{ij}=\dfrac{x_{ij}}{\sqrt{\sum_i x_{ij}^2}}$ | – (kierunek uwzględnia się dalej) | $(0,1)$ | standard w **TOPSIS**; kolumna ma długość 1 |
| **Standaryzacja (z-score)** | $z_{ij}=\dfrac{x_{ij}-\bar{x}_j}{s_j}$ | $-z_{ij}$ | bez ustalonych granic | średnia 0, odchylenie 1; może dawać wartości ujemne |
| **Względem punktu idealnego i nadiru** | $z_{ij}=\dfrac{x_{ij}-m_j}{z^*_j-m_j}$ | to samo (z $z^*$ = najlepsza, $m$ = najgorsza) | $[0,1]$ | ma sens przy dowolnym typie kryterium; używana w metodach punktu idealnego |
| **Funkcje użyteczności / wartości** | $z_{ij}=u_j(x_{ij})$ | – | $[0,1]$ lub 0–100 | nieliniowe (np. odcinkowe), odzwierciedlają preferencje decydenta, progi |

Odległość względna od ideału (dla zadania min–max): $\ d_j(x)=\dfrac{z^*_j-f_j(x)}{z^*_j-m_j}\in[0,1]$, gdzie 0 = wartość idealna.

## Przykład

Cztery laptopy; kryteria: cena [zł] (destymulanta), wydajność [pkt] (stymulanta), waga [kg] (destymulanta):

| Wariant | Cena | Wydajność | Waga |
| :--- | :-: | :-: | :-: |
| A | 3000 | 80 | 2,0 |
| B | 4500 | 95 | 1,6 |
| C | 3600 | 90 | 1,8 |
| D | 5000 | 100 | 1,4 |

**Unitaryzacja zerowana** (wszystkie kryteria zamienione na „więcej = lepiej"):

| Wariant | Cena | Wydajność | Waga |
| :--- | :-: | :-: | :-: |
| A | 1,00 | 0,00 | 0,00 |
| B | 0,25 | 0,75 | 0,67 |
| C | 0,70 | 0,50 | 0,33 |
| D | 0,00 | 1,00 | 1,00 |

(np. cena C: $\frac{5000-3600}{5000-3000}=0{,}70$; wydajność B: $\frac{95-80}{100-80}=0{,}75$.)

**Wektorowa** dla wydajności: $\sqrt{80^2+95^2+90^2+100^2}=183{,}0$ → A: 0,437; B: 0,519; C: 0,492; D: 0,546.

Suma ważona po unitaryzacji z wagami $w=(0{,}5;\,0{,}3;\,0{,}2)$: A = 0,500; B = 0,483; **C = 0,567**; D = 0,500 → najlepszy wariant C. Bez normalizacji suma ważona z cenami rzędu tysięcy całkowicie zdominowałaby wynik – wagi nie miałyby sensu.

## Problemy i pułapki

- **Wybór metody wpływa na wynik** – różne normalizacje mogą dać różne rankingi; sprawdza się stabilność wyniku (analiza wrażliwości).
- **Wartości odstające** zaburzają min–max (jedna skrajna wartość ściska resztę do wąskiego przedziału) i z-score (zawyżone $s$).
- **Odwrócenie rang (rank reversal)**: dodanie/usunięcie wariantu zmienia $x^{\max}$, $x^{\min}$ i może zmienić ranking pozostałych (dotyczy normalizacji zależnych od zbioru wariantów).
- Przy normalizacji względem punktu idealnego/nadiru trzeba zdefiniować nadir (zwykle na zbiorze Pareto).
- Normalizacja **nie zastępuje wag**: wyrównuje skale, ale ważność kryteriów ustala decydent (np. AHP, temat 3).
- Kryteria jakościowe: skala porządkowa przeliczana na liczby (np. bdb = 5, db = 4, dst = 3) wprowadza arbitralne odległości między stopniami.

## Podsumowanie

- Normalizacja przekształca kryteria na wspólną, bezwymiarową skalę (zwykle $[0,1]$) i ujednolica kierunek preferencji.
- Główne zadania: porównywalność kryteriów, eliminacja wpływu jednostek, wyrównanie wpływu kryteriów, możliwość agregacji z wagami, zamiana destymulant na stymulanty, ujednolicenie danych jakościowych i ilościowych.
- Metody: unitaryzacja zerowana (min–max), ilorazowa, względem sumy, wektorowa (TOPSIS), z-score, względem ideału/nadiru, funkcje użyteczności.
- Wybór metody może wpłynąć na ranking – warto zrobić analizę wrażliwości.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Metoda_leksykograficzna.md)