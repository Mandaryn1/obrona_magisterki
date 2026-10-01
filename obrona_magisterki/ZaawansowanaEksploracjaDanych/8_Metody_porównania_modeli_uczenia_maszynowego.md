# Metody porównania modeli uczenia maszynowego

## Problem

Dwa (lub więcej) modele osiągnęły różne wyniki (np. dokładność 91% i 89%). Czy różnica jest **prawdziwa**, czy wynika z **przypadku** (losowy podział danych, szum)? Odpowiedź dają **statystyczne testy istotności**, a nie samo porównanie średnich.

Plan porównania:

1. wybór **miary jakości** (accuracy, F1, AUC, RMSE, ... – zob. tematy 9 i 10) oraz uwzględnienie kosztów, czasu, interpretowalności,
2. **protokół oceny** (hold-out 80/20, **k-krotna walidacja krzyżowa**, powtarzana walidacja) – na tych samych podziałach dla wszystkich modeli,
3. **test statystyczny** dobrany do liczby modeli i liczby zbiorów danych.

## Dobór testu

| Sytuacja | Test |
| :--- | :--- |
| 2 klasyfikatory, **1 zbiór**, jeden zbiór testowy (zgodność odpowiedzi) | **McNemara** |
| 2 klasyfikatory, **1 zbiór**, wyniki z k-fold CV | sparowany test $t$ ze **skorygowaną wariancją (Nadeau-Bengio)**; test 5×2cv |
| 2 klasyfikatory, **wiele zbiorów** | **Wilcoxona** (rangowanych znaków), test znaków |
| $>2$ klasyfikatorów, **wiele zbiorów** | **Friedmana** (+ Iman-Davenport) i post-hoc **Nemenyiego** |

## Test McNemara (2 klasyfikatory, jeden zbiór testowy)

Nieparametryczny test dla **dwóch prób zależnych** i zmiennej dychotomicznej. Oba klasyfikatory oceniamy na **tych samych** obiektach testowych i zapisujemy, czy dany obiekt został sklasyfikowany poprawnie.

Tabela kontyngencji $2\times2$:

| | Model B poprawny | Model B błędny |
| :--- | :-: | :-: |
| **Model A poprawny** | $n_{11}$ | $n_{10}$ |
| **Model A błędny** | $n_{01}$ | $n_{00}$ |

- $H_0:\ n_{10}=n_{01}$ (modele popełniają błędy w podobny sposób, nie różnią się istotnie),
- $H_1:\ n_{10}\ne n_{01}$.

Liczą się tylko **przypadki niezgodne** ($n_{10}$, $n_{01}$):

$$\chi^2=\frac{(n_{01}-n_{10})^2}{n_{01}+n_{10}},\qquad \text{z poprawką na ciągłość: }\ \chi^2=\frac{(\lvert n_{01}-n_{10}\rvert-1)^2}{n_{01}+n_{10}},\quad df=1$$

$p\le\alpha$ → odrzucamy $H_0$; $\chi^2_{0{,}05}(1)=3{,}841$.

**Przykład z wykładu**: dwa klasyfikatory (LDC – liniowa i QDC – kwadratowa analiza dyskryminacyjna) rozróżniają dwie odmiany irysów: $n_{11}=31$, $n_{10}=0$, $n_{01}=6$, $n_{00}=13$.
$\chi^2=\frac{(\lvert6-0\rvert-1)^2}{6}=\frac{25}{6}=4{,}17>3{,}841$ → odrzucamy $H_0$: klasyfikatory **różnią się istotnie**.

Zalety: prosty, wymaga tylko jednego uruchomienia każdego modelu. Wady: nie uwzględnia zmienności wynikającej z doboru zbioru uczącego, wynik dotyczy jednego podziału.

## Dwa klasyfikatory, jeden zbiór – wyniki z walidacji krzyżowej

Dla każdego z $n$ foldów mamy błędy $x_{Ai}$, $x_{Bi}$. Test dla różnic par: $d_i=x_{Ai}-x_{Bi}$,

$$\bar{d}=\frac{1}{n}\sum d_i,\quad s^2=\frac{1}{n-1}\sum(d_i-\bar{d})^2,\quad t=\frac{\bar{d}}{s/\sqrt{n}},\ \ df=n-1$$

### Problem: naruszenie niezależności

W k-fold CV zbiory treningowe w różnych iteracjach **nakładają się**, więc wyniki **nie są niezależne**. Zwykły sparowany test $t$ **zaniża wariancję** różnic, przez co błąd I rodzaju przekracza poziom $\alpha$ – można „znaleźć” istotną różnicę, której nie ma. **Zwykły sparowany test $t$ nie jest poprawny do porównywania modeli ML.**

### Poprawka Nadeau i Bengio (2003)

Skorygowana wariancja:

$$\hat{s}^2=\left(\frac{1}{n}+\frac{n_1}{n_2}\right)s^2$$

($n$ – liczba foldów/powtórzeń, $n_1$ – liczność zbioru testowego, $n_2$ – treningowego), statystyka $t=\dfrac{\bar{d}}{\hat{s}}$, $df=n-1$.

