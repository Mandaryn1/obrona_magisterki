# Metody redukcji wymiaru i liczności próby

## Po co redukować dane

Algorytmy eksploracji danych są bardzo czułe na jakość i ilość danych. Redukcja może dotyczyć:

- **wymiaru** (liczby zmiennych/cech/kolumn),
- **liczności próby** (liczby obserwacji/wierszy).

### Przekleństwo wymiarowości

Wraz z każdą nową zmienną **gwałtownie (wykładniczo) rośnie liczba możliwych kombinacji wartości**, a więc liczba obserwacji potrzebnych do rzetelnego opisu przestrzeni. Problemy łatwe w przestrzeni o małej liczbie wymiarów stają się trudne w wielowymiarowej (dane stają się „rzadkie”, odległości tracą sens, rośnie ryzyko przeuczenia).

### Korzyści z redukcji wymiaru

- poprawa zdolności predykcyjnych (mniej szumu, mniejsze ryzyko przeuczenia),
- zwiększenie wydajności obliczeniowej,
- zmniejszenie wymagań i kosztów zbierania danych (np. mniej testów diagnostycznych),
- prostszy, bardziej czytelny model (inspekcja przez eksperta),
- możliwość **wizualizacji** (redukcja do 2–3 wymiarów).

## Przygotowanie danych (przed redukcją)

- Usuwanie **braków** (zasada minimalnej zmiany rozkładu): zastąpienie średnią/medianą/dominantą; usunięcie obserwacji (gdy $\le$ ok. 20% i dużo danych); usunięcie zmiennych (braki powyżej ok. 25%); uzupełnienie na podstawie innych zmiennych.
- Usuwanie **duplikatów** i danych **błędnych/niespójnych**.
- Usuwanie lub zastępowanie **wartości odstających** (zob. temat 1).
- **Standaryzacja** (z-score): $z=\frac{x-\bar{x}}{s}$ → średnia 0, odch. std. 1 (`StandardScaler`). Wymagana przez wiele metod (PCA, k-średnich, SVM, regresje z regularyzacją).
- **Normalizacja** próbek do normy jednostkowej (`normalize`, normy $L_1$/$L_2$) – przydatna do mierzenia podobieństwa par próbek. Odróżnić od skalowania min–max do $[0,1]$.

## Redukcja wymiaru – rodzaje podejść

| Podejście | Opis | Przykłady |
| :--- | :--- | :--- |
| **Selekcja cech** | wybór **podzbioru** istniejących cech | filter, wrapper, embedded |
| **Konstrukcja/ekstrakcja nowych cech** | nowe cechy jako kombinacje starych (projekcja) | **PCA**, LDA, ICA, SOM, autoenkodery, t-SNE/UMAP (wizualizacja) |
| **Wiedza dziedzinowa** | ręczne tworzenie/łączenie cech | ekspert |

### Rodzaje atrybutów

- **istotne (relevant)** – wpływają na wynik, nie da się ich zastąpić,
- **nieistotne (irrelevant)** – nie niosą informacji (szum),
- **nadmiarowe (redundant)** – nie wnoszą nic ponad inne wybrane (są zastępowalne, np. silnie skorelowane).

## Selekcja cech

Cel: najmniejszy podzbiór atrybutów, dla którego rozkład klas jest jak najbliższy oryginalnemu (lub klasyfikator ma jak największą trafność).

Proste kroki: usunięcie kolumn o zbyt małej zmienności (niemal stała wartość), analiza korelacji (zmienna wyjściowa powinna korelować z wejściowymi, **wejściowe nie powinny korelować ze sobą**).

| Rodzaj | Zasada | Zalety | Wady |
| :--- | :--- | :--- | :--- |
| **Filter** (filtry) | ocena cech niezależnie od modelu: korelacja $r$, $\chi^2$, informacja wzajemna, test $F$ (`SelectKBest`) | szybkie, dobrze wskazują istotne | nie optymalizują predykcji |
| **Wrapper** (opakowane) | szukanie podzbioru z użyciem modelu, np. **RFE** (rekurencyjna eliminacja cech: uczymy model, usuwamy najmniej ważne, powtarzamy) | poprawiają predykcję | **bardzo wolne** |
| **Embedded** (wbudowane) | selekcja w trakcie uczenia: Lasso (L1), ważność cech w Random Forest | kompromis | tylko dla wybranych algorytmów |

