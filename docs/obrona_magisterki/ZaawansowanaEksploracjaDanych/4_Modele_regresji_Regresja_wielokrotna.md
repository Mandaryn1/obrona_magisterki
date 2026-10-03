# Modele regresji. Regresja wielokrotna. Postawienie zagadnienia

## 1. Istota i cel analizy regresji

* **Definicja:** Analiza regresji to metoda statystyczna służąca do badania zależności i modelowania związku pomiędzy zmienną zależną (objaśnianą, celem $(Y)$) a jedną lub wieloma zmiennymi niezależnymi (objaśniającymi $(X_1, X_2, \dots, X_k)$).
* **Cel:** Opis charakteru związku (kształt, kierunek) oraz predykcja (prognozowanie) wartości zmiennej zależnej na podstawie znanych wartości zmiennych objaśniających.

---

## 2. Regresja wielokrotna – postawienie zagadnienia

* **Postać liniowego modelu regresji wielokrotnej:**  
  Gdy zmienna celu $(Y)$ zależy od $(k)$ zmiennych objaśniających, model liniowy w populacji przyjmuje postać:
  $[Y = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \dots + \beta_k X_k + \epsilon]$
  lub w zapisie macierzowym:
  $[y = X\beta + \epsilon$]
  gdzie:
  * $(y$) – $(n$)-wymiarowy wektor obserwacji zmiennej zależnej.
  * $(X$) – macierz obserwacji na zmiennych objaśniających o wymiarze $(n \times (k+1)$) (pierwsza kolumna jedynek odpowiada wyrazowi wolnemu).
  * $(\beta$) – wektor nieznanych parametrów (współczynników regresji).
  * $(\epsilon$) – wektor składników losowych (błędów/zakłóceń).

* **Interpretacja współczynników ($(\beta_j$)):**  
  Współczynnik $(\beta_j$) informuje, o ile średnio zmieni się wartość zmiennej zależnej $(Y$), gdy wartość zmiennej objaśniającej $(X_j$) wzrośnie o jedną jednostkę, przy założeniu, że pozostałe zmienne objaśniające w modelu pozostają bez zmian (*ceteris paribus*).

---

## 3. Założenia klasycznego modelu regresji liniowej

Poprawność wnioskowania w modelu regresji wymaga spełnienia założeń dotyczących składnika losowego $(\epsilon$):
* **Oczekiwana wartość błędu wynosi zero:** $(E(\epsilon_i) = 0$).
* **Stałość wariancji (homoscedastyczność):** $(Var(\epsilon_i) = \sigma^2$) dla wszystkich obserwacji.
* **Brak autokorelacji:** Składniki losowe dla różnych obserwacji są niezależne ($(Cov(\epsilon_i, \epsilon_j) = 0$) dla $(i \neq j$)).
* **Rozkład normalny:** Składnik losowy ma rozkład normalny $(\epsilon \sim N(0, \sigma^2)$).

---

## 4. Estymacja parametrów i ocena jakości modelu

* **Metoda Najmniejszych Kwadratów (MNK / KNM):**  
  Estymatory $(\hat{\beta}$) wyznacza się tak, aby zminimalizować sumę kwadratów reszt (odchyleń wartości rzeczywistych od wartości przewidywanych przez model):
  $[SS_E = \sum (y_i - \hat{y}_i)^2 \rightarrow \min$]
* **Współczynnik determinacji ($(R^2$)):**  
  Mierzy, jaka część całkowitej zmienności zmiennej $(Y$) została wyjaśniona przez liniowy model regresji. Przyjmuje wartości z przedziału $($).
* **Skorygowany $(R^2$) ($(R^2_{adj}$)):**  
  Uwzględnia "karę" za liczbę zmiennych wprowadzonych do modelu, zapobiegając sztucznemu zawyżaniu $(R^2$) przez nieistotne zmienne objaśniające.
* **Weryfikacja istotności (ANOVA dla regresji):**
  * **Test $(F$) (istotność całego modelu):** weryfikuje hipotezę zerową $(H_0: \beta_1 = \beta_2 = \dots = \beta_k = 0$) (brak liniowej zależności od całego zbioru zmiennych).
  * **Test $(t$)-Studenta (istotność pojedynczej zmiennej):** weryfikuje hipotezę $(H_0: \beta_j = 0$) przy założeniu obecności pozostałych zmiennych w modelu.

## Podsumowanie

- Regresja wielokrotna: $Y=\beta_0+\sum\beta_jX_j+\varepsilon$; estymacja MNK: $\mathbf{b}=(\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T\mathbf{y}$.
- Założenia: średnia błędów 0, stała wariancja, niezależność, normalność, brak współliniowości.
- Ocena: $R^2$ i $R^2_{adj}$, test $F$ (całość), testy $t$ (współczynniki), analiza reszt.
- Zmienne jakościowe → zmienne sztuczne ($m-1$); wybór zmiennych: forward, backward, stepwise, najlepsze podzbiory, Lasso.
- Alternatywa dla MNK: spadek gradientowy (SGD).
- Pokrewne: szeregi czasowe (trend + sezonowość), regresja logistyczna (klasyfikacja, MLE).

---
[⬅️️ Poprzedni temat](3_Wnioskowanie_statystyczne_ANOVA_MANOVA.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Metody_redukcji_wymiaru_i_liczności_próby.md)