**Przykład z wykładu** (10-fold, błędy klasyfikacji A i B): $\bar{d}=3{,}73$, $s=4{,}509$; zwykły sparowany test $t$: $t=2{,}62$, $p=0{,}028$ („istotne”). Po korekcie ($n_1/n_2=1/9$): $t=\dfrac{3{,}73}{4{,}509\sqrt{1/10+1/9}}\approx1{,}80$, $p\approx0{,}105$ → **brak podstaw do odrzucenia $H_0$** – po korekcie różnicy nie można uznać za istotną.

```python
import numpy as np
from scipy.stats import t

d = np.array(A_score) - np.array(B_score)         # różnice wyników w foldach
d_bar, s2 = d.mean(), d.var(ddof=1)
n = len(d)                                        # liczba foldów
n1, n2 = len(y_test), len(y_train)                # rozmiary zbiorów w foldzie
s2_mod = s2 * (1/n + n1/n2)
t_stat = d_bar / np.sqrt(s2_mod)
p_value = 2 * (1 - t.cdf(abs(t_stat), n - 1))
```

## Wiele zbiorów danych (Demšar, 2006)

Oszacowanie wariancji jest tu trudne, a założenie normalności różnic wątpliwe (różne zbiory – różne skale błędów) → zalecane **testy nieparametryczne**:

### Dwa klasyfikatory

1. **test rangowanych znaków Wilcoxona** (rangi różnic wyników na kolejnych zbiorach),
2. **test znaków** (liczba zbiorów, na których A jest lepszy od B; mniej mocny).

### Więcej niż dwa klasyfikatory ($m$ modeli, $n$ zbiorów)

**Test Friedmana**: na każdym zbiorze szereguje się modele (rangi 1…$m$; 1 = najlepszy; wiązania – rangi średnie), liczy średnią rangę każdego modelu $R_j=\frac{1}{n}\sum_i r_i^j$.

$$\chi_F^2=\frac{12n}{m(m+1)}\left(\sum_{j=1}^{m}R_j^2-\frac{m(m+1)^2}{4}\right),\quad df=m-1$$

**Modyfikacja Iman & Davenport** (zwykle lepsza moc):

$$F_F=\frac{(n-1)\chi_F^2}{n(m-1)-\chi_F^2}\ \sim\ F\big(m-1,\ (m-1)(n-1)\big)$$

$H_0$: wszystkie modele są równoważne (średnie rangi równe). Jeśli odrzucona → **test post-hoc Nemenyiego**: dla pary modeli $i,j$

$$z=\frac{R_i-R_j}{\sqrt{m(m+1)/(6n)}}$$

(równoważnie: różnica średnich rang większa niż **krytyczna różnica** $CD=q_\alpha\sqrt{\frac{m(m+1)}{6n}}$ oznacza istotną różnicę; wynik często prezentuje się na diagramie krytycznych różnic).

**Przykład z wykładu**: 11 modeli × 20 zbiorów danych: $F_F=0{,}681$, $p=0{,}74>0{,}05$ → brak podstaw do odrzucenia $H_0$; modeli nie można uznać za istotnie różne, więc post-hoc nie jest potrzebny.

## Inne sposoby porównywania modeli

- **Metryki jakości na tym samym zbiorze testowym**: accuracy, precision/recall, F1, AUC (krzywe ROC; test DeLonga dla AUC), RMSE/MAE – zob. tematy 9 i 10.
- **Walidacja krzyżowa** (k=10, stratyfikowana, powtarzana) – bardziej wiarygodna niż pojedynczy podział.
- **Rozkład wyników w foldach** (średnia ± odchylenie std., wykresy pudełkowe).
- **Kryteria informacyjne** (AIC, BIC) i testy ilorazu wiarygodności dla modeli statystycznych (regresja).
- **Złożoność i koszt**: czas uczenia i predykcji, pamięć, skalowalność.
- **Interpretowalność** (drzewa, regresja vs sieci neuronowe).
- **Odporność** (na szum, outliery), stabilność.
- Zasada **brzytwy Ockhama**: przy zbliżonej jakości wybieramy prostszy model.
- Dobre praktyki: te same dane i podziały dla wszystkich modeli, strojenie hiperparametrów (np. `RandomizedSearchCV`) tylko na zbiorze uczącym/walidacyjnym, **ostateczna ocena na niezależnym zbiorze testowym**, unikanie wycieku danych.

## Podsumowanie

- Samo „A ma wyższą dokładność” nie wystarcza – trzeba sprawdzić **istotność** różnicy.
- 2 modele, 1 zbiór: **McNemar** (zgodność odpowiedzi) lub sparowany $t$ **z korektą Nadeau-Bengio** (zwykły test $t$ jest niepoprawny przez zależność foldów).
- 2 modele, wiele zbiorów: **Wilcoxon**, test znaków.
- $>2$ modeli, wiele zbiorów: **Friedman** (+ Iman-Davenport), post-hoc **Nemenyi**.
- Poza testami: porównanie miar jakości w walidacji krzyżowej, kosztu, interpretowalności i złożoności.
