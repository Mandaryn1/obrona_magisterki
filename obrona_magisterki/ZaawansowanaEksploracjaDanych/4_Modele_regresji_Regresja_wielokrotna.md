# Modele regresji. Regresja wielokrotna – postawienie zagadnienia

## Korelacja i regresja – po co

Metody **korelacyjne** wykrywają związek między zmiennymi i oceniają jego siłę, kierunek i istotność. **Analiza regresji** idzie dalej: opisuje kształt zależności i pozwala **przewidywać** wartość zmiennej zależnej $Y$ (objaśnianej) na podstawie jednej lub wielu zmiennych niezależnych $X$ (objaśniających).

- Najpierw uzasadnienie **merytoryczne** związku (korelacja ≠ przyczynowość – związek może być przypadkowy).
- Wstępna ocena: wykres rozrzutu, kowariancja, **współczynnik korelacji Pearsona** $r\in[-1,1]$.
- **Współczynnik determinacji** $R^2=r^2$ – jaka część zmienności $Y$ jest wyjaśniona przez $X$.
- Dla skal słabszych (porządkowa): korelacje rangowe **Spearmana** i **Kendalla**; dla cech nominalnych: $\chi^2$, V Cramera.

Nazwa „regresja” – F. Galton (1885), badania zależności wzrostu potomstwa od wzrostu rodziców.

### Regresja I i II rodzaju

- **funkcja regresji I rodzaju**: $E(Y\mid X=x)$ – jak zmienia się wartość oczekiwana $Y$ ze zmianą $x$ (jej postać zwykle nie jest znana),
- **regresja II rodzaju**: funkcja dopasowana do danych z próby (empiryczna linia regresji).

### Etapy budowy modelu

1. **specyfikacja** modelu (dobór postaci funkcji i zmiennych),
2. **estymacja** parametrów,
3. **weryfikacja** (istotność, dopasowanie, założenia, reszty),
4. **użycie** do prognozowania.

## Regresja liniowa prosta

$$Y=\beta_0+\beta_1 X+\varepsilon,\qquad \hat{y}=b_0+b_1x$$

Parametry szacuje **metoda najmniejszych kwadratów (MNK)** – minimalizacja sumy kwadratów odległości punktów od prostej:

$$\min\sum_{i=1}^{n}(y_i-b_0-b_1x_i)^2\ \Rightarrow\ b_1=\frac{\sum(x_i-\bar{x})(y_i-\bar{y})}{\sum(x_i-\bar{x})^2},\quad b_0=\bar{y}-b_1\bar{x}$$

## Regresja wielokrotna (wieloraka) – postawienie zagadnienia

### Model

Zmienna zależna $Y$ jest **liniową funkcją $k$ zmiennych objaśniających** i składnika losowego:

$$Y=\beta_0+\beta_1X_1+\beta_2X_2+\dots+\beta_kX_k+\varepsilon$$

- $\beta_j$ – parametry modelu (nieznane; szacujemy je z próby),
- $\varepsilon$ – składnik losowy (błąd).

**Interpretacja współczynnika $b_j$**: oszacowana zmiana wartości $Y$ przy wzroście $X_j$ o jednostkę, **przy założeniu, że pozostałe zmienne są stałe**.

### Zapis macierzowy

Dla próby $n$-elementowej: $\ \mathbf{y}=\mathbf{X}\boldsymbol{\beta}+\boldsymbol{\varepsilon}$, gdzie

- $\mathbf{y}$ – wektor $n\times1$ obserwacji $Y$,
- $\mathbf{X}$ – macierz $n\times(k+1)$ obserwacji zmiennych objaśniających (ostatnia/pierwsza kolumna jedynek dla wyrazu wolnego),
- $\boldsymbol{\beta}$ – wektor $(k+1)\times1$ współczynników regresji,
- $\boldsymbol{\varepsilon}$ – wektor błędów losowych.

Estymator MNK (nieobciążony):

