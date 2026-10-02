# Wnioskowanie statystyczne. ANOVA, MANOVA – postawienie zagadnienia, przykłady zastosowań

## Wnioskowanie statystyczne – podstawy

**Statystyka matematyczna** zajmuje się wnioskowaniem o całej zbiorowości (populacji) na podstawie zbadania jej części – **próby**. To wnioskowanie indukcyjne ma dwa działy:

| Dział | Cel | Kierunek |
| :--- | :--- | :--- |
| **Estymacja** | szacowanie wartości parametrów lub postaci rozkładu w populacji | z próby → wniosek o populacji |
| **Weryfikacja (testowanie) hipotez** | sprawdzanie przypuszczeń o parametrach lub rozkładach populacji | najpierw hipoteza o populacji → sprawdzenie na próbie |

**Próba jest reprezentatywna**, gdy (1) elementy są pobierane losowo, (2) jest dostatecznie liczna (rozkład empiryczny mało różni się od teoretycznego).

### Centralne twierdzenie graniczne (CTG)

Suma (średnia) niezależnych zmiennych losowych o tym samym rozkładzie, ze skończoną wariancją $\sigma^2$, ma **asymptotycznie rozkład normalny**:

$$\frac{S_n - E(S_n)}{\sigma\sqrt{n}} \to N(0,1), \qquad \bar{X} \approx N\!\left(\mu, \frac{\sigma}{\sqrt{n}}\right)$$

Praktycznie: dla $n \ge 30$ rozkład średnich z próby jest bliski normalnemu, **niezależnie od kształtu rozkładu cechy**. Dlatego testy dla średnich działają także przy dużych próbach bez normalności.

## Estymacja

**Estymator** – statystyka (funkcja próby) przybliżająca parametr populacji (np. $\bar{x}$ szacuje $\mu$, $s^2$ szacuje $\sigma^2$).

Własności dobrego estymatora:

1. **nieobciążoność** – $E(\hat\theta)=\theta$ (brak systematycznego zawyżania/zaniżania),
2. **zgodność** – $\hat\theta \to \theta$ ze wzrostem $n$,
3. **efektywność** – najmniejsza wariancja,
4. **dostateczność** – wykorzystuje całą informację z próby (średnia tak, mediana nie).

Wariancja z próby z dzielnikiem $n-1$ jest nieobciążona: $s^2 = \frac{1}{n-1}\sum(x_i-\bar{x})^2$ (z dzielnikiem $n$ jest obciążona).

| Rodzaj | Co daje |
| :--- | :--- |
| **punktowa** | jedna liczba (np. $\bar{x}=244{,}2$ g) |
| **przedziałowa** | przedział ufności, który z prawdopodobieństwem $1-\alpha$ (zwykle 0,90; 0,95; 0,99) obejmuje nieznany parametr |

Przedział ufności dla średniej (małe $n$, rozkład normalny): $\;\bar{x} \pm t_{1-\alpha/2,\,n-1}\dfrac{s}{\sqrt{n}}$.

## Testowanie hipotez

### Pojęcia

- **$H_0$ (hipoteza zerowa)** – weryfikowana, zwykle chcemy ją odrzucić (np. „średnie są równe”),
- **$H_1$ (alternatywna)** – przyjmowana po odrzuceniu $H_0$,
- **błąd I rodzaju** ($\alpha$) – odrzucenie prawdziwej $H_0$; **$\alpha$ = poziom istotności** (zwykle 0,05; też 0,1 i 0,01),
- **błąd II rodzaju** ($\beta$) – przyjęcie fałszywej $H_0$; **moc testu** $=1-\beta$,
- **obszar krytyczny** – wartości statystyki testowej, przy których odrzucamy $H_0$,
- **$p$-wartość** – najmniejszy poziom istotności, przy którym obserwowana wartość statystyki prowadzi do odrzucenia $H_0$.

Testy istotności **kontrolują błąd I rodzaju**. Nie „przyjmujemy” $H_0$ – decydujemy o jej **odrzuceniu** lub **braku podstaw do odrzucenia**.

**Reguła decyzyjna**:

- $p \le \alpha$ → odrzucamy $H_0$ na korzyść $H_1$,
- $p > \alpha$ → brak podstaw do odrzucenia $H_0$.

Dla $\alpha=0{,}05$: $p<0{,}01$ – odrzucenie jednoznaczne; $p>0{,}1$ – brak podstaw jednoznaczny.

### Etapy weryfikacji

