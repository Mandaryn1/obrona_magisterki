# Systemy rekomendacji – rodzaje i omówienie przykładowego

## Czym jest system rekomendacji

Zadaniem systemu rekomendacji jest **wybranie z wielu dostępnych opcji tych, które odpowiadają potrzebom użytkownika**. Rekomendowanie jest **uogólnieniem wyszukiwania** – system musi zwrócić wynik także wtedy, gdy użytkownik **nie sprecyzował** wszystkich kryteriów. Rekomendacja to w istocie **system predykcji** (przewidywanie, co spodoba się użytkownikowi).

Zastosowania:

- wybieranie wyświetlanych reklam,
- sugerowanie zakupu produktów,
- polecanie książek, filmów, muzyki,
- wybór płatnych linków w wyszukiwarkach,
- tworzenie profili demograficznych i behawioralnych użytkowników.

### Dane wejściowe

Macierz **użytkownik × przedmiot** z ocenami (jawnymi: gwiazdki, lajki; lub niejawnymi: kliknięcia, czas oglądania, zakupy). Jest bardzo **rzadka** (większość ocen brakuje). Zadanie systemu: **uzupełnić brakujące oceny** (przewidzieć je) i zwrócić najlepsze pozycje.

## Proste techniki

- oparte na **danych demograficznych** (np. „kobiety kupując samochód zwracają uwagę na kolor”) – zbyt ogólne,
- **podsumowania statystyczne** (najczęściej kupowane, średnia ocen, najczęściej polecane),
- wykorzystujące **korelację atrybutów** (czerwone buty + czerwona torebka),
- **analiza koszyka** (produkty kupowane razem, np. pieluszki i piwo w weekendy).

## Rodzaje systemów rekomendacji

Sposób uzupełniania brakujących danych jest podstawą podziału na modele:

| Rodzaj | Zasada działania | Uwagi |
| :--- | :--- | :--- |
| **Na podstawie wyszukiwania** | użytkownik wpisuje frazę, system wyszukuje | proste, brak personalizacji |
| **Na podstawie kategorii / klasyfikacji** | przedmioty przypisane do kategorii; wybór kategorii wg historii użytkownika lub zapytania | |
| **Przy użyciu grupowania** | użytkownicy dzieleni na klastry wg historii zakupów; użytkownik przypisany do klastra dostaje średnie klastra | |
| **Filtrowanie cech przedmiotu** (*content-based*) | profil użytkownika z jego zachowań + cechy produktów; model (regresja, odległość między wektorem użytkownika i wektorami produktów) | nie potrzebuje innych użytkowników; trudność: potrzeba cech produktów |
| **Filtrowanie kolektywne / kolaboratywne** (*collaborative filtering*) | wykorzystuje oceny **innych użytkowników** o podobnych gustach | najpopularniejsze; problem zimnego startu |
| **Oparte na asocjacjach** | reguły asocjacyjne z danych transakcyjnych (zbiory współwystępujących obiektów) | analiza koszykowa |
| **Oparte na wiedzy / demografii** | reguły eksperckie, dane demograficzne | |
| **Sesyjne** | rekomendacja z bieżącej sesji (sekwencja kliknięć) | |
| **Oparte na grafach heterogenicznych** | użytkownicy, przedmioty, atrybuty jako węzły grafu | |
| **Hybrydowe** | łączą zalety kilku metod | najlepsze w praktyce |

### Filtrowanie kolaboratywne – podejścia

- **statystyczne (memory-based)**: znajdź zbiór użytkowników **najbardziej podobnych** (odległość, korelacja Pearsona, cosinus), a ocenę przewidź jako **sumę ważoną** ocen podobnych osób (*user-based*). Wariant *item-based*: podobieństwo między przedmiotami,
- **modelowe (model-based)**: zbudowany model predykcyjny oceny, np. **faktoryzacja macierzy** (SVD), modele latentne,
- **zimny start (cold start)**: nowy użytkownik (nic o nim nie wiadomo) lub nowy, nieoceniany przedmiot → potrzebne inne techniki (popularność, demografia, content-based).

Mocne strony: nie wymaga wiedzy o cechach przedmiotów, odkrywa nieoczywiste powiązania. Słabe: zimny start, rzadkość danych, skalowalność, efekt „baniek”.

## Historia (przykłady systemów)

