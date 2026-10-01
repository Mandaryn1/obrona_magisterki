# Metody estymacji gęstości rozkładu prawdopodobieństwa

## Pojęcia podstawowe

**Gęstość** $f(x)$ zmiennej losowej ciągłej to funkcja, dla której:

1. $f(x) \ge 0$ dla każdego $x$,
2. $\int_{-\infty}^{\infty} f(x)\,dx = 1$,

a prawdopodobieństwo trafienia w przedział to $P(a \le X \le b) = \int_a^b f(x)\,dx$.

**Dystrybuanta**: $F(x) = P(X < x) = \int_{-\infty}^{x} f(t)\,dt$, przy czym $f(x) = F'(x)$.

**Estymacja gęstości** – oszacowanie nieznanej funkcji $f$ na podstawie próby losowej $X_1, X_2, \dots, X_n$. Zastosowania: poznanie kształtu rozkładu (skośność, wielomodalność), wykrywanie anomalii, analiza skupień (klastry = maksima gęstości), klasyfikacja bayesowska.

## Przegląd metod

### Metody parametryczne (klasyczne)

Arbitralnie **wybieramy typ rozkładu** (normalny, wykładniczy, gamma, ...), a z próby szacujemy tylko jego parametry (np. metodą największej wiarygodności: $\hat\mu = \bar{x}$, $\hat\sigma^2 = s^2$).

- (+) prosta, mało danych wystarcza, zwarty opis,
- (−) ograniczona do kilkunastu standardowych typów gęstości; **błędny wybór rozkładu daje błędny wynik**.

Rozszerzeniem są **mieszaniny rozkładów** (np. mieszaniny gaussowskie, GMM) – sumy ważone kilku składowych, pozwalają modelować wielomodalność.

### Metody nieparametryczne

Nie zakładają postaci rozkładu – kształt wynika z danych.

1. **histogram** (najprostsza),
2. **estymatory najbliższego sąsiedztwa** (k-NN),
3. **estymatory jądrowe** (kernel density estimation, KDE).

> Metody z wykładu: histogram, estymator najbliższego sąsiedztwa, estymator jądrowy (omówiona szczegółowo: **estymator jądrowy**).

## Histogram

Dla rodziny przedziałów o szerokości $h$ i próby $X_1,\dots,X_n$:

$$\hat{f}(x) = \frac{1}{n h}\,\#\{\,i : X_i \in \text{przedział zawierający } x\,\}$$

**Wady**:

- parametr $h$ (szerokość klasy) ma zasadnicze znaczenie: zbyt duży **maskuje cechy rozkładu**, zbyt mały daje **wiele lokalnych ekstremów**,
- jest **nieciągły**, a jego pochodna wynosi zero wszędzie poza punktami styku klas (gdzie nie istnieje),
- zależy od wyboru lewego końca pierwszego przedziału.

**Zaleta**: łatwość konstrukcji i interpretacji – podstawowe narzędzie wizualizacji w początkowej analizie danych.

## Estymator najbliższego sąsiedztwa

$$\hat{f}(x) = \frac{k-1}{2\,n\,d_k(x)}$$

gdzie $d_k(x)$ – odległość punktu $x$ od jego $k$-tego najbliższego sąsiada w próbie, a $k$ najczęściej przyjmuje się jako część całkowitą z $\sqrt{n}$. (W literaturze spotyka się też wariant z licznikiem $k$; idea jest ta sama: gęstość jest odwrotnie proporcjonalna do rozmiaru obszaru zawierającego $k$ sąsiadów.)

Intuicja: tam, gdzie dane są gęste, $k$ sąsiadów znajdzie się blisko (małe $d_k$) → duża gęstość.

Własności:

- wykres jest „sklejeniem hiperbol”, w miejscach sklejenia pochodna nie istnieje,
- kształt nieregularny,
- **nie spełnia warunku $\int \hat{f} = 1$** (nie jest prawdziwą gęstością),
- ogony zanikają bardzo wolno.

**Przykład z wykładu**: próba `2,1  2,4  2,5  2,6  4,1  4,5  5,2`; $n=7$, $k=\lfloor\sqrt{7}\rfloor=2$. Dla $x=2$: odległości do próby to `0,1; 0,4; 0,5; 0,6; 2,1; 2,5; 3,2`, drugi najbliższy sąsiad jest w odległości $d_2 = 0{,}4$, więc $\hat{f}(2) = \frac{1}{2\cdot 7\cdot 0{,}4} \approx 0{,}179$. Dla $x=2{,}4$: $d_2 = 0{,}1$, więc $\hat f(2{,}4) \approx 0{,}714$.

## Estymator jądrowy (omówienie)

### Idea

Zamiast „schodków” histogramu, w **każdym punkcie próby** umieszczamy gładką „górkę” (jądro), a wszystkie górki **sumujemy**. Wynik jest ciągły (i różniczkowalny, jeśli jądro jest różniczkowalne).

### Wzór

$$\hat{f}_h(x) = \frac{1}{n h}\sum_{i=1}^{n} K\!\left(\frac{x - X_i}{h}\right)$$

- $K$ – **jądro**: funkcja symetryczna względem zera, z maksimum globalnym w zerze, spełniająca $\int K(u)\,du = 1$,
- $h > 0$ – **parametr wygładzania** (szerokość pasma, *bandwidth*).

