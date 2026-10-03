# Zaawansowana eksploracja danych – wprowadzenie

## Podstawowe pojęcia

- **Eksploracja danych (data mining, DM)** – proces, który na dużych zbiorach danych ma za zadanie odkryć **reguły i wzorce**. Wykorzystuje algorytmy uczenia maszynowego.
- **Uczenie maszynowe (machine learning, ML)** – analiza, projektowanie i rozwój algorytmów, które automatycznie doskonalą się (uczą) na podstawie własnych wyników.
- **Metody statystyczne (SM)** – modele probabilistyczne; wiele metod DM i ML to przeniesienie metod znanych ze statystyki.
- **Odkrywanie wiedzy (KDD)** – pojęcie szersze niż eksploracja danych. Oznacza **cały proces** złożony z kilku etapów (przygotowanie danych, eksploracja, ocena, interpretacja).

> „Eksploracja danych jest procesem odkrywania znaczących nowych powiązań, wzorców i trendów przez przeszukiwanie dużej ilości danych (...) przy wykorzystaniu metod rozpoznawania wzorców, jak również metod statystycznych i matematycznych” (Larose).

Przy niewłaściwych analizach lub błędnych założeniach można otrzymać nieprawdziwe wyniki, dlatego trzeba rozumieć teorię statystyczną stojącą za algorytmami.

## Etapy odkrywania wiedzy

| Nr | Etap | Na czym polega |
| :-: | :--- | :--- |
| 1 | Czyszczenie danych (*data cleaning*) | usuwanie danych niepełnych i niepoprawnych |
| 2 | Integracja danych (*data integration*) | łączenie danych z heterogenicznych źródeł w jeden zbiór |
| 3 | Selekcja danych (*data selection*) | wybór danych istotnych z punktu widzenia analizy |
| 4 | Konsolidacja i transformacja | konwersja typów atrybutów, dyskretyzacja wartości ciągłych |
| 5 | Eksploracja danych (*data mining*) | zastosowanie wybranych metod (sieci neuronowe, drzewa decyzyjne itd.) |
| 6 | Ocena (*evaluation*) | ocena jakości wzorców lub modeli |
| 7 | Interpretacja i wizualizacja (*presentation*) | prezentacja wyników, wybór najciekawszej wiedzy |

Czyszczenie i przygotowanie danych bywa nawet **80% czasu pracy analityka**. Zasada: *Garbage In, Garbage Out* – algorytmy są bardzo czułe na jakość danych źródłowych.

## Rodzaje metod eksploracji danych

### Ze względu na charakter

- **metody opisu danych** – odkrywają wcześniej nieznane reguły i wzorce opisujące ogólne cechy zbiorów (np. analiza koszyka zakupów),
- **metody predykcji danych** – przewidują trendy i zachowania (np. wynik terapii, zachowanie klienta na aukcji).

### Ze względu na cel

- odkrywanie asocjacji (reguły asocjacyjne),
- klasyfikacja i predykcja,
- grupowanie (analiza skupień),
- odkrywanie charakterystyk,
- analiza sekwencji i przebiegów czasowych,
- eksploracja tekstu i danych semistrukturalnych,
- eksploracja WWW,
- eksploracja grafów i sieci społecznościowych,
- eksploracja danych multimedialnych,
- eksploracja danych przestrzennych,
- wykrywanie anomalii (punktów odstających),
- rekomendacja.

## Przekształcanie danych (data wrangling)

1. Usuwanie zbędnych kolumn (ID, kolumny z dużą liczbą braków, o małej wariancji).
2. Usuwanie wierszy z pustymi wartościami.
3. Usuwanie szumu: zastąpienie błędnych wartości przybliżeniami, usunięcie obserwacji odstających, normalizacja rozkładów.
4. Uzupełnianie (*impute*) brakujących wartości.
5. Transformacja danych kategorycznych i dat.
6. Transformacja cech: dyskretyzacja (*binning*), skalowanie, normalizacja.
7. Redukcja wymiarowości (usuwanie skorelowanych cech, wybór cech, PCA).
8. Podział na zbiór uczący i testowy.

### Obsługa braków danych

- Zastąpienie stałą (średnia, mediana, dominanta) – zasada minimalnej zmiany rozkładu.
- Usunięcie niekompletnych przykładów – tylko gdy danych jest dużo, a usuwamy nie więcej niż ok. 20%.
- Usunięcie zmiennej, jeśli braków jest powyżej ok. 25%.
- Uzupełnienie na podstawie innych zmiennych (np. model regresji, k-NN).

W Pythonie (pandas): `dropna`, `fillna`, `isnull` / `notnull`, `duplicated`.

## Przypomnienie z statystyki opisowej

- **Cecha jakościowa** (niemierzalna – płeć, zawód) i **ilościowa** (mierzalna – wzrost, dochód; ciągła lub skokowa).
- **Populacja** (zbiorowość generalna) i **próba**. Próba musi być **losowa i reprezentatywna** – każda jednostka ma równe szanse trafienia do niej.
- **Miary położenia**: średnia arytmetyczna, harmoniczna (dla jednostek względnych, np. km/h), geometryczna, dominanta (moda), mediana, kwantyle (kwartyle, decyle, percentyle).
- **Miary zmienności**: rozstęp, wariancja, odchylenie standardowe, odchylenie przeciętne, rozstęp międzykwartylowy $IQR = Q_3 - Q_1$, odchylenie ćwiartkowe, współczynnik zmienności $V = s/\bar{x}$ (miara względna).
- **Miary asymetrii** (skośność) i **koncentracji** (kurtoza). Dla rozkładu normalnego skośność i kurtoza (nadwyżkowa) są bliskie 0.

### Rozkład normalny $N(\mu, \sigma)$

$$f(x)=\frac{1}{\sigma\sqrt{2\pi}}\,e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$

- symetryczny, jednomodalny, kształt dzwonu; średnia = mediana = dominanta,
- punkty przegięcia w $\mu \pm \sigma$,
- **reguła trzech sigm**: w $\mu\pm\sigma$ jest ok. 68,3% obserwacji, w $\mu\pm 1{,}96\sigma$ – 95%, w $\mu\pm3\sigma$ – 99,7%,
- **standaryzacja**: $U=\frac{X-\mu}{\sigma} \sim N(0,1)$.

---
[⬅️ Poprzedni temat](ZaawansowanaEksploracjaDanych_tytul.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](1_Metody_identyfikacji_obserwacji_odstających.md)