1. **1979** – „komputerowy bibliotekarz”: stereotypy użytkowników z krótkiego wywiadu.
2. **Tapestry (Xerox PARC, 1992)** – rekomendacja maili/wiadomości w grupach dyskusyjnych (filtrowanie współpracujące).
3. **GroupLens (Resnick, Riedl, 1994)** – filtrowanie newsów Usenet na podstawie opinii innych.
4. **Netflix Prize (2006–2009)** – konkurs z nagrodą 1 mln USD za poprawę algorytmu Cinematch o 10%; dane: ponad 100 mln ocen, ok. 480 tys. użytkowników, ok. 18 tys. filmów. Wygrał zespół BellKor's Pragmatic Chaos.
5. **Amazon (1998)** – algorytm **item-to-item collaborative filtering** (opatentowany); personalizacja sklepu, skuteczność mierzona kliknięciami i współczynnikiem konwersji.
6. **Spotify** – BaRT (*Bandits for Recommendations as Treatments*): mechanizm wielorękiego bandyty + filtrowanie kolaboratywne.
7. **Reddit** – rekomendacje wg profili użytkowników i subredditów.
8. **TikTok** – śledzenie niejawnych zachowań (przewinięcia, czas oglądania, lokalizacja) bez konieczności jawnych polubień.

## Omówienie przykładowego systemu: item-to-item collaborative filtering (Amazon)

### Idea

Zamiast szukać podobnych **użytkowników** (kosztowne przy milionach), szukamy podobnych **przedmiotów**. Podobieństwo przedmiotów liczy się **offline**, a rekomendacja w czasie rzeczywistym to tylko wyszukanie przedmiotów podobnych do już kupionych/ocenionych. Dobrze skaluje się.

### Algorytm

1. Zbuduj **binarną macierz użytkownik–przedmiot** (1 = kupił/obejrzał).
2. Dla każdej pary przedmiotów policz **podobieństwo kosinusowe** wektorów-kolumn:

$$\operatorname{sim}(i,j)=\cos(\mathbf{r}_i,\mathbf{r}_j)=\frac{\mathbf{r}_i\cdot\mathbf{r}_j}{\lVert\mathbf{r}_i\rVert\,\lVert\mathbf{r}_j\rVert}$$

3. Dla przedmiotów, które użytkownik już ma, polecaj te o najwyższym podobieństwie (jeszcze nieposiadane).

### Przykład (wartości z wykładu)

Przykładowa macierz zgodna z wynikami: użytkownik 1 kupił przedmioty {1, 3}, użytkownik 2 – {2, 3}, użytkownik 3 – {2}.

| | Item1 | Item2 | Item3 |
| :--- | :-: | :-: | :-: |
| U1 | 1 | 0 | 1 |
| U2 | 0 | 1 | 1 |
| U3 | 0 | 1 | 0 |

- $\operatorname{sim}(1,3)=\frac{1}{\sqrt{1\cdot2}}\approx0{,}71$, $\ \operatorname{sim}(2,3)=\frac{1}{\sqrt{2\cdot2}}=0{,}5$, $\ \operatorname{sim}(1,2)=0$,
- rekomendacje: Item1 → Item3; Item2 → Item3; Item3 → Item1, Item2.

## Systemy oparte na regułach asocjacyjnych

### Analiza koszykowa

**Analiza koszyka (Market Basket Analysis)** – odkrywanie grup produktów kupowanych razem. Każdy koszyk (transakcja) = zbiór produktów, reprezentowany jako binarny wektor (produkt jest/nie ma). Zastosowania: rozmieszczenie towarów, katalogi, promocje, decyzje biznesowe.

Dane: format **transakcyjny** (id transakcji, produkt – lepszy dla danych rzadkich) lub **macierzowy** (0/1 – lepszy dla gęstych).

### Reguła asocjacyjna

$$A\ \Rightarrow\ B,\qquad A\cap B=\emptyset$$

„Jeżeli klient kupuje $A$, to (z pewnym prawdopodobieństwem) kupi także $B$”. Miary jakości ($N$ – liczba wszystkich transakcji):

