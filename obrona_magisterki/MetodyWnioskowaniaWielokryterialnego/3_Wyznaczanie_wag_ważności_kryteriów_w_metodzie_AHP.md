# Wyznaczanie wag ważności kryteriów w metodzie AHP

## Czym jest AHP

**AHP (Analytic Hierarchy Process, analityczny proces hierarchiczny)** – metoda wielokryterialnego wspomagania decyzji opracowana przez **T. Saaty'ego** (lata 70. XX w.). Opiera się na:

1. **hierarchizacji problemu**,
2. **porównaniach parami** (kryteriów między sobą oraz wariantów względem każdego kryterium),
3. wyznaczeniu **wag (priorytetów)** z macierzy porównań,
4. **agregacji** do rankingu wariantów,
5. kontroli **spójności** ocen decydenta.

Pytanie dotyczy kroku 3 – skąd biorą się **wagi ważności kryteriów**.

## Hierarchia problemu

Poziom 1: **cel** (np. wybór laptopa) → poziom 2: **kryteria** (cena, wydajność, serwis; ewentualnie podkryteria) → poziom najniższy: **warianty**.

## Krok 1: porównania parami kryteriów

Decydent (ekspert) dla każdej pary kryteriów $(i,j)$ ocenia, **o ile ważniejsze jest kryterium $i$ od $j$**, według **skali Saaty'ego 1–9**:

| Wartość $a_{ij}$ | Znaczenie |
| :-: | :--- |
| 1 | kryteria jednakowo ważne |
| 3 | umiarkowana przewaga $i$ nad $j$ |
| 5 | silna przewaga |
| 7 | bardzo silna (wyraźnie wykazana) przewaga |
| 9 | ekstremalna przewaga |
| 2, 4, 6, 8 | wartości pośrednie (kompromis) |
| $1/k$ | odwrotność – $j$ jest ważniejsze od $i$ w stopniu $k$ |

Powstaje **macierz porównań parami** $\mathbf{A}=[a_{ij}]_{n\times n}$ ($n$ – liczba kryteriów):

- $a_{ii}=1$,
- **własność odwrotnej symetrii**: $a_{ji}=\dfrac{1}{a_{ij}}$,
- wystarczy zatem ocenić $\dfrac{n(n-1)}{2}$ par.

Gdyby decydent był idealnie spójny, zachodziłaby **przechodniość**: $a_{ij}\cdot a_{jk}=a_{ik}$, a macierz miałaby postać $a_{ij}=w_i/w_j$.

## Krok 2: wyznaczenie wag

Wagi $\mathbf{w}=(w_1,\dots,w_n)$, $\sum w_i=1$, otrzymuje się z macierzy $\mathbf{A}$.

### Metoda główna: wektor własny (Saaty)

Waga to **znormalizowany główny wektor własny** macierzy $\mathbf{A}$, tj. wektor odpowiadający największej wartości własnej $\lambda_{\max}$:

$$\mathbf{A}\,\mathbf{w}=\lambda_{\max}\,\mathbf{w},\qquad \sum_{i}w_i=1$$

Dla macierzy w pełni spójnej $\lambda_{\max}=n$; w praktyce $\lambda_{\max}\ge n$. Wektor własny można wyznaczyć metodą potęgową (iteracyjnie mnożymy macierz przez wektor i normalizujemy).

### Metody przybliżone (do obliczeń ręcznych)

1. **Normalizacja kolumn i uśrednianie wierszy**: 
   - podziel każdy element przez sumę swojej kolumny, 
   - wagę $w_i$ = średnia arytmetyczna $i$-tego wiersza znormalizowanej macierzy.
2. **Średnia geometryczna wierszy**: 
   $$w_i=\frac{\left(\prod_{j=1}^{n}a_{ij}\right)^{1/n}}{\sum_{k=1}^{n}\left(\prod_{j=1}^{n}a_{kj}\right)^{1/n}}$$
   (przy macierzy w pełni spójnej wszystkie metody dają ten sam wynik; przy niespójnej wyniki nieco się różnią, a średnia geometryczna bywa stosowana jako prostsza alternatywa dla wektora własnego; dla macierzy $3\times3$ oba podejścia dają identyczny rezultat).

## Krok 3: kontrola spójności

Ludzkie oceny nie muszą być spójne (np. $A>B$, $B>C$, ale $C>A$). AHP mierzy to wskaźnikami:

$$CI=\frac{\lambda_{\max}-n}{n-1},\qquad CR=\frac{CI}{RI}$$

