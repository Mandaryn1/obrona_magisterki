# Ocena jakości modeli regresyjnych i prognozujących

## Idea

Model regresyjny (lub prognozujący szereg czasowy) przewiduje wartość **ciągłą** $\hat{y}_i$. Jakość oceniamy porównując ją z wartością rzeczywistą $y_i$ na danych **testowych** (niewidzianych w uczeniu). Podstawą jest **błąd (reszta)**:

$$e_i=y_i-\hat{y}_i$$

($n$ – liczba obserwacji testowych, $\bar{y}$ – średnia wartości rzeczywistych).

Trafność predykcji ocenia się według kilku miar; do oceny trafności i wiarygodności modeli regresyjnych i klasyfikacyjnych stosuje się **k-krotną walidację krzyżową**.

## Miary dla modeli regresyjnych

| Miara | Wzór | Charakterystyka |
| :--- | :--- | :--- |
| **MAE** – średni błąd bezwzględny | $\dfrac{1}{n}\sum\lvert e_i\rvert$ | w jednostkach zmiennej; odporny na outliery; wszystkie błędy tak samo ważne |
| **MSE** – błąd średniokwadratowy | $\dfrac{1}{n}\sum e_i^2$ | silnie **karze duże błędy**; jednostki kwadratowe; wrażliwy na outliery; różniczkowalny (funkcja celu MNK) |
| **RMSE** – pierwiastek z MSE | $\sqrt{\dfrac{1}{n}\sum e_i^2}$ | w jednostkach zmiennej; $\text{RMSE}\ge\text{MAE}$; duża różnica = obecność dużych błędów |
| **$R^2$** – współczynnik determinacji | $1-\dfrac{SSE}{SST}=1-\dfrac{\sum(y_i-\hat{y}_i)^2}{\sum(y_i-\bar{y})^2}$ | część zmienności wyjaśniona przez model; $1$ – idealny, $0$ – tak dobry jak średnia, **może być ujemny** na zbiorze testowym |
| **$R^2_{adj}$** – skorygowany | $1-(1-R^2)\dfrac{n-1}{n-k-1}$ | kara za liczbę zmiennych $k$ |
| **logarytm funkcji wiarygodności** ($\ln L$) | – | dopasowanie modelu probabilistycznego; podstawa AIC/BIC |
| **AIC / BIC** | $2k-2\ln L$ / $k\ln n-2\ln L$ | porównanie modeli o różnej złożoności (mniejsze = lepsze) |

Wskazówki:

- MAE jest bardziej intuicyjny, RMSE preferowany, gdy **duże błędy są szczególnie niepożądane**,
- błędy bezwzględne zależą od skali zmiennej – nie nadają się do porównań między różnymi zmiennymi/zbiorami (wtedy $R^2$ lub miary względne),
- $R^2$ liczony na zbiorze uczącym nie wykrywa przeuczenia – trzeba go liczyć także na zbiorze testowym,
- zawsze analizować **reszty** (wykres reszt względem wartości dopasowanych, histogram/Q-Q plot, autokorelacja): powinny być losowe, o średniej 0, stałej wariancji, bez struktury.

### Przykład liczbowy

$y=(3;\,5;\,2;\,7)$, $\hat{y}=(2{,}5;\,5{,}5;\,2;\,8)$ → błędy $(0{,}5;\,-0{,}5;\,0;\,-1)$:

- MAE $=\frac{0{,}5+0{,}5+0+1}{4}=0{,}5$,
- MSE $=\frac{0{,}25+0{,}25+0+1}{4}=0{,}375$, RMSE $\approx0{,}612$,
- $\bar{y}=4{,}25$, $SST=14{,}75$, $SSE=1{,}5$ → $R^2=1-\frac{1{,}5}{14{,}75}\approx0{,}898$.

## Miary dla modeli prognozujących (szeregi czasowe)

Stosuje się te same miary co dla regresji, a dodatkowo **miary względne (procentowe)** i skalowane, ponieważ szeregi czasowe mają różne skale.

