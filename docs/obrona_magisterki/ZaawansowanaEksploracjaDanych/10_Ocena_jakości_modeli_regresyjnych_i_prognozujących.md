# Ocena jakości modeli regresyjnych i prognozujących

> **💬 Gotowa wypowiedź ustna:**
> *"W modelach regresyjnych i prognozujących przewidujemy wartości ciągłe, a jakość oceniamy na podstawie analizy błędów, czyli różnic między wartością rzeczywistą a prognozowaną.
> 
> Do głównych miar należą MAE, określające średni błąd w oryginalnych jednostkach, RMSE, które silniej karze duże odchylenia, oraz MAPE pokazujący błąd w procentach. Dopasowanie modelu mierzymy współczynnikiem $R^2$. Bardzo ważną zasadą przy szeregach czasowych jest zakaz stosowania losowej walidacji krzyżowej – dane musimy dzielić ściśle chronologicznie, uczyć model na przeszłości i testować na przyszłości, aby uniknąć wycieku danych."*

## 1. Podstawowe miary błędu predykcji

* **MAE (Mean Absolute Error – Średni błąd bezwzględny):**  
  $[MAE = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|]$  
  Mierzy średnie odchylenie predykcji od wartości rzeczywistych w tych samych jednostkach co zmienna objaśniana. Jest mało wrażliwy na pojedyncze wartości odstające.
* **MSE (Mean Squared Error – Błąd średniokwadratowy):**  
  $[MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2]$  
  Silnie karze duże błędy ze względu na podnoszenie odchyleń do kwadratu.
* **RMSE (Root Mean Squared Error – Pierwiastek błędu średniokwadratowego):**  
  $[RMSE = \sqrt{MSE}]$  
  Pierwiastek z błędu średniokwadratowego; przywraca jednostkę zmiennej zależnej, zachowując jednocześnie wyższą wrażliwość na duże błędy.
* **MAPE (Mean Absolute Percentage Error – Średni procentowy błąd bezwzględny):**  
  $[MAPE = \frac{100\%}{n} \sum_{i=1}^{n} \left|\frac{y_i - \hat{y}_i}{y_i}\right|]$  
  Wyraża błąd w procentach, co umożliwia łatwą interpretację i porównywanie modeli na różnych zbiorach. Nie nadaje się do stosowania, gdy wartości rzeczywiste $(y_i)$ są równe zero.

---

## 2. Miary dopasowania modelu

* **Współczynnik determinacji ($(R^2)$):**  
  Mierzy frakcję wariancji zmiennej zależnej wyjaśnianą przez model. Przyjmuje wartości z przedziału $()$ (lub procentowo $(0\% - 100\%)$).
* **Skorygowany $(R^2)$ ($(R^2_{adj})$):**  
  Modyfikacja $(R^2)$ uwzględniająca liczbę zmiennych w modelu. Chroni przed sztucznym wzrostem wskaźnika przy dodawaniu kolejnych, nieistotnych zmiennych objaśniających.

---

## 3. Diagnoza i analiza reszt modelu

* **Analiza reszt ($(e_i = y_i - \hat{y}_i)$):** Jakość modelu regresyjnego wymaga zweryfikowania założeń dotyczących składnika losowego:
  * **Oczekiwana wartość równa zero:** średnia z reszt powinna wynosić 0.
  * **Homoscedastyczność:** stałość wariancji reszt dla wszystkich predykcji (np. test Breuscha-Pagana).
  * **Normalność rozkładu reszt:** sprawdzana testami statystycznymi (np. Shapiro-Wilka) lub na wykresie Q-Q.
  * **Brak autokorelacji reszt:** reszty nie powinny wywoływać wzorców zależnych od siebie (np. test Durbina-Watsona).

---

## 4. Specyfika oceny modeli prognozujących (szeregi czasowe)

* **Ochrona przed wyciekiem danych w czasie (Time Series Split):**  
  W szeregach czasowych nie wolno stosować losowej walidacji krzyżowej (k-fold CV). Dane dzieli się ściśle chronologicznie (np. walidacja wprzód / *rolling window*), aby model nie uczył się na danych z "przyszłości".
* **Ocena prognoz ex-post:**  
  Polega na porównaniu wartości prognozowanych z wartościami rzeczywistymi, które rzeczywiście wystąpiły po postawieniu prognozy (wykorzystując metryki MAE, RMSE, MAPE).
* **Test Diebolda-Mariano:**  
  Test statystyczny służący do formalnego porównania dokładności prognoz wygenerowanych przez dwa różne modele szeregów czasowych.

## Podsumowanie

- Błąd $e_i=y_i-\hat{y}_i$ na danych testowych; podstawowe miary: **MAE, MSE, RMSE, $R^2$** ($R^2_{adj}$, ln $L$/AIC/BIC dla porównań modeli).
- MAE – średni błąd w jednostkach zmiennej; RMSE – bardziej karze duże błędy; $R^2$ – część wyjaśnionej zmienności.
- Dla **prognoz** dodatkowo: ME (obciążenie), **MPE, MAPE, sMAPE, MASE** (miary względne i skalowane).
- Szeregi czasowe: podział **chronologiczny**, walidacja z przesuwanym początkiem, porównanie z prognozą naiwną.
- Zawsze ocena na danych niewidzianych (hold-out, k-fold CV) i analiza reszt.

---
[⬅️ Poprzedni temat](9_Ocena_jakości_modeli_klasyfikacyjnych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../MetodyWnioskowaniaWielokryterialnego/MetodyWnioskowaniaWielokryterialnego_tytul.md)