1. zebranie danych,
2. sformułowanie $H_0$ i $H_1$,
3. wybór poziomu istotności $\alpha$,
4. dobór testu (zależy od skali, rozkładu, liczby i zależności grup),
5. obliczenie statystyki testowej,
6. wyznaczenie obszaru krytycznego (z tablic) lub $p$-wartości,
7. decyzja.

## Testy parametryczne

### Test $t$ dla jednej średniej

Założenie: cecha ma rozkład normalny. $H_0:\ \mu=\mu_0$.

$$t = \frac{\bar{x}-\mu_0}{s/\sqrt{n}},\quad df=n-1 \quad (\text{dla dużej próby: } N(0,1))$$

**Przykład (czekolada)**: 10 tabliczek, $\bar{x}=244{,}2$ g, $s=6{,}14$, $H_0:\ \mu=250$ g. $t=\frac{244{,}2-250}{6{,}14/\sqrt{10}} = -2{,}99$; $t_{krytyczne}(9;\,0{,}05)=2{,}26$ → **odrzucamy $H_0$** ($p\approx0{,}015$). 95% przedział ufności dla średniej: ok. (239,8; 248,6) g – nie zawiera 250.

### Test $t$ dla dwóch średnich

- **próby niezależne** (np. chorzy vs zdrowi): założenia – normalność, **jednorodność wariancji**; $H_0:\ \mu_1=\mu_2$,
- **próby zależne** (przed/po na tych samych obiektach): test dla różnic par; $H_0:\ \mu_d = 0$; założenie – normalność **różnic**.

## Testy nieparametryczne (gdy brak normalności lub dane porządkowe)

| Test | Zastępuje | Czego dotyczy |
| :--- | :--- | :--- |
| **U Manna-Whitneya** | $t$ dla 2 prób niezależnych | porównanie rozkładów/median dwóch niezależnych grup |
| **Wilcoxona** (rangowanych znaków) | $t$ dla prób zależnych | porównanie dwóch pomiarów zależnych |
| **Kruskala-Wallisa** | jednoczynnikowa ANOVA | $k>2$ grup niezależnych |
| **Friedmana** | ANOVA z powtarzanymi pomiarami | $k \ge 2$ pomiarów zależnych |
| **$\chi^2$ niezależności** | – | związek dwóch cech nominalnych |

### Test U Manna-Whitneya

Najmocniejszy test nieparametryczny dla dwóch prób niezależnych (przy spełnionych założeniach testu $t$ jego moc to ok. 95% mocy testu $t$).

1. połącz obie próby, uporządkuj rosnąco i nadaj **rangi** (wartościom równym – **rangi wiązane**, czyli średnia z rang),
2. policz sumy rang $R_1, R_2$,
3. $U_1 = n_1 n_2 + \frac{n_1(n_1+1)}{2} - R_1$, $\;U_2 = n_1 n_2 - U_1$, statystyka $U=\min(U_1,U_2)$,
4. $U \le U_{kryt}$ → odrzucamy $H_0$. Dla $n>20$ stosuje się przybliżenie normalne $Z$ (obszar krytyczny lewostronny).

### Test Wilcoxona (prób zależnych)

Różnice par → rangi wartości bezwzględnych różnic → sumy rang różnic dodatnich $T^+$ i ujemnych $T^-$ → $T=\min(T^+,T^-)$; $T\le T_{kryt}$ → odrzucamy $H_0$.

### Test niezależności $\chi^2$ Pearsona

Dla dwóch cech nominalnych w tablicy kontyngencji ($w$ wierszy, $k$ kolumn). $H_0$: cechy są niezależne.

$$\chi^2=\sum_{i=1}^{w}\sum_{j=1}^{k}\frac{(n_{ij}-\tilde{n}_{ij})^2}{\tilde{n}_{ij}},\qquad \tilde{n}_{ij}=\frac{n_{i\cdot}\,n_{\cdot j}}{n},\qquad df=(w-1)(k-1)$$

$n_{ij}$ – liczebności obserwowane, $\tilde{n}_{ij}$ – oczekiwane przy niezależności. Przykład: czy palenie zależy od wykształcenia. Miary siły związku: V Cramera, C Pearsona, Czuprowa (zakres 0–1), Yule'a dla tablic 2×2.

### Wybór testu – schemat

1. Skala nominalna → $\chi^2$.
2. Skala ilościowa, normalność? → tak: testy $t$/ANOVA; nie: testy nieparametryczne.
3. Liczba grup: 2 czy więcej? Grupy niezależne czy zależne?

