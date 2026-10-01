# Metoda leksykograficzna

## Idea

Metoda leksykograficzna (**ścisła hierarchia celów**) stosuje się, gdy decydent potrafi **uporządkować kryteria od najważniejszego do najmniej ważnego** i uważa, że **ważniejsze kryterium jest nieskończenie ważniejsze od dowolnego mniej ważnego**. Nazwa pochodzi od porządku **słownikowego**: porównując słowa, patrzymy na pierwszą literę; dopiero gdy są takie same – na drugą itd.

- kryteria nie podlegają wymianie (**brak kompensacji**): duży zysk na kryterium dalszym nie zrekompensuje nawet minimalnej straty na wcześniejszym,
- **nie wymaga wag liczbowych ani normalizacji** – wystarczy kolejność ważności (skala porządkowa),
- stosowana m.in. w sytuacjach, gdy istnieją twarde priorytety (bezpieczeństwo > koszt > wygoda).

## Definicja formalna

Niech kryteria są ponumerowane tak, że $f_1$ jest najważniejsze, $f_2$ drugie itd. (wszystkie maksymalizowane). Wektor ocen $f(x)$ jest **leksykograficznie większy** od $f(y)$, co zapisujemy $x\succ_{lex}y$, jeśli:

$$\exists k:\ f_i(x)=f_i(y)\ \ \forall i<k\quad\text{oraz}\quad f_k(x)>f_k(y)$$

Rozwiązaniem leksykograficznym jest decyzja $x^*$, dla której nie istnieje $y\in D$ takie, że $y\succ_{lex}x^*$.

## Algorytm (procedura sekwencyjna)

1. Zdefiniuj $D_1=D$. Maksymalizuj najważniejsze kryterium: $f_1^*=\max_{x\in D_1}f_1(x)$.
2. Utwórz $D_2=\{x\in D_1:\ f_1(x)=f_1^*\}$ – zbiór rozwiązań optymalnych dla $f_1$.
   - Jeśli $D_2$ ma **jeden** element → koniec, to rozwiązanie leksykograficzne.
3. Na $D_2$ maksymalizuj $f_2$: $D_3=\{x\in D_2: f_2(x)=\max_{D_2}f_2\}$.
4. Powtarzaj dla kolejnych kryteriów aż zostanie jedno rozwiązanie albo wyczerpią się kryteria (wtedy wszystkie pozostałe są równoważne).

W wersji **dyskretnej** (wariantów) odpowiada to **sortowaniu wierszy macierzy decyzyjnej** wg kolumn w kolejności ważności (jak sortowanie w arkuszu kalkulacyjnym według kilku kolumn).

## Przykład

Wybór dostawcy. Kryteria w kolejności ważności: cena (min) → jakość (max) → czas dostawy (min):

| Dostawca | Cena | Jakość | Czas [dni] |
| :--- | :-: | :-: | :-: |
| W1 | 100 | 8 | 3 |
| W2 | 100 | 9 | 5 |
| W3 | 100 | 9 | 2 |
| W4 | 120 | 10 | 1 |
| W5 | 100 | 7 | 1 |

1. Cena: minimum 100 → zostają W1, W2, W3, W5 (W4 odpada, choć ma najlepszą jakość i czas),
2. Jakość: maksimum 9 → zostają W2, W3,
3. Czas: minimum 2 dni → **W3**.

Rozwiązanie: **W3**. Zauważ, że W4 nigdy nie był brany pod uwagę, mimo że jest lepszy na dwóch z trzech kryteriów – to typowa cecha metody.

## Wersja z tolerancją (łagodzona hierarchia celów)

Ścisła wersja jest bardzo restrykcyjna (przy kryteriach ciągłych zwykle już $f_1$ wyznacza jedno rozwiązanie i kolejne kryteria nie mają znaczenia). Dlatego wprowadza się **dopuszczalne odstępstwo** od optimum ważniejszego kryterium.

Dla kryterium $f_k$ definiujemy na bieżącym zbiorze $D_k$:

$$M_k=\max_{x\in D_k}f_k(x),\quad m_k=\min_{x\in D_k}f_k(x),\quad t_k=M_k-m_k$$

oraz współczynnik odstępstwa $d_k\in[0,1]$ (lub bezwzględną tolerancję $\Delta_k$). Zbiór dla następnego kryterium:

$$D_{k+1}=\{x\in D_k:\ f_k(x)\ge M_k-d_k\,t_k\}$$

i maksymalizujemy $f_{k+1}$ na $D_{k+1}$. Ostatnie zadanie ($k=s$) daje rozwiązanie kompromisowe.

- $d_k=0$ → klasyczna metoda leksykograficzna (żadnego odstępstwa),
- $d_k=1$ → kryterium $f_k$ jest ignorowane,
- pośrednie $d_k$ → kompromis: poświęcamy trochę ważniejszego kryterium, by zyskać na dalszych.

**Przykład z tolerancją**: w zadaniu powyżej dla ceny dopuszczamy odstępstwo $\Delta=25$ (cena $\le125$). Wtedy do dalszej analizy wchodzi też W4 (cena 120), a ma najwyższą jakość 10 → **W4** staje się rozwiązaniem. Tolerancja zmieniła wynik.

## Własności

- Rozwiązanie leksykograficzne jest **Pareto-optymalne** (efektywne): gdyby istniała decyzja nie gorsza na wszystkich kryteriach i lepsza na co najmniej jednym, byłaby ona leksykograficznie większa – sprzeczność.
- Dla kryteriów ciągłych zwykle wyznacza **jedno** rozwiązanie.
- **Zależność od kolejności** kryteriów: inna permutacja daje (zwykle) inny wynik.
- Należy do metod **a priori** (preferencje podane przed obliczeniami).

## Zalety i wady

| Zalety | Wady |
| :--- | :--- |
| prosta, intuicyjna, łatwa do wytłumaczenia | **skrajna niekompensacyjność** – nawet mikroskopijna przewaga na kryterium ważniejszym przesądza wynik |
| nie wymaga wag ani normalizacji | gdy $f_1$ ma jedno optimum, kryteria drugorzędne **nie mają żadnego znaczenia** |
| wystarcza skala porządkowa (kolejność ważności) | duża wrażliwość na drobne różnice wartości (stąd potrzeba tolerancji) |
| gwarantuje rozwiązanie sprawne (Pareto) | trudno uzasadnić „nieskończoną" przewagę kryteriów dla wielu praktycznych zastosowań |
| łatwa implementacja (sortowanie, sekwencja zadań optymalizacji) | wynik zależy od uporządkowania kryteriów, a nie od wielkości różnic |

## Porównanie z sumą ważoną

| | Metoda leksykograficzna | Suma ważona |
| :--- | :--- | :--- |
| Preferencje | tylko **kolejność** kryteriów | **wagi liczbowe** |
| Kompensacja | **brak** | **jest** (duży zysk rekompensuje stratę) |
| Normalizacja | niepotrzebna | konieczna |

## Podsumowanie

- Kryteria porządkuje się od najważniejszego; kolejno **maksymalizuje się** każde z nich na zbiorze rozwiązań optymalnych dla poprzednich.
- Odpowiada porządkowi słownikowemu; brak kompensacji i wag.
- Wersja z tolerancją (współczynniki odstępstwa $d_k$) łagodzi surowość metody.
- Rozwiązanie jest Pareto-optymalne, ale wynik zależy wyłącznie od kolejności i bardzo wrażliwy na drobne różnice na kryteriach najważniejszych.
