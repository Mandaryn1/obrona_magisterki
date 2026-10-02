# Metody analizy skupień

## Czym jest analiza skupień

**Analiza skupień (clustering, klasteryzacja, grupowanie)** to metoda **uczenia nienadzorowanego** (bez zmiennej docelowej / etykiet). Polega na podziale obiektów na **rozłączne, możliwie jednorodne grupy (skupienia, klastry)** tak, aby:

- podobieństwo obiektów **wewnątrz** grupy było **maksymalne**,
- podobieństwo do obiektów **spoza** grupy było **minimalne**.

Termin wprowadził Tryon (1939). Polski wkład: Jan Czekanowski (diagramowa metoda klasyfikacji, 1913), Hugo Steinhaus, Zdzisław Hellwig (wrocławska szkoła taksonomiczna).

Cel: wykrycie regularności i „naturalnych” (dających się sensownie zinterpretować) struktur w danych.

### Zastosowania

- eksploracja danych: segmentacja klientów,
- wyszukiwanie informacji: klasyfikacja książek, stron WWW,
- wstępna analiza danych (wyodrębnienie jednorodnych grup do dalszej analizy),
- segmentacja obrazu (np. po kolorze, intensywności),
- kompresja obrazów i sygnałów,
- redukcja ilości danych do kilku kategorii, poznanie nieznanej struktury, porównywanie obiektów o wielu cechach.

### Decyzje wstępne

- **miara podobieństwa/odległości** (euklidesowa, Manhattan, Mahalanobisa, cosinusowa; dla zmiennych jakościowych np. Gower, Jaccard),
- kodowanie zmiennych jakościowych,
- **standaryzacja/normalizacja** zmiennych ilościowych (inaczej zmienne o dużych wartościach dominują),
- wybór cech najlepiej charakteryzujących obiekty; przy $>3$ cechach można zredukować wymiar **PCA**,
- liczba skupień (pomocna wizualizacja).

### Dlaczego nie przeszukać wszystkich podziałów

Liczba podziałów $n$ obiektów na $K$ niepustych skupień to liczba Stirlinga drugiego rodzaju, $\approx K^n/K!$. Dla $n=100$, $K=4$ jest to ok. $7\cdot10^{58}$ → bezpośrednie przeszukanie niemożliwe, potrzebne algorytmy heurystyczne.

## Klasyfikacja metod

| Grupa metod | Idea | Przykłady |
| :--- | :--- | :--- |
| **Podziałowo-optymalizacyjne** (kombinatoryczne) | podział na zadaną liczbę $K$ skupień wg kryterium | **k-średnich**, k-medoidów (PAM), k-medians |
| **Hierarchiczne** | drzewiasta struktura skupień (dendrogram); aglomeracyjne i deglomeracyjne | AGNES, DIANA, linkage (single, complete, average, Ward) |
| **Gęstościowe** (density-based) | skupienia = obszary o większej gęstości obserwacji | **DBSCAN**, OPTICS, mean shift |
| **Siatkowe** (grid-based) | wielowymiarowy podział przestrzeni siatką | STING, CLIQUE |
| **Modelowe** (model-based) | hipoteza o modelu skupienia i jego estymacja | mieszaniny gaussowskie (EM), **sieci Kohonena (SOM)** |

## Omówienie: algorytm k-średnich (k-means)

### Sformułowanie

Mamy $n$ obserwacji $x_i\in\mathbb{R}^p$ i chcemy je podzielić na $K$ skupień. Suma kwadratów odległości między wszystkimi parami punktów $T$ jest **stała** i rozkłada się na część wewnątrz- i międzygrupową:

$$T=W+B$$

($W$ – suma kwadratów odległości par w tym samym skupieniu, $B$ – w różnych skupieniach). Skoro $T$ stałe → **minimalizacja $W$ ⇔ maksymalizacja $B$**. Można pokazać, że jest to równoważne minimalizacji sumy kwadratów odległości punktów od **środków (centroidów) skupień**:

$$J=\sum_{k=1}^{K}\sum_{x_i\in C_k}\lVert x_i-\mu_k\rVert^2,\qquad \mu_k=\frac{1}{n_k}\sum_{x_i\in C_k}x_i$$

### Algorytm

1. Ustal liczbę skupień $K$.
2. Wybierz **początkowe centra** $\mu_1,\dots,\mu_K$ (zwykle losowo spośród punktów próby; wariant ulepszony: k-means++).
3. **Przypisz** każdy obiekt do skupienia o najbliższym środku.
4. **Przelicz** nowe środki jako środki ciężkości (średnie) punktów skupień.
5. Powtarzaj kroki 3–4, aż przypisanie obiektów przestanie się zmieniać.

### Własności