## Testy założeń ANOVA

### Testy normalności

Hipoteza $H_0$: rozkład badanej cechy jest normalny. Wybór zależy od liczebności próby:

- **Shapiro-Wilka** – zalecany dla małych/średnich prób; $H_0$ odrzucamy, gdy $W$ jest mniejsze od wartości krytycznej (małe $W$ = odstępstwo od normalności),
- **Kołmogorowa-Smirnowa** (i zmodyfikowany, Lilleforsa) – porównanie dystrybuanty empirycznej z teoretyczną: $D=\sup|F_n(x)-F(x)|$,
- **$\chi^2$ Pearsona** – dla dużych prób; porównanie liczebności w klasach z oczekiwanymi.

### Testy jednorodności wariancji

$H_0:\ \sigma_1^2=\dots=\sigma_k^2$ (przy założeniu normalności):

- **Bartletta** (dowolne liczebności), **Hartleya** ($F_{max}=s^2_{max}/s^2_{min}$, próby równoliczne), **Caldwella** (oparty na rozstępach, równoliczne),
- w praktyce często **Levene'a** (odporniejszy na brak normalności; `scipy.stats.levene`).

## ANOVA – analiza wariancji

### Postawienie zagadnienia

Eksperyment: jak jeden (lub więcej) **czynnik** wpływa na wartości badanej cechy ilościowej? Rozwinięta przez **R. A. Fishera** (doświadczalnictwo rolnicze).

ANOVA weryfikuje **jednoczesną równość więcej niż dwóch średnich**:

$$H_0:\ \mu_1=\mu_2=\dots=\mu_k \qquad H_1:\ \text{co najmniej dwie średnie różnią się}$$

**Dlaczego nie wiele testów $t$?** Przy wielu porównaniach tych samych danych rośnie prawdopodobieństwo, że któreś wyjdzie „istotne” przypadkiem (inflacja błędu I rodzaju; częściowe rozwiązanie: poprawka Bonferroniego). ANOVA ma ponadto większą moc.

**Założenia**: zmienne mierzalne; populacje niezależne; rozkład normalny w każdej grupie; **jednakowe wariancje**.

### Jednoczynnikowa ANOVA

Model: $x_{ij} = \mu + \alpha_i + \varepsilon_{ij}$, gdzie $\mu$ – średnia ogólna, $\alpha_i$ – efekt $i$-tej grupy (czynnika), $\varepsilon_{ij}\sim N(0,\sigma^2)$ – składnik losowy.

**Rozkład zmienności** na dwa addytywne składniki:

$$SS_{total} = SS_{between} + SS_{within}$$

$$SS_{between}=\sum_{i=1}^{k} n_i(\bar{x}_i-\bar{x})^2,\qquad SS_{within}=\sum_{i=1}^{k}\sum_{j=1}^{n_i}(x_{ij}-\bar{x}_i)^2$$

Średnie kwadraty (sumy kwadratów podzielone przez stopnie swobody) i statystyka:

$$MS_{between}=\frac{SS_{between}}{k-1},\quad MS_{within}=\frac{SS_{within}}{N-k},\quad F=\frac{MS_{between}}{MS_{within}}\sim F(k-1,\,N-k)$$

| Źródło zmienności | $SS$ | $df$ | $MS$ | $F$ |
| :--- | :--- | :--- | :--- | :--- |
| Między grupami (efekt) | $SS_{between}$ | $k-1$ | $SS_{b}/(k-1)$ | $MS_b/MS_w$ |
| Wewnątrz grup (błąd) | $SS_{within}$ | $N-k$ | $SS_w/(N-k)$ | |
| Razem | $SS_{total}$ | $N-1$ | | |

Jeśli $F > F_{kryt}$ → odrzucamy $H_0$ (czynnik istotnie wpływa na wynik).

#### Przykład (koszty materiałowe, 3 metody produkcji)

A: `25,15,20,30,10` ($\bar{x}=20$), B: `40,20,25,50,10,35` ($\bar{x}=30$), C: `5,15,20,20,40,10,30` ($\bar{x}=20$); $N=18$, $\bar{x}=23{,}3$.

- $SS_{between}=400$, $df=2$ → $MS_b = 200$,
- $SS_{within}=250+1050+850=2150$, $df=15$ → $MS_w=143{,}3$,
- $F=200/143{,}3=1{,}39$; $F_{kryt}(2;15;0{,}05)=3{,}68$ → **brak podstaw do odrzucenia $H_0$** (metody nie różnią się istotnie kosztem).

