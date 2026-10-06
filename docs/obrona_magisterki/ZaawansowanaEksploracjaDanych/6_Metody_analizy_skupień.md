# Metody analizy skupień. Wymień znane metody i omów jedną z nich

> **💬 Gotowa wypowiedź ustna:**
> *"Analiza skupień to nienadzorowana metoda grupowania obiektów w jednorodne klastry bez znajomości wcześniejszych etykiet. Metody dzielimy na podziałowe, hierarchiczne dające dendrogram oraz gęstościowe, takie jak DBSCAN.
> 
> Naj popularniejszym algorytmem podziałowym jest algorytm $k$-średnich. Wymaga on podania liczby klastrów $k$. Działa w pętli: najpierw losuje $k$ centroidów, przypisuje każdy punkt do najbliższego centroidu, a następnie przelicza nowe położenie centroidów jako średnią punktów w danej grupie. Proces powtarza się do ustabilizowania wyników. Zalety to szybkość i prostota, a wady to konieczność wyboru $k$ oraz wrażliwość na wartości odstające."*

## 1. Istota i cel analizy skupień (klasteryzacji)

* **Definicja:** Analiza skupień to nienadzorowana metoda uczenia maszynowego służąca do podziału zbioru obiektów na jednorodne grupy (skupienia/klastry).
* **Główny cel:** Maksymalizacja podobobieństwa obiektów wewnątrz tego samego klastra (duża spójność wewnętrzna) przy jednoczesnej maksymalizacji różnic pomiędzy obiektami należącymi do różnych klastrów (duża separacja zewnętrzna).
* **Brak etykiet:** W przeciwieństwie do klasyfikacji, w analizie skupień z góry nie znamy prawdziwych etykiet klas ani prawidłowego podziału obiektów.

---

## 2. Przegląd znanych metod

* **Metody podziałowe (optymalizacyjne):**
  * *Algorytm $k$-średnich ($k$-means):* podział danych na $k$ rozłącznych sferycznych klastrów wokół ich centroidów.
  * *Algorytm $k$-medoidów (PAM):* wariant odporny na wartości odstające, gdzie centrum klastra stanowi rzeczywisty obiekt ze zbioru danych.
* **Metody hierarchiczne:**
  * *Aglomeracyjne (łączeniowe):* wychodzą z założenia, że każdy obiekt tworzy osobny klaster, a następnie iteracyjnie łączy się najbliższe grupy na podstawie wybranej odległości (np. metoda Warda, metoda najbliższego/najdalszego sąsiada). Wynik przedstawia się w postaci drzewa (dendrogramu).
  * *Dzielone (divisive):* rozpoczynają od jednego zbiorczego klastra i sukcesywnie dzielą go na mniejsze grupy.
* **Metody gęstościowe:**
  * *DBSCAN / OPTICS:* definiują klastry jako obszary o wysokiej gęstości punktów oddzielone obszarami o niskiej gęstości; automatycznie wykrywają szum/obserwacje odstające oraz klastry o dowolnych, nieregularnych kształtach.
* **Metody oparte na sieciach neuronowych:**
  * Samoorganizujące się mapy Kohonena (SOM).

---

## 3. Omówienie wybranej metody: Algorytm $k$-średnich ($k$-means)

* **Zasada działania:**  
  Jest to metoda podziałowa, w której z góry należy określić zakładaną liczbę klastrów $k$. Dąży do zminimalizowania sumy kwadratów odległości obiektów od środków klastrów (centroidów).

* **Przebieg algorytmu w krokach:**
  1. **Inicjalizacja:** Losowy wybór $k$ punktów w przestrzeni jako początkowych centrów (centroidów) klastrów.
  2. **Przypisanie:** Każdy obiekt ze zbioru zostaje przypisany do najbliższego centroidu (najczęściej na podstawie odległości euklidesowej).
  3. **Aktualizacja:** Dla każdego z $k$ klastrów oblicza się nowy centroid jako średnią arytmetyczną współrzędnych wszystkich przypisanych do niego punktów.
  4. **Iteracja:** Powtarzanie kroków przypisania i aktualizacji do momentu, gdy położenie centroidów przestanie się zmieniać (algorytm osiągnie zbieżność) lub zostanie osiągnięta maksymalna liczba iteracji.

* **Zalety:**
  * Bardzo prosta implementacja, wysoka szybkość i wydajność obliczeniowa nawet dla dużych zbiorów danych.
  * Łatwa interpretacja wyników.

* **Wady:**
  * Konieczność arbitralnego podania liczby klastrów $k$ z góry (często stosuje się "metodę łokcia" lub wskaźnik sylwetki/silhouette do jej ustalenia).
  * Wrażliwość na początkowy (losowy) wybór centroidów oraz na obecność wartości odstających.
  * Tendencja do tworzenia jedynie sferycznych (kulistych) klastrów o zbliżonych rozmiarach.

## Podsumowanie

- Skupienia: maksymalna jednorodność wewnątrz, maksymalne różnice między grupami; uczenie nienadzorowane.
- Rodziny: podziałowe (k-średnich), hierarchiczne (dendrogram), gęstościowe (DBSCAN, mean shift), siatkowe, modelowe (GMM, SOM).
- **k-średnich**: losowe centra → przypisanie do najbliższego → przeliczenie średnich → powtarzanie do stabilizacji; minimalizuje $\sum\lVert x-\mu_k\rVert^2$; trzeba podać $K$; kuliste grupy, wrażliwy na outliery i inicjalizację.
- **Hierarchiczne**: aglomeracja (single/complete/average/Ward), dendrogram, nie trzeba $K$.
- **DBSCAN**: $\varepsilon$ + `min_samples`; punkty rdzeniowe/brzegowe/szum; dowolne kształty i odporność na outliery.
- Przed klasteryzacją: standaryzacja zmiennych, ewentualnie PCA.

---
[⬅️ Poprzedni temat](5_Metody_redukcji_wymiaru_i_liczności_próby.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Systemy_rekomendacji.md)