- Wszystkie wersje są **zbieżne**, ale **nie gwarantują** minimum globalnego → uruchamia się algorytm wiele razy z różnymi punktami startowymi i wybiera najlepszy wynik (najmniejsze $J$).
- Zakłada skupienia **kuliste (wypukłe)**, o zbliżonej wielkości.
- Złożoność liniowa względem $n$ – skalowalny.

### Wady i zalety

| Zalety | Wady |
| :--- | :--- |
| prosty, szybki, skalowalny | trzeba z góry podać $K$ |
| łatwa interpretacja (centroidy) | wrażliwy na inicjalizację (minimum lokalne) |
| dobry dla zwartych, kulistych grup | **wrażliwy na obserwacje odstające** (średnia) |
| | nie radzi sobie z grupami niewypukłymi i różnej gęstości; wymaga standaryzacji |

**Dobór $K$**: metoda łokcia (*elbow* – wykres $J$ od $K$), współczynnik sylwetki (silhouette), indeks Daviesa-Bouldina, wizualizacja (PCA).

*Przykład z wykładu: trzy gatunki chrząszczy skaczących (6 pomiarów, rzut na 2 pierwsze składowe główne) – k-średnich osiągnęły dokładność ok. 97%.*

## Metody hierarchiczne

Nie wymagają z góry liczby skupień. Wykorzystują **miary niepodobieństwa między skupieniami**.

### Miary odległości między skupieniami (linkage)

| Metoda | Odległość między skupieniami $i$ i $j$ | Charakter |
| :--- | :--- | :--- |
| **najbliższego sąsiada** (single) | minimum odległości między parami z różnych skupień | skłonność do „łańcuchowania” |
| **najdalszego sąsiada** (complete) | maksimum | zwarte, podobnej średnicy skupienia |
| **średnia** (average) | średnia odległości par | kompromis |
| **Warda** | minimalny przyrost sumy kwadratów wewnątrzgrupowych | zwarte, podobnej liczności |

### Metoda aglomeracyjna (od dołu do góry)

1. Każdy obiekt = osobne skupienie; policz **macierz odległości**.
2. Znajdź **najbliższą parę** skupień, połącz je w jedno, uaktualnij macierz.
3. Powtarzaj, aż zostanie jedno skupienie.

Wynik: **dendrogram** (liście = obiekty; węzły na wysokości równej niepodobieństwu łączonych skupień). **Liczbę skupień** wybiera się „tnąc” dendrogram na wybranej wysokości (progowa wartość niepodobieństwa).

### Metoda deglomeracyjna (od góry do dołu)

Zaczyna od jednego skupienia obejmującego wszystko i dzieli je coraz drobniej. Bardziej kosztowna obliczeniowo, w praktyce **rzadko stosowana**.

**Zalety**: szybka, uniwersalna (dane ilościowe i jakościowe), pokazuje całą hierarchię. **Wady**: zasadniczy wpływ wyboru miary niepodobieństwa, decyzje łączenia są **nieodwracalne**, kosztowna dla dużych $n$ ($O(n^2)$ pamięci). *Przykład: dla chrząszczy dendrogram dał 3 skupienia (19, 22, 30 elementów); ok. 96% poprawnie.*

## Metody gęstościowe

### Idea (Fukunaga i Hostetler, 1975)

Wyznacz **estymator jądrowy gęstości** rozkładu próby (zob. temat 2). Skupienia odpowiadają **modom** (lokalnym maksimom) tego estymatora. Elementy przypisuje się do skupień **przesuwając je w kierunku gradientu** gęstości (iteracyjnie, jak w metodzie Newtona, tzw. mean shift), aż dotrą do maksimum. Do estymacji używa się jądra produktowego z jąder normalnych; parametry wygładzania dobiera się metodą podstawień.

### DBSCAN

**DBSCAN (Density-Based Spatial Clustering of Applications with Noise)** buduje skupienia na podstawie **zagęszczenia** obserwacji.

**Parametry**:

- $\varepsilon$ (`eps`) – promień sąsiedztwa (maksymalna odległość, by uznać obserwacje za sąsiadów),
- `min_samples` – minimalna liczba obserwacji w sąsiedztwie, by punkt był **rdzeniowy**.

**Rodzaje punktów**:

- **rdzeniowy (core)** – w promieniu $\varepsilon$ ma co najmniej `min_samples` sąsiadów,
- **brzegowy/graniczny (border)** – nie jest rdzeniowy, ale leży w sąsiedztwie punktu rdzeniowego,
- **szum (noise)** – ani rdzeniowy, ani brzegowy → **obserwacja odstająca**.

**Algorytm**:

