# Metody redukcji wymiaru i liczności próby. Wymień znane metody i omów jedną z nich

> **💬 Gotowa wypowiedź ustna:**
> *"Redukcja wymiarowości i liczności danych zapobiega przekleństwu wymiarowości, które prowadzi do przeuczenia modeli i wysokich kosztów obliczeniowych. Redukcję liczności próby wykonujemy poprzez losowanie lub klasteryzację, natomiast redukcję wymiaru poprzez selekcję cech lub ich ekstrakcję.
> 
> Najważniejszą metodą ekstrakcji jest Analiza Składowych Głównych (PCA). Jest to metoda nienadzorowana, która zamienia skorelowane zmienne pierwotne w nowy układ nieskorelowanych zmiennych, zwanych składowymi głównymi. Pierwsza składowa przejmuje największą część wariancji danych, a kolejne coraz mniej. Liczbę składowych dobieramy na podstawie procentu wyjaśnionej wariancji, kryterium Kaisera lub wykresu osypiska. Zalety PCA to usunięcie współliniowości i spadek wymiaru, a wadą jest trudniejsza interpretacja nowych zmiennych."*

## 1. Cel i istota redukcji

- **Redukcja wymiaru** – zmniejszamy liczbę **cech (kolumn)**: usuwamy zmienne nieistotne lub powtarzające się albo tworzymy mniejszy zestaw nowych cech.
- **Redukcja liczności próby** – zmniejszamy liczbę **obserwacji (wierszy)**, zachowując reprezentatywność danych.
- **Klątwa wymiarowości** – im więcej cech, tym szybciej rośnie przestrzeń, w której leżą dane. Potrzeba wtedy znacznie więcej obserwacji, obliczenia trwają dłużej, a rośnie ryzyko **przeuczenia** modelu.
- **Korzyści:** szybsze uczenie modeli, mniej szumu, niższe koszty zbierania danych, łatwiejsza wizualizacja (np. na wykresie 2D lub 3D).

## 2. Przegląd znanych metod

### Redukcja wymiaru (cech)

**Selekcja cech** – wybieramy podzbiór istniejących zmiennych:

- **Filter (filtry)** – oceniamy cechy statystyką, bez udziału modelu (np. test chi-kwadrat, korelacja Pearsona, próg wariancji).
- **Wrapper (opakowane)** – sprawdzamy podzbiory cech, używając konkretnego modelu (np. rekurencyjna eliminacja cech, RFE).
- **Embedded (wbudowane)** – selekcja zachodzi w trakcie uczenia modelu (np. regularyzacja L1/Lasso, ważność cech w lasach losowych).

**Ekstrakcja cech** – tworzymy nowe cechy z istniejących:

- **PCA** – metoda nienadzorowana, nowe cechy uporządkowane według ilości informacji.
- **LDA** – metoda nadzorowana, nowe osie najlepiej rozdzielają klasy.
- **Inne:** ICA oraz metody nieliniowe (t-SNE, UMAP, mapy Kohonena SOM).

### Redukcja liczności próby (obserwacji)

- **Losowanie próby (sampling)** – proste, warstwowe lub systematyczne.
- **Agregacja i analiza skupień** – grupę podobnych obiektów zastępujemy jej reprezentantem (centroidem), np. w algorytmie k-średnich.
- **Czyszczenie danych** – usuwanie duplikatów i wartości odstających.
- **Redukcja klas w zbiorach niezbalansowanych** – podpróbkowanie (undersampling), np. Tomek Links.

## 3. Omówienie wybranej metody: PCA (Analiza Składowych Głównych)

### Zasada działania

PCA przekształca pierwotne, skorelowane zmienne w nowy zestaw **nieskorelowanych zmiennych**, zwanych **składowymi głównymi**. Każda składowa jest kombinacją liniową zmiennych pierwotnych. Są one uporządkowane od tej, która niesie najwięcej informacji (wyjaśnia największą część zmienności), do tej, która niesie najmniej. Zwykle zostawiamy kilka pierwszych i pomijamy resztę.

*Intuicja:* PCA szuka „najlepszego kąta patrzenia" na dane, czyli takiego kierunku, w którym dane są najbardziej rozproszone, i rzutuje na niego obserwacje, jak cień na ścianę.

### Kroki algorytmu

1. **Standaryzacja danych** – każda zmienna ma średnią 0 i odchylenie standardowe 1 (z-score), żeby żadna nie dominowała przez samą skalę.
2. **Macierz kowariancji (lub korelacji)** – opis zależności liniowych między wszystkimi parami zmiennych.
3. **Wartości i wektory własne:**
   - *wektory własne* wyznaczają nowe kierunki (osie),
   - *wartości własne* mówią, ile zmienności niesie dana składowa.
4. **Projekcja** – obserwacje rzutujemy na wybrane wektory własne i otrzymujemy dane o mniejszej liczbie wymiarów.

### Dobór liczby składowych

- **Procent wyjaśnionej wariancji** – zostawiamy tyle składowych, by łącznie wyjaśniały ustalony próg (np. 80–95%).
- **Kryterium Kaisera** – zostawiamy składowe o wartości własnej większej niż 1.
- **Wykres osypiska (scree plot)** – szukamy „łokcia", czyli punktu załamania wykresu, i odrzucamy składowe za nim.

### Zalety i wady

**Zalety:**

- usuwa współliniowość zmiennych,
- daje dużą redukcję wymiaru przy małej utracie informacji,
- jest przydatna do wizualizacji.

**Wady:**

- składowe trudniej zinterpretować niż oryginalne zmienne,
- wykrywa tylko zależności **liniowe**,
- jest wrażliwa na skalę zmiennych (stąd standaryzacja).

## Podsumowanie

- Redukcja = mniej zmiennych (wymiar) i/lub mniej obserwacji (liczność), co poprawia jakość, szybkość i czytelność analizy.
- Wymiar: **selekcja cech** (filter, wrapper, embedded) albo **ekstrakcja cech** (PCA, LDA).
- **PCA:** standaryzacja → macierz kowariancji → wektory i wartości własne → wybór liczby składowych (próg wariancji, Kaiser, osypisko) → projekcja.
- Liczność: próbkowanie, klastrowanie, usuwanie duplikatów i odstających, podpróbkowanie klas.

---
[⬅️ Poprzedni temat](4_Modele_regresji_Regresja_wielokrotna.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_Metody_analizy_skupień.md)