#### Testy post-hoc (porównań wielokrotnych)

Istotne $F$ mówi tylko, że **jakieś** średnie się różnią, nie które. Aby wskazać pary, stosuje się testy post-hoc: **Tukeya (HSD)**, Scheffégo, Duncana, Newmana-Keulsa, Bonferroniego, NIR (najmniejszej istotnej różnicy).

Przykład z Pythona: `f_oneway` dała $F=85{,}4$, $p=4{,}1\cdot10^{-13}$ (odrzucamy $H_0$), a Tukey HSD (`statsmodels`) pokazał istotne różnice wszystkich trzech par lat.

### ANOVA dla powtarzanych pomiarów (grupy zależne)

Ci sami badani mierzeni $k$ razy (różne warunki/czas). Założenie: normalność **różnic** par pomiarów i podobne wariancje różnic (sferyczność). Zmienność całkowita dzieli się na:

$$SS_{total}=SS_{conditions}+SS_{subjects}+SS_{error}$$

Wydzielenie zmienności między osobami zmniejsza błąd i zwiększa moc. $F=MS_{conditions}/MS_{error}$ z $df=(k-1,\ (k-1)(n-1))$.

**Przykład**: 8 pacjentów, pomiar na początku / w trakcie / na końcu terapii. $SS_{cond}=6{,}08$, $SS_{subj}=48{,}5$, $SS_{err}=25{,}25$; $F=\frac{3{,}04}{1{,}80}=1{,}69 < F_{kryt}(2;14)=3{,}74$ → terapia nie zmieniła istotnie parametru.

### Nieparametryczne alternatywy ANOVA

**Kruskala-Wallisa** (1 czynnik, grupy niezależne): rangi dla wszystkich obserwacji łącznie (rangi wiązane dla równych wartości):

$$H=\frac{12}{N(N+1)}\sum_{i=1}^{k}\frac{R_i^2}{n_i}-3(N+1)\ \sim\ \chi^2(k-1)$$

(z poprawką na rangi wiązane: dzielimy przez $C=1-\frac{\sum(t^3-t)}{N^3-N}$). Dla przykładu kosztów: sumy rang 42; 72,5; 56,5, $H\approx2{,}16$ (z poprawką), $\chi^2_{kryt}(2)=5{,}99$ → brak podstaw do odrzucenia $H_0$.

**Friedmana** (powtarzane pomiary): rangi **w obrębie każdego wiersza** (osoby):

$$\chi_F^2=\frac{12}{nk(k+1)}\sum_{j=1}^{k}R_j^2-3n(k+1)\ \sim\ \chi^2(k-1)$$

Przykład (7 pacjentów, 3 pomiary, sumy rang 11; 17; 14): $\chi_F^2=2{,}57 < 5{,}99$ → brak podstaw. Post-hoc: test **Dunna**.

### Dwuczynnikowa ANOVA (klasyfikacja podwójna)

Na cechę wpływają **dwa czynniki** A ($r$ poziomów) i B ($k$ poziomów):

$$SS_{total}=SS_A+SS_B+SS_{error}\quad(\text{bez powtórzeń})$$

Testujemy osobno wpływ A ($F_A=MS_A/MS_E$, $df=(r-1,\ (r-1)(k-1))$) i B ($F_B=MS_B/MS_E$).

**Z powtórzeniami** (co najmniej 2 obserwacje w każdej komórce, równoliczne): dodatkowo wydziela się **interakcję** AB:

$$SS_{total}=SS_A+SS_B+SS_{AB}+SS_{error}$$

**Interakcja** = wpływ jednego czynnika zależy od poziomu drugiego. Przy istotnej interakcji efektów głównych nie interpretuje się osobno. Po testach – post-hoc.

**Przykład bez powtórzeń**: koszty materiałowe dla 4 zakładów (A) × 3 metod produkcji (B):

| | $SS$ | $df$ | $MS$ | $F$ | $F_{kryt}$ |
| :--- | :-: | :-: | :-: | :-: | :-: |
| Zakład (A) | 177 | 3 | 59,0 | 2,30 | 4,76 |
| Metoda (B) | 620,7 | 2 | 310,3 | 12,09 | 5,14 |
| Reszta | 154 | 6 | 25,7 | | |

Wniosek: **zakład nie ma istotnego wpływu, metoda produkcji ma**.

