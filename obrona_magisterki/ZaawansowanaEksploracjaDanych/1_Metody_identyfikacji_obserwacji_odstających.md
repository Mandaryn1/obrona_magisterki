# Metody identyfikacji obserwacji odstających. Wymień znane metody i omów jedną z nich

## 1. Definicja i znaczenie obserwacji odstających

* **Obserwacje odstające (outliers):** to skrajne wartości w zbiorze danych, które leżą blisko granic zakresu zmiennej lub są sprzeczne z ogólnym trendem pozostałych danych.
* **Przyczyny i wpływ:** Mogą wynikać z błędów w zapisie danych lub być poprawnymi pomiarami nietypowych zjawisk. Identyfikacja jest kluczowa, ponieważ statystyki nieodporne (np. średnia arytmetyczna czy odchylenie standardowe) dają przy nich niestabilne i zniekształcone wyniki.

---

## 2. Znane metody identyfikacji

* **Metody graficzne:**
  * Wykres pudełkowy / skrzynkowy (box-plot).
  * Histogram (wykrywanie pojedynczych, oddalonych słupków).
  * Dwuwymiarowy wykres rozrzutu (dla analizy wielowymiarowej).
* **Metody statystyczne / parametryczne:**
  * Standaryzacja (wartość z-score / reguła 3 sigm) – uznająca za odstające wartości mniejsze od -3 lub większe od 3.
* **Metody oparte na kwartylach:**
  * Reguła rozstępu międzykwartylowego (1.5xIQR oraz 3xIQR).
* **Metody oparte na uczeniu maszynowym:**
  * Algorytmy analizy skupień (np. DBSCAN, w którym punkty nieprzypisane do żadnej grupy stanowią szum/odstające).
  * Algorytm Isolation Forest (drzewa izolacyjne).

---

## 3. Omówienie wybranej metody: Wykres pudełkowy i reguła IQR

* **Konstrukcja i wskaźniki:**
  * Metoda opiera się na kwartylach: kwartylu dolnym \\(Q_1\\) (25% obserwacji) oraz kwartylu górnym \\(Q_3\\) (75% obserwacji).
  * Oblicza się **rozstęp międzykwartylowy**: \\(IQR = Q_3 - Q_1\\).
* **Kryterium kwalifikacji obserwacji:**
  * **Obserwacja odstająca:** wartość leżąca w odległości większej niż \\(1.5 \cdot IQR\\) poniżej kwartyla dolnego lub powyżej kwartyla górnego (poza zakresem wąsów).
  * **Obserwacja ekstremalnie odstająca:** wartość oddalona od kwartyli o więcej niż \\(3 \cdot IQR\\).
* **Główne zalety:**
  * **Odporność (robustness):** W przeciwieństwie do metody z-score opartej na średniej, kwartyle i mediana są odporne na obecność samych wartości ekstremalnych, co daje stabilne granice detekcji.
  * Prosta wizualizacja i jednoznaczna interpretacja graficzna.

## Podsumowanie

- Outlier = wartość odbiegająca od reszty; może być błędem lub cenną informacją.
- Najprostsze narzędzia: wykres skrzynkowy ($1{,}5 IQR$, $3 IQR$), z-score ($|z|>3$).
- Metody oparte na kwartylach są odporne; średnia i odchylenie standardowe – nie.
- W wielu wymiarach: odległość Mahalanobisa, klastrowanie (DBSCAN), LOF, Isolation Forest.
- Zawsze: sprawdzić, czy to błąd, zanim usuniemy obserwację.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Metody_estymacji_gęstości_rozkładu_prawdopodobieństwa.md)