| Miara | Wzór | Uwagi |
| :--- | :--- | :--- |
| **ME / MFE** – błąd średni | $\dfrac{1}{n}\sum e_t$ | pokazuje **obciążenie** (systematyczne zawyżanie/zaniżanie); błędy różnych znaków się znoszą |
| **MAE** | $\dfrac{1}{n}\sum\lvert e_t\rvert$ | |
| **RMSE** | $\sqrt{\frac{1}{n}\sum e_t^2}$ | |
| **MPE** – średni błąd procentowy | $\dfrac{100\%}{n}\sum\dfrac{e_t}{y_t}$ | kierunek błędu względnego |
| **MAPE** – średni bezwzględny błąd procentowy | $\dfrac{100\%}{n}\sum\left\lvert\dfrac{e_t}{y_t}\right\rvert$ | **intuicyjny, niezależny od skali**; nie działa dla $y_t=0$, asymetryczny (większa kara za zawyżenie niż zaniżenie) |
| **sMAPE** – symetryczny MAPE | $\dfrac{100\%}{n}\sum\dfrac{2\lvert e_t\rvert}{\lvert y_t\rvert+\lvert\hat{y}_t\rvert}$ | łagodzi problemy MAPE |
| **MASE** – średni bezwzględny błąd skalowany | $\dfrac{\frac{1}{n}\sum\lvert e_t\rvert}{\frac{1}{T-1}\sum_{t=2}^{T}\lvert y_t-y_{t-1}\rvert}$ | błąd względem **prognozy naiwnej** (poprzednia wartość); $<1$ – model lepszy od naiwnego; odporny na skalę i zera |

Dla przykładu liczbowego powyżej MAPE $=\frac{100\%}{4}\left(\frac{0{,}5}{3}+\frac{0{,}5}{5}+0+\frac{1}{7}\right)\approx10{,}2\%$.

### Specyfika walidacji szeregów czasowych

- **Nie wolno losowo mieszać** obserwacji (przyszłość nie może „wyciekać” do uczenia): zbiór testowy to **ostatni fragment szeregu**, podział chronologiczny,
- **walidacja z przesuwanym początkiem** (*rolling origin / walk-forward*, `TimeSeriesSplit`): uczenie na danych do $t$, prognoza dla $t+1\dots t+h$, przesunięcie okna,
- porównanie z **modelem naiwnym** (ostatnia wartość / wartość sprzed sezonu) – prognoza musi być lepsza od naiwnej (MASE<1),
- ocena osobno dla różnych horyzontów prognozy; analiza reszt (autokorelacja – test Ljung-Boxa).

## Ewaluacja i walidacja – ogólnie

- **Hold-out**: np. 80% uczący / 20% testowy,
- **k-fold CV** (zwykle $k=10$): wynik = średnia z $k$ przebiegów; ogranicza przeuczenie, lepsze wykorzystanie danych (nie stosować dla szeregów czasowych w klasycznej postaci),
- **przeuczenie** (overfitting): bardzo mały błąd na uczącym, duży na testowym; **niedouczenie**: duży błąd na obu (model zbyt prosty).

*Reguła powrotu do średniej*: wartości nietypowe z czasem dążą do średniej – model uczony na danych historycznych może gorzej prognozować w nowych warunkach.

## Narzędzia (Python)

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
import numpy as np

mae  = mean_absolute_error(y_test, y_pred)
mse  = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
r2   = r2_score(y_test, y_pred)
mape = np.mean(np.abs((y_test - y_pred) / y_test)) * 100
```

## Podsumowanie

- Błąd $e_i=y_i-\hat{y}_i$ na danych testowych; podstawowe miary: **MAE, MSE, RMSE, $R^2$** ($R^2_{adj}$, ln $L$/AIC/BIC dla porównań modeli).
- MAE – średni błąd w jednostkach zmiennej; RMSE – bardziej karze duże błędy; $R^2$ – część wyjaśnionej zmienności.
- Dla **prognoz** dodatkowo: ME (obciążenie), **MPE, MAPE, sMAPE, MASE** (miary względne i skalowane).
- Szeregi czasowe: podział **chronologiczny**, walidacja z przesuwanym początkiem, porównanie z prognozą naiwną.
- Zawsze ocena na danych niewidzianych (hold-out, k-fold CV) i analiza reszt.