Korelacje: dwie zmienne numeryczne (Pearson/Spearman), dwie kategoryczne ($\chi^2$, V Cramera), numeryczna–kategoryczna (ANOVA, korelacja punktowo-dwuseryjna). Korelacja ≠ przyczynowość.

## Analiza składowych głównych PCA (omówienie)

### Idea

**PCA (Principal Component Analysis)** – najczęściej stosowana metoda **nienadzorowanej** redukcji wymiaru. Jest **ortogonalnym przekształceniem** oryginalnych, skorelowanych zmiennych w nowy zestaw **nieskorelowanych** zmiennych – **składowych głównych**. Każda jest **liniową kombinacją** zmiennych pierwotnych.

- Składowe porządkuje się według **wariancji**, którą wyjaśniają.
- 1. składowa maksymalizuje wariancję; 2. maksymalizuje wariancję **niewyjaśnioną przez poprzednią** (i jest do niej ortogonalna); itd.
- Zachowujemy pierwsze $k\ll p$ składowych – tracimy jak najmniej informacji.
- Geometrycznie: obrót układu współrzędnych tak, by osie były kierunkami maksymalnej zmienności („rzutowanie wzdłuż kierunków największego rozrzutu”). Rzut na kierunek prostopadły do głównego trendu dałby dużą stratę informacji.

### Kroki algorytmu

1. **Standaryzacja** zmiennych (z-score). Wtedy macierz kowariancji = macierz korelacji.
2. Obliczenie **macierzy kowariancji** $\mathbf{S}$ (lub korelacji $\mathbf{R}$) zmiennych.
3. Wyznaczenie **wartości własnych** $\lambda_1\ge\lambda_2\ge\dots\ge\lambda_p$ i **wektorów własnych** $\mathbf{v}_i$ macierzy: $\ (\mathbf{S}-\lambda\mathbf{I})\mathbf{v}=\mathbf{0}$.
4. Wybór $k$ wektorów własnych o największych wartościach własnych.
5. **Projekcja** danych: $\ PC_i=\mathbf{v}_i^T\mathbf{z}$.

Własności:

- wektor własny = kierunek (współczynniki kombinacji liniowej, „ładunki”), wartość własna = **wariancja** składowej,
- składowe są ortogonalne (nieskorelowane),
- całkowita wariancja = suma wartości własnych = $p$ (dla danych standaryzowanych, liczba zmiennych):

$$\text{udział } i\text{-tej składowej}=\frac{\lambda_i}{\sum_{j=1}^{p}\lambda_j}$$

### Dobór liczby składowych $k$

1. **procent wyjaśnionej wariancji** – kumulowany 90–95% (lub inny próg),
2. **kryterium Kaisera** – składowe o wartości własnej $\lambda>1$ (przy standaryzacji),
3. **kryterium osypiska** (*scree plot*) – punkt „załamania” wykresu wartości własnych,
4. **kryterium zasobu zmienności wspólnej** (komunalności) – dla każdej zmiennej suma kwadratów ładunków po wybranych składowych powinna przekraczać 0,5.

### Założenia i uwagi praktyczne

- zmienne ilościowe (co najmniej skala przedziałowa), **liniowe zależności** (korelacja Pearsona),
- zmienne powinny być skorelowane. Jeśli wszystkie korelacje $<0{,}3$, PCA nie ma sensu. **Test Bartletta sferyczności** ($H_0$: macierz korelacji jest jednostkowa) sprawdza zasadność PCA,
- normalność nie jest konieczna do opisu, ale potrzebna do wnioskowania o istotności składowych,
- liczebność: minimum 50 obserwacji (lepiej 100), co najmniej 5 obserwacji na zmienną,
- próba jednorodna i losowa; **wartości odstające mogą zafałszować wyniki** – usunąć przed analizą,
- silna współliniowość między zmiennymi ułatwia redukcję; braki: usunąć wiersze lub uzupełnić średnimi,
- wada: składowe trudniej interpretować niż pierwotne cechy, metoda tylko liniowa, wrażliwa na skalę (stąd standaryzacja).

### Walidacja składowych

Podział na zbiór uczący i testowy → PCA na uczącym → PCA na testowym → porównanie. Jeśli wyniki zgodne, składowe można uogólniać.

### Przykłady z wykładu