$$\mathbf{b}=(\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T\mathbf{y}$$

Macierz wariancji estymatora i estymator wariancji składnika losowego:

$$\operatorname{Var}(\mathbf{b})=s^2(\mathbf{X}^T\mathbf{X})^{-1},\qquad s^2=\frac{SSE}{n-k-1}$$

### Założenia klasycznego modelu

- składnik losowy ma średnią 0 i **stałą wariancję** $\sigma^2$ (homoskedastyczność),
- błędy $\varepsilon_i$ są **niezależne** (brak autokorelacji),
- błędy mają **rozkład normalny** (potrzebne do testów i przedziałów ufności),
- zależność jest **liniowa** względem parametrów,
- zmienne objaśniające **nie są (silnie) współliniowe** (macierz $\mathbf{X}^T\mathbf{X}$ odwracalna). Zmienna objaśniana skorelowana z objaśniającymi, a objaśniające między sobą nie.

## Rozkład zmienności i ocena dopasowania

Reszty $e_i=y_i-\hat{y}_i$ – pionowa odległość punktu od płaszczyzny/hiperpłaszczyzny regresji.

| Wielkość | Wzór | Znaczenie |
| :--- | :--- | :--- |
| $SST$ | $\sum(y_i-\bar{y})^2$ | zmienność całkowita |
| $SSR$ | $\sum(\hat{y}_i-\bar{y})^2$ | zmienność **wyjaśniona** regresją |
| $SSE$ | $\sum(y_i-\hat{y}_i)^2$ | zmienność **niewyjaśniona** (błąd) |

$$SST=SSR+SSE$$

**Współczynnik determinacji wielokrotnej**:

$$R^2=\frac{SSR}{SST}=1-\frac{SSE}{SST}$$

- część zmienności $Y$ wyjaśniona przez związek liniowy ze zbiorem zmiennych objaśniających,
- **dodanie zmiennej zawsze zwiększa $R^2$**, nawet bezużytecznej → **skorygowany**:

$$R^2_{adj}=1-(1-R^2)\frac{n-1}{n-k-1}$$

(kara za nadmiar zmiennych; jest miarą do porównywania modeli o różnej liczbie zmiennych).

### Tablica ANOVA dla regresji

| Źródło | $SS$ | $df$ | $MS$ | $F$ |
| :--- | :--- | :-: | :--- | :--- |
| Regresja | $SSR$ | $k$ | $MSR=SSR/k$ | $MSR/MSE$ |
| Błąd (reszta) | $SSE$ | $n-k-1$ | $MSE=SSE/(n-k-1)$ | |
| Całkowita | $SST$ | $n-1$ | | |

## Wnioskowanie w modelu regresji wielokrotnej

1. **Test istotności całej regresji** (test $F$): $H_0:\ \beta_1=\dots=\beta_k=0$ (żadna zmienna nie wyjaśnia $Y$) vs $H_1$: przynajmniej jeden $\beta_j\ne0$. $MSE$ jest dobrym estymatorem wariancji zawsze, $MSR$ tylko gdy $H_0$ prawdziwa; jeśli $H_0$ fałszywa, $MSR$ jest duże → duże $F$ → odrzucenie.
2. **Test $t$ dla pojedynczego współczynnika**: $H_0:\ \beta_j=0$ przy uwzględnieniu pozostałych zmiennych; $t=\frac{b_j}{s_{b_j}}$, $df=n-k-1$. Odrzucenie $H_0$ potwierdza zależność liniową z $X_j$.
3. **Przedział ufności** dla współczynnika $\beta_j$: $b_j\pm t\cdot s_{b_j}$.
4. **Przedział ufności dla wartości średniej** $Y$ dla zadanych $x$.
5. **Przedział predykcji** dla pojedynczej wartości $Y$ (szerszy niż przedział dla średniej).

## Zmienne jakościowe – zmienne sztuczne (wskaźnikowe)

Zmienną jakościową o $m$ kategoriach zamieniamy na **$m-1$ zmiennych 0/1**. Wartość 1 oznacza przynależność do danej kategorii. Kategoria bez własnej zmiennej to **kategoria odniesienia** (wszystkie zmienne sztuczne = 0). Współczynnik przy zmiennej sztucznej to różnica względem kategorii odniesienia.

*Przykład z zajęć (płatki śniadaniowe): `rating` ~ `sugars` + `fiber` + zmienna wskaźnikowa `shelf` (półka).*

## Wybór zmiennych objaśniających

| Metoda | Opis |
| :--- | :--- |
| Częściowy test $F$ | porównanie modelu pełnego i okrojonego |
| **Dołączanie** (forward) | dodajemy po jednej najlepszej zmiennej |
| **Eliminacja** (backward) | startujemy z pełnym modelem, usuwamy najmniej istotne |
| **Regresja krokowa** (stepwise) | łączy dołączanie i eliminację |
| Najlepsze podzbiory | dla każdej liczby zmiennych szukamy najlepszego zestawu |
| Wszystkie możliwe regresje | pełny przegląd (kosztowne) |

W ML: regularyzacja **Lasso (L1)** – wbudowana selekcja, **Ridge (L2)** – zmniejsza wpływ współliniowości.

## Alternatywa dla MNK: spadek gradientowy

Zamiast rozwiązania analitycznego $(\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T\mathbf{y}$ – metoda **iteracyjna** (optymalizacja numeryczna). Idziemy w kierunku **antygradientu** funkcji celu $J(\boldsymbol\beta)=\frac{1}{2n}\sum(y_i-\hat y_i)^2$:

$$\boldsymbol{\beta}^{(t+1)}=\boldsymbol{\beta}^{(t)}-\alpha\,\nabla J\big(\boldsymbol{\beta}^{(t)}\big)$$

- $\alpha$ – krok (współczynnik uczenia): za duży → rozbieżność, za mały → wolna zbieżność (często malejący w czasie),
- stop: mała zmiana parametrów/gradientu lub maksymalna liczba iteracji,
- metoda zachłanna – ogólnie może utknąć w minimum lokalnym (dla regresji liniowej funkcja celu jest wypukła, więc minimum jest globalne),
- **stochastyczny gradient (SGD)**: gradient z jednej obserwacji/małej paczki – szybki, skalowalny, nie wymaga wczytania całego zbioru do pamięci, prosty; najpopularniejszy w ML; wada: wolniejsza/niestabilna zbieżność, dobór kroku.

## Inne modele regresji (z wykładu)

### Szeregi czasowe – trend i sezonowość

Dane zależne od czasu. Dwie podstawowe właściwości: **trend** (wzrost/spadek/stałość – model regresji liniowej lub wykładniczej względem czasu) i **sezonowość** (regularne wzorce powtarzające się, np. wzrost sprzedaży w grudniu; aby ją wykryć, zwiększamy granulację danych). Prosty model: $\hat{y}_t=a+bt+s_m$, gdzie $s_m$ to średnia różnica rzeczywistości od trendu dla danego miesiąca $m$. Prognoza = trend + poprawka sezonowa.

### Regresja logistyczna (klasyfikacja binarna)

Zmienna zależna dychotomiczna ($Y\in\{0,1\}$, np. choroba tak/nie). MNK nie pasuje (brak stałości wariancji, przewidywania poza $[0,1]$), więc stosuje się funkcję **logistyczną** (sigmoidalną) i **metodę największej wiarygodności (MLE)**:

$$P(Y=1\mid\mathbf{x})=\frac{1}{1+e^{-(\beta_0+\beta_1x_1+\dots+\beta_kx_k)}},\qquad \ln\frac{p}{1-p}=\beta_0+\beta_1x_1+\dots$$

- wynik interpretujemy jako **prawdopodobieństwo**; klasa wg progu odcięcia (zwykle 0,5),
- funkcja kosztu: logarytmiczna (log loss), nie błąd kwadratowy,
- istotność: test **ilorazu wiarygodności** ($G\sim\chi^2$) i test **Walda**,
- ocena: dokładność, precyzja, czułość, F1 (zob. temat 9).

*Przykład z wykładu (wiek → choroba, $n=20$): $\hat\beta_0\approx-4{,}37$, $\hat\beta_1\approx0{,}067$; dla 50-latka $P\approx26\%$, dla 72-latka $P\approx61\%$; $G\approx5{,}7>3{,}84$ → wiek jest przydatny w przewidywaniu choroby.*

## Przykład: obliczenia w Pythonie

```python
import statsmodels.api as sm

X = sm.add_constant(df[['sugars', 'fiber']])   # dodanie wyrazu wolnego
model = sm.OLS(df['rating'], X).fit()
print(model.summary())      # współczynniki, t, p, R^2, R^2_adj, F, tablica
```

## Podsumowanie

- Regresja wielokrotna: $Y=\beta_0+\sum\beta_jX_j+\varepsilon$; estymacja MNK: $\mathbf{b}=(\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T\mathbf{y}$.
- Założenia: średnia błędów 0, stała wariancja, niezależność, normalność, brak współliniowości.
- Ocena: $R^2$ i $R^2_{adj}$, test $F$ (całość), testy $t$ (współczynniki), analiza reszt.
- Zmienne jakościowe → zmienne sztuczne ($m-1$); wybór zmiennych: forward, backward, stepwise, najlepsze podzbiory, Lasso.
- Alternatywa dla MNK: spadek gradientowy (SGD).
- Pokrewne: szeregi czasowe (trend + sezonowość), regresja logistyczna (klasyfikacja, MLE).