- $\lambda_{\max}$ liczymy np. jako $\frac{1}{n}\sum_i\frac{(\mathbf{A}\mathbf{w})_i}{w_i}$,
- $RI$ – **losowy wskaźnik spójności** (średni $CI$ dla losowych macierzy tego rozmiaru):

| $n$ | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| $RI$ | 0 | 0 | 0,58 | 0,90 | 1,12 | 1,24 | 1,32 | 1,41 | 1,45 | 1,49 |

**Reguła**: $CR<0{,}1$ (10%) → spójność akceptowalna. Jeśli $CR\ge0{,}1$, decydent powinien **zrewidować** porównania (najbardziej niespójne pary).

## Przykład

Kryteria: **K1 – cena**, **K2 – wydajność**, **K3 – serwis**. Ocena decydenta: cena umiarkowanie ważniejsza od wydajności (3), silnie ważniejsza od serwisu (5), wydajność umiarkowanie ważniejsza od serwisu (3):

$$\mathbf{A}=\begin{bmatrix}1&3&5\\ \tfrac13&1&3\\ \tfrac15&\tfrac13&1\end{bmatrix}$$

**Metoda przybliżona (normalizacja kolumn)**: sumy kolumn: $1{,}533;\ 4{,}333;\ 9$. Znormalizowana macierz:

| | K1 | K2 | K3 | średnia wiersza = waga |
| :--- | :-: | :-: | :-: | :-: |
| K1 | 0,652 | 0,692 | 0,556 | **0,633** |
| K2 | 0,217 | 0,231 | 0,333 | **0,260** |
| K3 | 0,130 | 0,077 | 0,111 | **0,106** |

**Dokładnie** (wektor własny; dla macierzy $3\times3$ to samo daje średnia geometryczna wierszy): $\mathbf{w}=(0{,}637;\ 0{,}258;\ 0{,}105)$, $\lambda_{\max}=3{,}039$.

**Spójność**: $CI=\dfrac{3{,}039-3}{2}=0{,}019$, $CR=\dfrac{0{,}019}{0{,}58}=0{,}033<0{,}1$ → oceny są spójne.

Wniosek: cena ma wagę ok. 64%, wydajność 26%, serwis 10%.

## Dalsze kroki AHP (po wyznaczeniu wag)

1. Dla **każdego kryterium** buduje się osobną macierz porównań parami **wariantów** i wyznacza **lokalne priorytety** wariantów $p_{ik}$ (tak samo: wektor własny, kontrola spójności).
2. **Synteza** – globalny priorytet wariantu $i$:

$$P_i=\sum_{k=1}^{s}w_k\,p_{ik}$$

3. Ranking: wariant o największym $P_i$ jest najlepszy.

(Przy hierarchii wielopoziomowej wagi podkryteriów mnoży się przez wagi kryteriów nadrzędnych.)

## Zalety i wady

| Zalety | Wady / krytyka |
| :--- | :--- |
| wygodne dla decydenta – tylko porównania **parami** | liczba porównań rośnie kwadratowo ($n(n-1)/2$) |
| łączy kryteria **ilościowe i jakościowe** | skala 1–9 jest ograniczona i subiektywna |
| wbudowana **kontrola spójności** | możliwe **odwrócenie rang** przy dodaniu/usunięciu wariantu |
| przejrzysta hierarchia, łatwa do wytłumaczenia | wynik zależy od ekspertów; niski $CR$ nie gwarantuje trafności ocen |
| nie wymaga normalizacji kryteriów | założenie niezależności kryteriów (ograniczenie rozwiązuje ANP) |
| możliwa agregacja ocen wielu ekspertów (średnia geometryczna) | |

## Podsumowanie

- Wagi kryteriów w AHP wyznacza się z **macierzy porównań parami** ocenianych w **skali Saaty'ego 1–9** ($a_{ji}=1/a_{ij}$).
- Wagi = **znormalizowany główny wektor własny** macierzy ($\mathbf{A}\mathbf{w}=\lambda_{\max}\mathbf{w}$, $\sum w=1$); w praktyce przybliża się je normalizacją kolumn lub **średnią geometryczną wierszy**.
- Spójność ocen sprawdza się wskaźnikami $CI=\frac{\lambda_{\max}-n}{n-1}$ i $CR=CI/RI$; akceptowalne $CR<0{,}1$.
- Końcowy ranking: $P_i=\sum_k w_kp_{ik}$.

---
[⬅️ Poprzedni temat](2_Metoda_leksykograficzna.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Warianty_optymalne_w_sensie_Pareto.md)