**Przykład z interakcją (SPSS)**: wpływ płci i wykształcenia na zainteresowanie polityką: płeć nieistotna ($p=0{,}448$), wykształcenie istotne ($p<0{,}001$), **interakcja istotna** ($F(2,52)=7{,}3$; $p=0{,}002$) – mężczyźni bardziej zainteresowani tylko na poziomie uniwersyteckim.

## MANOVA – wielowymiarowa analiza wariancji

### Postawienie zagadnienia

Uogólnienie ANOVA na **wiele zmiennych zależnych jednocześnie** (wektor wyników) względem jednego lub wielu czynników (i ewentualnych współzmiennych):

$$H_0:\ \boldsymbol{\mu}_1=\boldsymbol{\mu}_2=\dots=\boldsymbol{\mu}_k\quad(\text{równość wektorów średnich})$$

**Dlaczego nie wiele ANOVA?** MANOVA uwzględnia **korelacje między zmiennymi zależnymi**, kontroluje błąd I rodzaju przy wielu zmiennych i bywa skuteczniejsza niż oddzielne testy jednowymiarowe (może wykryć różnicę widoczną dopiero w kombinacji zmiennych).

**Założenia**: wielowymiarowy rozkład normalny; **jednorodność macierzy kowariancji** (np. test Boxa); niezależność obserwacji.

**Statystyki testowe** (z przybliżeniem rozkładem $F$): **lambda Wilksa**, **ślad Pillaia**, **ślad Hotellinga-Lawleya**, **największy pierwiastek Roya**. Po istotnym wyniku – analiza które zmienne różnicują (ANOVA dla każdej zmiennej) i testy post-hoc (Tukey).

### Przykład: irysy Fishera (1936)

4 zmienne (długość i szerokość działki kielicha, długość i szerokość płatka) × 3 gatunki (Setosa, Versicolor, Virginica), po 50 obserwacji. $H_0$: równość wektorów średnich 4 pomiarów w 3 grupach.

- Wilks $\Lambda=0{,}0234$, $F(8,288)=199{,}1$, $p<0{,}001$ → odrzucamy $H_0$ (wszystkie cztery testy zgodne),
- ANOVA dla każdej zmiennej osobno: $F=119{,}3$ (dł. działki), $49{,}2$ (szer. działki), $1180{,}2$ (dł. płatka), $960{,}0$ (szer. płatka) – wszystkie $p<0{,}001$,
- Tukey HSD: wszystkie pary gatunków różnią się istotnie.

## Przykłady zastosowań

- **medycyna**: porównanie skuteczności kilku terapii/leków; pomiary parametru przed–w trakcie–po (powtarzane pomiary); MANOVA dla zestawu parametrów laboratoryjnych,
- **rolnictwo** (pierwotne zastosowanie): wpływ nawozu i odmiany na plon (dwuczynnikowa, z interakcją),
- **przemysł**: wpływ metody produkcji i zakładu na koszty; kontrola jakości,
- **psychologia/socjologia**: wpływ płci i wykształcenia na postawy,
- **informatyka/ML**: porównanie błędów wielu algorytmów (testy Friedmana), badanie wpływu hiperparametrów na jakość modelu, eksperymenty UX (A/B/n),
- **biologia/taksonomia**: różnicowanie gatunków po zestawie pomiarów (MANOVA, analiza dyskryminacyjna).

## Podsumowanie

- Wnioskowanie = estymacja + testowanie hipotez; kluczowe: $H_0$, $\alpha$, $p$-wartość, błędy I i II rodzaju.
- Testy parametryczne wymagają normalności (i jednorodności wariancji); gdy nie są spełnione – nieparametryczne (rangi).
- **ANOVA**: $H_0:\ \mu_1=\dots=\mu_k$; rozkład $SS_{total}=SS_{between}+SS_{within}$; $F=MS_b/MS_w$; po istotnym $F$ – post-hoc (Tukey).
- Warianty: jednoczynnikowa, powtarzane pomiary, dwuczynnikowa (bez/z powtórzeniami, interakcja); nieparametryczne: Kruskal-Wallis, Friedman.
- **MANOVA**: wiele zmiennych zależnych naraz, testy Wilksa/Pillaia/Hotellinga/Roya; wymaga wielowymiarowej normalności i jednorodności macierzy kowariancji.

---
[⬅️ Poprzedni temat](2_Metody_estymacji_gęstości_rozkładu_prawdopodobieństwa.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Modele_regresji_Regresja_wielokrotna.md)