**Iris** (4 zmienne): wartości własne 2,918; 0,914; 0,147; 0,021 → udziały 72,96%; 22,85%; 3,67%; 0,52%. Dwie składowe wyjaśniają **95,8%** wariancji ($k=2$ na wykresie osypiska).

**Dane mieszkaniowe (Kalifornia 1990, 8 zmiennych, $n=20\,640$)**: wartości własne 3,91; 1,91; 1,07; 0,82; ... → pierwsze 3 składowe = 86,1%, pierwsze 4 = 96,4%. Interpretacja składowych: (1) rozmiar mieszkania, (2) położenie geograficzne, (3) dochód, (4) średni wiek mieszkania. Kryterium zasobu zmienności wspólnej: dla zmiennej „średni wiek mieszkania” trzy składowe dają $(-0{,}432)^2+(-0{,}023)^2+(-0{,}406)^2=0{,}35<0{,}5$ → potrzebna 4. składowa.

### Kod

```python
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

Z = StandardScaler().fit_transform(X)       # standaryzacja
pca = PCA(n_components=2)
PC = pca.fit_transform(Z)                    # rzut na 2 składowe
print(pca.explained_variance_ratio_)         # np. [0.76, 0.22]
```

## Redukcja liczności próby (numerosity reduction)

Cel: zmniejszyć liczbę obserwacji bez istotnej utraty informacji (szybsze uczenie, mniejsze koszty pamięci i obliczeń, dopasowanie do ograniczeń algorytmu, usunięcie szumu).

| Metoda | Opis |
| :--- | :--- |
| **Próbkowanie losowe proste** | losujemy podzbiór; ze zwracaniem lub bez; każda jednostka ma równe szanse |
| **Próbkowanie systematyczne** | co $k$-ty element z uporządkowanej listy |
| **Próbkowanie warstwowe (stratyfikowane)** | losowanie z każdej warstwy/klasy proporcjonalnie – zachowuje strukturę (ważne przy klasach niezrównoważonych) |
| **Próbkowanie zespołowe (grupowe)** | losujemy całe grupy (klastry) |
| **Usuwanie duplikatów** | wielokrotnie powtarzające się rekordy |
| **Usuwanie obserwacji odstających i z brakami** | tam, gdzie usunięcie nie zmienia rozkładu |
| **Reprezentanci klastrów** | grupowanie (np. k-średnich) i zastąpienie grupy centroidem lub kilkoma reprezentantami |
| **Selekcja przykładów (instance selection)** | wybór najbardziej informatywnych (np. przy granicy decyzyjnej): CNN, ENN, k-NN |
| **Under-sampling** (odrzucanie przykładów klasy większościowej) | wyrównywanie klas; przeciwieństwo: over-sampling (SMOTE) – zwiększa liczność |
| **Agregacja** | zastąpienie wielu obserwacji statystykami (średnia, suma) |
| **Dyskretyzacja/histogramy** | zastąpienie danych ciągłych częstościami w przedziałach |
| **Modele parametryczne** | zapamiętanie tylko parametrów modelu (regresja, rozkład) |

Wniosek z wykładu o próbie: **próba musi być reprezentatywna** (losowa i dostatecznie liczna), a dla PCA co najmniej 5 obserwacji na zmienną.

*Uwaga: podział na zbiór uczący/testowy (np. 80/20) i walidacja krzyżowa to także świadome „ograniczanie” próby do uczenia.*

## Podsumowanie

- Redukcja = mniej zmiennych (wymiar) i/lub mniej obserwacji (liczność), aby poprawić jakość, szybkość i czytelność.
- Przekleństwo wymiarowości: liczba potrzebnych obserwacji rośnie wykładniczo z liczbą zmiennych.
- Wymiar: **selekcja cech** (filter / wrapper / embedded) albo **konstrukcja cech** (PCA).
- **PCA**: standaryzacja → macierz kowariancji → wektory i wartości własne → wybór $k$ składowych (90–95% wariancji, Kaiser $\lambda>1$, osypisko) → projekcja. Składowe są ortogonalne, wariancja składowej = wartość własna.
- Liczność: próbkowanie (proste, systematyczne, warstwowe), selekcja przykładów, klastrowanie, usuwanie duplikatów/odstających.

---
[⬅️ Poprzedni temat](4_Modele_regresji_Regresja_wielokrotna.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_Metody_analizy_skupień.md)