1. Dla każdej obserwacji znajdź sąsiadów w odległości $<\varepsilon$.
2. Punkty z co najmniej `min_samples` sąsiadami to punkty rdzeniowe.
3. Połącz w jedno skupienie punkty rdzeniowe, które są ze sobą w zasięgu $\varepsilon$ (osiągalność gęstościowa).
4. Dołącz punkty brzegowe do istniejących skupień.
5. Pozostałe obserwacje → szum.

| Zalety | Wady |
| :--- | :--- |
| **nie wymaga podania liczby skupień** | trudny dobór $\varepsilon$ i `min_samples` (brak jednej sprawdzonej metody) |
| skupienia o **dowolnym (niewypukłym)** kształcie | kłopot z grupami o **różnej gęstości** |
| **odporny na obserwacje odstające**, wykrywa szum | słabiej w wysokich wymiarach (przekleństwo wymiarowości) |
| dowolna miara odległości, stosunkowo szybki | nie przypisuje wszystkich punktów do grup |

### DBSCAN vs k-średnich

| | k-średnich | DBSCAN |
| :--- | :--- | :--- |
| liczba skupień | podawana z góry | wynika z danych i parametrów |
| kształt skupień | kuliste, wypukłe | dowolne |
| outliery | wpływają na centroidy | oznaczane jako szum |
| przypisanie | każdy punkt do jakiegoś skupienia | punkty szumu poza skupieniami |

## Sieci Kohonena (mapy samoorganizujące SOM)

Sieć neuronowa uczona **bez nauczyciela** (nienadzorowana). Uczy się struktury danych, wykrywa skupienia i **rzutuje dane wielowymiarowe na prostokątną siatkę neuronów** (mapa topologiczna) tak, że **podobne obiekty trafiają blisko siebie**.

- Dwie warstwy: **wejściowa** i **wyjściowa** (neurony radialne; każdy neuron wyjściowy = skupienie), wagi inicjalizowane losowo; neuron to „narzędzie liczące odległość” wektora wejściowego od wektora wag.

**Uczenie** (wiele epok, każdy przypadek uczący prezentowany po kolei):

1. wszystkie neurony liczą odpowiedź na wejście,
2. wybierany jest **neuron zwycięski** (najbliższe centrum / największa aktywacja),
3. wagi zwycięzcy są przesuwane w stronę przypadku uczącego (ważona suma centrum i przypadku; współczynnik uczenia),
4. w mniejszym stopniu modyfikowani są **sąsiedzi** zwycięzcy (zgodnie z topologią siatki).

Sąsiedztwo i współczynnik uczenia maleją w czasie. Efekt: **uporządkowanie topologiczne** mapy.

Zastosowania: eksploracyjna analiza danych, klasyfikacja bezwzorcowa (np. skupiska podobnych miast po 24 parametrach komfortu życia), **detektor nowości** (nowe dane niepodobne do znanych klas nie zostaną rozpoznane).

## Powiązanie: analiza dyskryminacji (metoda nadzorowana)

Dla odróżnienia – **analiza dyskryminacji** klasyfikuje obiekty do **z góry zdefiniowanych** grup (znana przynależność w zbiorze uczącym), na podstawie **funkcji dyskryminacyjnych** (liniowych kombinacji zmiennych, maksymalizujących stosunek zmienności międzygrupowej do wewnątrzgrupowej – problem wartości własnych). Liczba funkcji = $\min(p,\ g-1)$. Ocena: lambda Wilksa, korelacja kanoniczna. Klasyfikacja nowych obiektów: funkcje klasyfikacyjne (największa wartość) lub odległość Mahalanobisa od centroidów grup. Analiza skupień **tworzy** grupy, dyskryminacja **przydziela** do istniejących.

## Podsumowanie

- Skupienia: maksymalna jednorodność wewnątrz, maksymalne różnice między grupami; uczenie nienadzorowane.
- Rodziny: podziałowe (k-średnich), hierarchiczne (dendrogram), gęstościowe (DBSCAN, mean shift), siatkowe, modelowe (GMM, SOM).
- **k-średnich**: losowe centra → przypisanie do najbliższego → przeliczenie średnich → powtarzanie do stabilizacji; minimalizuje $\sum\lVert x-\mu_k\rVert^2$; trzeba podać $K$; kuliste grupy, wrażliwy na outliery i inicjalizację.
- **Hierarchiczne**: aglomeracja (single/complete/average/Ward), dendrogram, nie trzeba $K$.
- **DBSCAN**: $\varepsilon$ + `min_samples`; punkty rdzeniowe/brzegowe/szum; dowolne kształty i odporność na outliery.
- Przed klasteryzacją: standaryzacja zmiennych, ewentualnie PCA.

---
[⬅️ Poprzedni temat](5_Metody_redukcji_wymiaru_i_liczności_próby.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Systemy_rekomendacji.md)