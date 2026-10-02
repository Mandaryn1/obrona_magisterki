# Metody wnioskowania wielokryterialnego – wprowadzenie

## Problem wielokryterialny

W zadaniach rzeczywistych rzadko liczy się jedno kryterium (np. tylko cena). Zwykle trzeba **jednocześnie** uwzględnić kilka, często **sprzecznych** kryteriów (koszt vs jakość vs czas). Zadanie wielokryterialne ma postać:

$$f_k(x)\to\max\quad (k=1,\dots,s),\qquad x\in D$$

- $x$ – decyzja (rozwiązanie, wariant),
- $D$ – zbiór decyzji **dopuszczalnych**,
- $f_k$ – funkcja celu (kryterium cząstkowe); kryterium „im mniej, tym lepiej” zamieniamy na „max” przez $-f_k$.

Ponieważ kryteria są ze sobą sprzeczne, **zwykle nie istnieje decyzja najlepsza jednocześnie według wszystkich kryteriów**. Zamiast „optimum” szukamy **rozwiązania kompromisowego**, które zależy od **preferencji decydenta**.

### Dwa ujęcia

| Ujęcie | Zbiór decyzji $D$ | Nazwy | Typowe metody |
| :--- | :--- | :--- | :--- |
| **dyskretne** | skończony zbiór **wariantów** | wielokryterialna analiza decyzyjna (MCDA/MCDM), wielokryterialne wspomaganie decyzji | AHP, TOPSIS, ELECTRE, PROMETHEE, suma ważona |
| **ciągłe** | zbiór opisany ograniczeniami (nieskończenie wiele decyzji) | optymalizacja wielokryterialna, polioptymalizacja | programowanie wielokryterialne, ε-ograniczeń, punkt idealny, algorytmy ewolucyjne (NSGA-II) |

### Macierz decyzyjna (przypadek dyskretny)

Warianty $W_1,\dots,W_n$ oceniane według kryteriów $K_1,\dots,K_s$:

| | $K_1$ | $K_2$ | … | $K_s$ |
| :--- | :-: | :-: | :-: | :-: |
| $W_1$ | $f_{11}$ | $f_{12}$ | … | $f_{1s}$ |
| … | … | … | … | … |
| $W_n$ | $f_{n1}$ | $f_{n2}$ | … | $f_{ns}$ |

Do tego dochodzi **wektor wag** $\mathbf{w}=(w_1,\dots,w_s)$, $w_k\ge0$, $\sum w_k=1$ (jeśli kryteria nie są równoważne).

## Podstawowe pojęcia

- **Kryterium stymulanta (zysk)** – im więcej, tym lepiej (wydajność, jakość); **destymulanta (koszt)** – im mniej, tym lepiej (cena, czas, zużycie energii); **nominanta** – najlepsza jest wartość pośrednia/nominalna.
- **Punkt idealny** $z^*=(z_1^*,\dots,z_s^*)$, $z_k^*=\max_{x\in D}f_k(x)$ – najlepsze osiągalne wartości każdego kryterium z osobna; zwykle **nieosiągalny** jednocześnie.
- **Punkt antyidealny (nadir)** $m=(m_1,\dots,m_s)$, $m_k=\min f_k$ – najgorsze wartości (w praktyce: najgorsze wartości na zbiorze Pareto).
- **Dominacja, rozwiązanie Pareto-optymalne (sprawne, efektywne)** – zob. temat 4.
- **Rozwiązanie kompromisowe** – jedno rozwiązanie wybrane przez decydenta ze zbioru sprawnych.

### Zgodność kryteriów

Dla kryteriów $K_1, K_2$ i dowolnych decyzji $x_1, x_2\in D$:

- **zgodne**: $K_1(x_1)\le K_1(x_2)\Rightarrow K_2(x_1)\le K_2(x_2)$ (poprawa jednego niesie poprawę drugiego) – wtedy problem sprowadza się do jednego kryterium,
- **przeciwstawne**: $K_1(x_1)\le K_1(x_2)\Rightarrow K_2(x_1)\ge K_2(x_2)$ (poprawa jednego pogarsza drugie) – każde rozwiązanie jest sprawne,
- **niezgodne**: w pozostałych przypadkach (zależność nie jest jednoznaczna) – typowa sytuacja.

## Podejścia do rozwiązywania

| Podejście | Kiedy decydent podaje preferencje | Przykłady |
| :--- | :--- | :--- |
| **a priori** | **przed** obliczeniami; metoda agreguje kryteria do jednego wyniku/rankingu | suma ważona (metakryterium), metoda leksykograficzna, AHP, TOPSIS, punkt idealny |
| **a posteriori** | **po** wyznaczeniu zbioru (frontu) Pareto | algorytmy ewolucyjne (NSGA-II, SPEA2), skalaryzacje parametryczne |
| **interaktywne** | **w trakcie** – decydent kolejno koryguje oczekiwania | STEM, metody punktu odniesienia |

### Najważniejsze metody agregacji

| Metoda | Idea |
| :--- | :--- |
| **Metakryterium (suma ważona)** | $u(x)=\sum_k w_kf_k(x)$; maksymalizujemy użyteczność. Wymaga normalizacji kryteriów (temat 1) i wag |
| **Kryterium główne + drugorzędne** (ε-ograniczeń) | maksymalizujemy jedno kryterium, pozostałym narzucamy minimalne poziomy $f_k(x)\ge p_k$ |
| **Ścisła hierarchia celów (leksykograficzna)** | kryteria uporządkowane wg ważności, optymalizacja po kolei (temat 2) |
| **Minimalizacja odległości od punktu idealnego** | wybieramy rozwiązanie najbliższe punktu idealnego (np. $\min\max_k\frac{z_k-f_k(x)}{z_k-m_k}$) |
| **AHP** | wagi i oceny z porównań parami (temat 3) |
| **TOPSIS** | bliskość do rozwiązania idealnego i oddalenie od antyidealnego |
| **ELECTRE, PROMETHEE** | relacje przewyższania: porównania parami z progami, zgodność/niezgodność |
| **Reguły większościowe** | wariant lepszy, gdy jest lepszy według większej liczby kryteriów (temat 5) |

## Wnioski ogólne

- Uporządkowanie wariantów **zależy od przyjętych kryteriów, wag i metody**; kryteria podobne mogą prowadzić do różnych wyników.
- Kryteria i wagi odzwierciedlają **preferencje decydenta**, a nie obiektywną rzeczywistość – nie ma rozwiązania „obiektywnie najlepszego”, jest najlepsze **w sensie przyjętych preferencji**.
- Dobra praktyka: (1) sformułować cele i kryteria, (2) zbudować macierz decyzyjną, (3) **wyeliminować warianty zdominowane** (zostawić zbiór Pareto), (4) znormalizować kryteria, (5) wyznaczyć wagi, (6) wybrać metodę agregacji, (7) **analiza wrażliwości** (czy ranking jest stabilny przy zmianie wag/progów).
- Porównywanie decyzji bywa wrażliwe na **progi nierozróżnialności**: różnica wartości kryterium poniżej progu nie zmienia preferencji (np. $D_1$ lepsza od $D_2$, gdy kryterium jest większe o $p\%$; $D_3$ gorsza od $D_2$, gdy mniejsze o $q\%$; progi $p$ i $q$ mogą być różne – brak symetrii).

---
[⬅️ Poprzedni temat](MetodyWnioskowaniaWielokryterialnego_tytul.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](1_Główne_zadania_normalizacji_wartości_kryteriów_optymalizacji.md)