Estymator dziedziczy własności różniczkowalności po jądrze $K$.

### Najczęściej stosowane jądra (jednowymiarowe)

| Jądro | Wzór (dla $\lvert u\rvert \le 1$) | Strata efektywności względem Epanecznikowa |
| :--- | :--- | :-: |
| **Epanecznikowa** | $\frac{3}{4}(1-u^2)$ | 0% |
| Jednostajne | $\frac{1}{2}$ | ok. 7% |
| Trójkątne | $1-\lvert u\rvert$ | ok. 1% |
| Dwuwagowe (biweight) | $\frac{15}{16}(1-u^2)^2$ | ok. 1% |
| Normalne (gaussowskie) | $\frac{1}{\sqrt{2\pi}}e^{-u^2/2}$ (nośnik nieograniczony) | ok. 5% |

**Wybór jądra nie ma istotnego wpływu na jakość estymacji.** Decyduje wygoda (postać analityczna, ograniczoność nośnika). Wybór kierowany jest kryterium minimalizacji **scałkowanego błędu średniokwadratowego** (MISE).

### Parametr wygładzania $h$ – najważniejszy

- $h$ **za małe** → wiele lokalnych ekstremów (estymator „poszarpany”, przeuczony do próby),
- $h$ **za duże** → nadmierne wygładzenie, ukrywa cechy rozkładu (np. wielomodalność).

Na wykładzie porównano $h=0{,}2$ i $h=0{,}8$ dla próby `2,1  2,4  2,5  2,6  4,1  4,5  5,2`.

Metody doboru $h$ (minimalizacja MISE):

| Metoda | Zastosowanie | Opis |
| :--- | :--- | :--- |
| **przybliżona** (reguła kciuka) | 1D | podstawienie wartości funkcjonału dla rozkładu normalnego; dla jądra normalnego $h \approx 1{,}06\,\hat\sigma\, n^{-1/5}$ |
| **podstawień** (plug-in) | 1D | szacuje nieznany funkcjonał gęstości (zależny od drugiej pochodnej $f''$) z danych; lepsza, im bardziej rozkład zbliżony do normalnego |
| **krzyżowego uwiarygodnienia** (cross-validation) | wielowymiarowy | wybór $h$ na podstawie danych bez użycia tej obserwacji, dla której liczymy estymator |

### Modyfikacja parametru wygładzania (zmienna szerokość jądra)

Zamiast jednego $h$ dla wszystkich jąder, każde jądro dostaje własny modyfikator $s_i$:

$$\hat{f}(x) = \frac{1}{n}\sum_{i=1}^{n}\frac{1}{h s_i}K\!\left(\frac{x-X_i}{h s_i}\right)$$

- najpierw wyznacza się estymator podstawowy $\hat{f}$,
- potem $s_i = \left(\hat{f}(X_i)/g\right)^{-c}$, gdzie $g$ – średnia geometryczna wartości $\hat{f}(X_i)$, $c$ – stała (zwykle 0,5).

Efekt: w obszarach o **małej gęstości** jądra są szersze (większe wygładzenie), w gęstych – węższe. Poprawia to estymację w ogonach.

### Przypadek wielowymiarowy

Dla $x \in \mathbb{R}^d$ stosuje się:

- **jądro produktowe** – iloczyn jąder jednowymiarowych dla poszczególnych współrzędnych (z osobnym parametrem wygładzania dla każdej),
- **jądro radialne** – zależy od odległości od środka.

### Zastosowania

- wizualizacja rozkładu (krzywa zamiast histogramu),
- **analiza skupień metodą gęstościową** – klastry odpowiadają **modom** (lokalnym maksimom) estymatora jądrowego (Fukunaga i Hostetler),
- wykrywanie anomalii (obszary o bardzo małej gęstości),
- klasyfikacja bayesowska (estymacja gęstości w klasach).

## Porównanie metod

| Cecha | Parametryczna | Histogram | k-NN | Jądrowa |
| :--- | :--- | :--- | :--- | :--- |
| Założenie o rozkładzie | tak | nie | nie | nie |
| Ciągłość | tak | **nie** | tak (nieróżniczkowalna) | **tak**, gładka |
| Całka równa 1 | tak | tak | **nie** | tak |
| Kluczowy parametr | typ rozkładu | szerokość klasy $h$ | liczba sąsiadów $k$ | **szerokość $h$**, jądro $K$ |
| Główna wada | ryzyko złego modelu | schodkowy, zależy od siatki | nieregularny, ogony | dobór $h$, koszt obliczeń |

## Podsumowanie

- Estymacja gęstości = odtworzenie $f(x)$ z próby; podejście parametryczne (wybór rozkładu) lub nieparametryczne.
- Nieparametryczne: histogram (prosty, ale nieciągły), k-NN (nieregularny), **jądrowy** (ciągły, gładki – najlepszy).
- Estymator jądrowy: $\hat f_h(x) = \frac{1}{nh}\sum K\big(\frac{x-X_i}{h}\big)$; **jądro mało ważne, $h$ kluczowe**.
- $h$ dobiera się minimalizując MISE (metody: przybliżona, podstawień, cross-validation).
- Modyfikacja $h$ pozwala dopasować wygładzenie do lokalnej gęstości.
