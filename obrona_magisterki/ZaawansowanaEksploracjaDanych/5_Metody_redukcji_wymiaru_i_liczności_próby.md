# Metody redukcji wymiaru i liczności próby. Wymień znane metody i omów jedną z nich

## 1. Cel i istota redukcji wymiarowości i liczności próby

* **Problem "klątwy wymiarowości":** Wraz ze wzrostem liczby cech (wymiarów) wykładniczo rośnie objętość przestrzeni danych, co wymaga drastycznego zwiększenia liczby obserwacji, wydłuża czas obliczeń i wywołuje ryzyko przeuczenia modelu (overfittingu).
* **Redukcja wymiaru (cech/kolumn):** Polega na usunięciu zmiennych nieistotnych, nadmiarowych (skorelowanych) lub na utworzeniu nowego, mniej licznego zestawu syntetycznych cech.
* **Redukcja liczności próby (obserwacji/wierszy):** Polega na zmniejszeniu liczby obiektów (wierszy) w zbiorze danych przy zachowaniu reprezentatywności całej populacji.
* **Główne korzyści:** Skrócenie czasu trenowania modeli, usunięcie szumu, obniżenie kosztów pozyskiwania danych oraz możliwość łatwiejszej wizualizacji (np. na płaszczyźnie 2D/3D).

---

## 2. Przegląd znanych metod

* **Metody redukcji wymiaru (cech):**
  * **Selekcja cech (wybór podzbioru zmiennych):**
    * *Metody oparte na filtrach (Filter):* ocena zmiennych za pomocą statystyk bez udziału modelu (np. test $(\chi^2$), współczynnik korelacji Pearsona, progi wariancji).
    * *Metody opakowane (Wrapper):* iteracyjne sprawdzanie podzbiorów cech z użyciem konkretnego modelu (np. rekurencyjna eliminacja cech – RFE).
    * *Metody wbudowane (Embedded):* algorytmy z automatyczną selekcją w trakcie uczenia (np. regularyzacja L1 / Lasso, ważność cech w Lasach Losowych).
  * **Ekstrakcja i konstrukcja nowych cech (projekcja):**
    * *Analiza Składowych Głównych (PCA – Principal Component Analysis)* – metoda nienadzorowana.
    * *Liniowa Analiza Dyskryminacyjna (LDA)* – metoda nadzorowana.
    * *Analiza Składowych Niezależnych (ICA)* oraz metody nieliniowe (np. t-SNE, UMAP, mapy Kohonena SOM).
* **Metody redukcji liczności próby (obserwacji):**
  * **Losowanie próby (Sampling):** losowanie proste, warstwowe lub systematyczne.
  * **Agregacja i analiza skupień:** zastępowanie grup podobnych obiektów ich reprezentantami/centroidami (np. algorytm $(k$)-średnich).
  * **Czyszczenie danych:** usuwanie zduplikowanych rekordów oraz wartości odstających.
  * **Redukcja klas w zbiorach niezbalansowanych:** podpróbkowanie (Undersampling, np. Tomek Links).

---

## 3. Omówienie wybranej metody: Analiza Składowych Głównych (PCA)

* **Zasada działania:**  
  PCA to nienadzorowana technika przekształcania pierwotnych skorelowanych zmiennych w nowy układ ortogonalnych (nieskorelowanych) zmiennych, zwanych **składowymi głównymi**. Zmienne te są liniowymi kombinacjami zmiennych pierwotnych i są uporządkowane według ilości wyjaśnianej wariancji.

* **Kroki algorytmu PCA:**
  1. **Standaryzacja danych:** Przekształcenie zmiennych tak, aby miały średnią równą $(0$) i odchylenie standardowe równe $(1$) (metoda Z-score).
  2. **Wyznaczenie macierzy kowariancji (lub korelacji):** Ocena zależności liniowych pomiędzy wszystkimi parami zmiennych.
  3. **Obliczenie wartości własnych i wektorów własnych:**
     * *Wektory własne* wyznaczają nowe kierunki (osie) w przestrzeni danych.
     * *Wartości własne* określają wielkość wariancji (ilość informacji) przenoszoną przez daną składową.
  4. **Projekcja danych:** Rzutowanie oryginalnych obserwacji na przestrzeń wyznaczoną przez wybrane wektory własne.

* **Kryteria doboru liczby składowych:**
  * **Procent wyjaśnionej wariancji:** Wybiera się tyle pierwszych składowych, aby łącznie wyjaśniały przyjęty próg całkowitej wariancji (np. 80%–95%).
  * **Kryterium Kaisera:** Pozostawia się tylko te składowe, których wartości własne są większe od $(1$).
  * **Kryterium osypiska (Scree plot):** Na wykresie wartości własnych szuka się punktu załamania ("łokcia") i odrzuca składowe leżące po prawej stronie.

* **Zalety i ograniczenia:**
  * **Zalety:** Całkowite wyeliminowanie problemu współliniowości zmiennych oraz skuteczna redukcja wymiaru przy minimalnej utracie informacji.
  * **Wady:** Nowo powstałe składowe główne są trudniejsze w bezpośredniej interpretacji dziedzinowej niż oryginalne zmienne.

## Podsumowanie

- Redukcja = mniej zmiennych (wymiar) i/lub mniej obserwacji (liczność), aby poprawić jakość, szybkość i czytelność.
- Przekleństwo wymiarowości: liczba potrzebnych obserwacji rośnie wykładniczo z liczbą zmiennych.
- Wymiar: **selekcja cech** (filter / wrapper / embedded) albo **konstrukcja cech** (PCA).
- **PCA**: standaryzacja → macierz kowariancji → wektory i wartości własne → wybór $k$ składowych (90–95% wariancji, Kaiser $\lambda>1$, osypisko) → projekcja. Składowe są ortogonalne, wariancja składowej = wartość własna.
- Liczność: próbkowanie (proste, systematyczne, warstwowe), selekcja przykładów, klastrowanie, usuwanie duplikatów/odstających.

---
[⬅️ Poprzedni temat](4_Modele_regresji_Regresja_wielokrotna.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_Metody_analizy_skupień.md)