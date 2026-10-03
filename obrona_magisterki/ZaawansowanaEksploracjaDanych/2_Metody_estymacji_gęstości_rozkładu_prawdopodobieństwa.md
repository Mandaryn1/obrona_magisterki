# Metody estymacji gęstości rozkładu prawdopodobieństwa. Wymień znane metody i omów jedną z nich

## 1. Podział metod estymacji

* **Metody klasyczne (parametryczne):** polegają na przyjęciu z góry konkretnego typu rozkładu (np. rozkładu normalnego) oraz wyznaczeniu jego parametrów na podstawie próby losowej.
* **Metody nieparametryczne:** nie narzucają z góry postaci rozkładu i pozwalają odtworzyć funkcję gęstości bezpośrednio z posiadanych danych.

---

## 2. Znane metody nieparametryczne

* **Histogram:** najprostsza metoda, oparta na podziale przestrzeni danych na rozłączne przedziały klasowe i zliczaniu w nich obserwacji.
* **Estymatory najbliższego sąsiedztwa (\\(k\\)-NN):** konstrukcja oparta na odległości do \\(k\\)-tego najbliższego sąsiada.
* **Estymatory jądrowe (KDE):** wygładzanie gęstości za pomocą symetrycznych funkcji jąder nakładanych na punkty pomiarowe.

---

### 3. Omówienie wybranej metody: Histogram

* **Zasada działania:** Przedział zawierający dane dzieli się na rozłączne podprzedziały (klasy), a następnie zlicza się liczbę obserwacji lub wyznacza częstość występowania danych w każdym koszyku.
* **Rola szerokości przedziału (klasy):** Jest to kluczowy parametr mający decydujący wpływ na wynik:
  * **Zbyt szerokie przedziały:** maskują charakterystyczne cechy i rzeczywisty kształt rozkładu.
  * **Zbyt wąskie przedziały:** wywołują szum oraz pojawienie się wielu sztucznych ekstremów lokalnych.
* **Zalety:** Wyjątkowa łatwość konstrukcji oraz intuicyjna interpretacja, dzięki czemu jest to podstawowe narzędzie do wstępnej wizualizacji danych.
* **Wady:** Funkcja jest nieciągła (schodkowa), a jej pochodna jest równa zeru wewnątrz klas i nie istnieje na ich stykach, co utrudnia bardziej zaawansowaną analizę matematyczną.

---

## Podsumowanie

- Estymacja gęstości = odtworzenie $f(x)$ z próby; podejście parametryczne (wybór rozkładu) lub nieparametryczne.
- Nieparametryczne: histogram (prosty, ale nieciągły), k-NN (nieregularny), **jądrowy** (ciągły, gładki – najlepszy).
- Estymator jądrowy: $\hat f_h(x) = \frac{1}{nh}\sum K\big(\frac{x-X_i}{h}\big)$; **jądro mało ważne, $h$ kluczowe**.
- $h$ dobiera się minimalizując MISE (metody: przybliżona, podstawień, cross-validation).
- Modyfikacja $h$ pozwala dopasować wygładzenie do lokalnej gęstości.

---
[⬅️ Poprzedni temat](1_Metody_identyfikacji_obserwacji_odstających.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](3_Wnioskowanie_statystyczne_ANOVA_MANOVA.md)