| Miara | Wzór | Znaczenie |
| :--- | :--- | :--- |
| **Wsparcie** (support) | $\operatorname{sup}(A\Rightarrow B)=\dfrac{\#(A\cup B)}{N}$ | jak często zbiór występuje; reguły o małym wsparciu dotyczą niewielu klientów; o bardzo dużym – mało interesujące |
| **Ufność** (confidence) | $\operatorname{conf}(A\Rightarrow B)=\dfrac{\#(A\cup B)}{\#A}=\dfrac{\operatorname{sup}(A\cup B)}{\operatorname{sup}(A)}$ | warunkowe prawdopodobieństwo $P(B\mid A)$; pewność reguły |
| **Lift** | $\operatorname{lift}(A\Rightarrow B)=\dfrac{\operatorname{conf}(A\Rightarrow B)}{\operatorname{sup}(B)}$ | ile razy częściej $A$ i $B$ występują razem niż przy niezależności; $=1$ niezależne, $>1$ dodatnia zależność, $<1$ ujemna |

Reguła jest „silna”/interesująca, gdy spełnia **minimalne progi** wsparcia (`minsup`) i ufności (`minconf`) ustawiane przez analityka. **Lift** chroni przed regułami trywialnymi (np. produkt kupowany przez wszystkich ma wysoką ufność, ale lift = 1).

### Algorytm Apriori (Agrawal i Srikant, 1993/94)

Proces dwuetapowy:

1. **Znajdź zbiory częste** (wsparcie $\ge$ `minsup`),
2. **Wygeneruj reguły** ze zbiorów częstych o ufności $\ge$ `minconf`.

**Własność a priori (monotoniczność wsparcia)**: *wszystkie podzbiory zbioru częstego są częste*; równoważnie – jeśli jakiś podzbiór jest rzadki, cały zbiór jest rzadki, więc nie trzeba go sprawdzać. To pozwala **przycinać** przestrzeń kandydatów (siatka podzbiorów).

Krok 1: $L_1$ = częste zbiory 1-elementowe. Dla $k=2,3,\dots$: generuj kandydatów $C_k$ przez łączenie $L_{k-1}$ (`apriori_gen`) i usuń tych, którzy mają rzadki podzbiór; przejrzyj bazę, zlicz wsparcie; $L_k=\{c\in C_k:\operatorname{sup}\ge\text{minsup}\}$. Koniec, gdy nie ma kandydatów.

Krok 2: dla każdego zbioru częstego $F$ i jego niepustego podzbioru $A$: reguła $A\Rightarrow F\setminus A$, jeśli $\frac{\operatorname{sup}(F)}{\operatorname{sup}(A)}\ge$ `minconf`.

Inne algorytmy: **Eclat**, **FP-Growth**, FreeSpan; dla danych ilościowych: GRI (uogólniona indukcja reguł) lub dyskretyzacja.

### Przykład obliczeniowy

Transakcje: `{chleb, jogurt}`, `{mleko, chleb, marchew}`, `{chleb, marchew}`, `{chleb, mleko}`, `{mleko, daktyle, marchew}`, `{mleko, daktyle, jogurt, chleb}`; $N=6$, `minsup` = 2 transakcje.

- zbiory 1-elementowe: chleb 5, mleko 4, marchew 3, jogurt 2, daktyle 2 (wszystkie częste),
- zbiory 2-elementowe częste: {chleb, mleko} 3, {chleb, marchew} 2, {chleb, jogurt} 2, {mleko, marchew} 2, {mleko, daktyle} 2,
- zbiory 3-elementowe: kandydaci nie przechodzą przycinania lub mają wsparcie < 2 → koniec.

Reguła **chleb ⇒ mleko**: wsparcie $3/6=50\%$, ufność $3/5=60\%$, lift $=0{,}6/(4/6)=0{,}9$ (<1: choć ufność wysoka, mleko jest kupowane często także bez chleba).
Reguła **mleko ⇒ chleb**: ufność $3/4=75\%$.

Przykład z wykładu (9 koszyków): mleko ⇒ ser: wsparcie $6/9=0{,}67$, ufność $6/6=1$, lift $=1/(7/9)=1{,}29$.

Przykład (woda mineralna i ciastka, 100 transakcji: oba 15, tylko woda 5, tylko ciastko 75, żadne 5): ufność woda ⇒ ciastko $=15/20=75\%$; ciastko ⇒ woda $=15/90\approx17\%$; lift $=0{,}75/0{,}9=0{,}83$.

### Zalety i wady rekomendacji asocjacyjnej

Zalety: czytelne, interpretowalne reguły; nie wymaga profili użytkowników; dobra do koszyków/cross-sellingu. Wady: eksplozja liczby reguł; brak personalizacji (jedna reguła dla wszystkich); trudny dobór progów; nie uwzględnia ocen ani kolejności.

## Ocena systemów rekomendacji

- trafność oceny: **RMSE, MAE** (przewidywane vs rzeczywiste oceny),
- jakość listy: **precision@k, recall@k, MAP, NDCG**,
- miary biznesowe: współczynnik klikalności (CTR), **współczynnik konwersji** (zakupy/odwiedziny),
- dodatkowo: pokrycie katalogu, różnorodność, nowość (*novelty*).

## Podsumowanie

- System rekomendacji przewiduje, co spodoba się użytkownikowi, nawet przy niepełnym zapytaniu.
- Rodzaje: wyszukiwanie, kategorie/klasyfikacja, grupowanie, filtrowanie cech przedmiotu (content-based), **filtrowanie kolaboratywne** (memory-/model-based, zimny start), asocjacje, wiedza/demografia, sesyjne, grafowe, **hybrydowe**.
- **Item-to-item CF (Amazon)**: macierz użytkownik–przedmiot → podobieństwo kosinusowe przedmiotów → polecanie podobnych do posiadanych.
- **Reguły asocjacyjne**: wsparcie, ufność, lift; Apriori wykorzystuje monotoniczność wsparcia (zbiór częsty ⇒ podzbiory częste).
- Historia: Tapestry (1992), GroupLens (1994), Amazon (1998), Netflix Prize (2006–09), Spotify, TikTok.
