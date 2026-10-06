# Modele regresji. Regresja wielokrotna. Postawienie zagadnienia

> **💬 Gotowa wypowiedź ustna:**
> *"Analiza regresji służy do modelowania zależności i prognozowania zmiennej objaśnianej $Y$ na podstawie zmiennych objaśniających $X$. W regresji wielokrotnej uwzględniamy wiele czynników naraz, a każdy współczynnik regresji mówi, o ile zmieni się wartość $Y$, gdy dana zmienna wzrośnie o jednostkę przy niezmienionych pozostałych zmiennych.
> 
> Parametry szacujemy Metodą Najmniejszych Kwadratów, minimalizując sumę kwadratów reszt. Aby wnioskowanie było poprawne, reszty modelu muszą mieć średnią zerową, stałą wariancję, brak autokorelacji oraz rozkład normalny. Jakość modelu oceniamy współczynnikiem determinacji $R^2$ oraz skorygowanym $R^2$, a istotność sprawdzamy testem F dla całego modelu oraz testem t dla poszczególnych zmiennych."*

**Regresja** to sposób **przewidywania jednej liczby na podstawie innych**. Szukamy zależności i zapisujemy ją jako prosty „przepis", którym potem można prognozować.

Przykład: chcemy przewidzieć **cenę mieszkania**. Cena to **zmienna objaśniana** (to, co przewidujemy). Dane, z których przewidujemy, to **zmienne objaśniające**, np. metraż, liczba pokoi, piętro, odległość od centrum.

**Regresja prosta** używa tylko jednej zmiennej objaśniającej (np. samego metrażu). Zależność to wtedy linia prosta: im większy metraż, tym wyższa cena.

**Regresja wielokrotna** używa **kilku zmiennych objaśniających naraz**. Cena zależy od metrażu, liczby pokoi, piętra i odległości od centrum jednocześnie. Model liczy dla każdej z nich **współczynnik**, który mówi, o ile zmienia się cena, gdy ta zmienna wzrośnie o jednostkę, a pozostałe się nie zmieniają. Do tego dochodzi stała wartość wyjściowa i **błąd losowy** (część ceny, której model nie wyjaśnia).

**Postawienie zagadnienia:**

- mamy zebrane dane o wielu przypadkach (mieszkaniach), w których znamy zarówno cechy, jak i prawdziwą cenę,
- szukamy takich współczynników, żeby przewidywane ceny były **jak najbliższe prawdziwym**. Zwykle robi się to metodą najmniejszych kwadratów: wybieramy współczynniki, dla których suma kwadratów błędów jest najmniejsza,
- gotowy model pozwala przewidzieć cenę nowego mieszkania i ocenić, **które cechy mają największy wpływ**.

**Założenia modelu** (żeby wyniki były wiarygodne):

- zależność jest w przybliżeniu **liniowa**,
- błędy są losowe, niezależne od siebie i mają podobny rozrzut,
- zmienne objaśniające **nie powtarzają tej samej informacji** (np. metraż i liczba metrów w pokojach).

**Jak ocenia się model:**

- **R²:** jaka część zmienności cen jest wyjaśniona przez model (blisko 1 to dobrze),
- **testy istotności:** czy dana cecha naprawdę wpływa na cenę, czy to przypadek,
- **analiza błędów:** czy nie ma wyraźnych wzorców, które świadczą o złym modelu.

**Dodatki:**

- cechy jakościowe (np. dzielnica) zamienia się na zmienne 0/1,
- zmienne do modelu wybiera się metodami: dołączanie, eliminacja lub krokowa,
- **regresja logistyczna** to pokrewna metoda dla odpowiedzi typu tak/nie.

**W skrócie:** regresja wielokrotna zamienia wiele cech na jedną prognozę i pokazuje, jak mocno każda cecha na nią wpływa.

## Podsumowanie

- Regresja wielokrotna: $Y=\beta_0+\sum\beta_jX_j+\varepsilon$; estymacja MNK: $\mathbf{b}=(\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T\mathbf{y}$.
- Założenia: średnia błędów 0, stała wariancja, niezależność, normalność, brak współliniowości.
- Ocena: $R^2$ i $R^2_{adj}$, test $F$ (całość), testy $t$ (współczynniki), analiza reszt.
- Zmienne jakościowe → zmienne sztuczne ($m-1$); wybór zmiennych: forward, backward, stepwise, najlepsze podzbiory, Lasso.
- Alternatywa dla MNK: spadek gradientowy (SGD).
- Pokrewne: szeregi czasowe (trend + sezonowość), regresja logistyczna (klasyfikacja, MLE).

---
[⬅️️ Poprzedni temat](3_Wnioskowanie_statystyczne_ANOVA_MANOVA.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Metody_redukcji_wymiaru_i_liczności_próby.md)