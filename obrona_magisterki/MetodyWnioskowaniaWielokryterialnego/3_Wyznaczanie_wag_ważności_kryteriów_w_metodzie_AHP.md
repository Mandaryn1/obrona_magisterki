# W jaki sposób wyznaczane są wagi ważności kryteriów w metodzie AHP?

## Czym jest AHP

**AHP (Analytic Hierarchy Process, analityczny proces hierarchiczny)** – metoda wielokryterialnego wspomagania decyzji opracowana przez **T. Saaty'ego** (lata 70. XX w.). Opiera się na:

1. **hierarchizacji problemu**,
2. **porównaniach parami** (kryteriów między sobą oraz wariantów względem każdego kryterium),
3. wyznaczeniu **wag (priorytetów)** z macierzy porównań,
4. **agregacji** do rankingu wariantów,
5. kontroli **spójności** ocen decydenta.

Pytanie dotyczy kroku 3 – skąd biorą się **wagi ważności kryteriów**.

W metodzie **AHP** (Analytic Hierarchy Process, Saaty) wagi wyznacza się z **porównań parami kryteriów**, a nie przez bezpośrednie przypisywanie liczb.

**Kroki:**

1. **Macierz porównań parami.** Decydent porównuje każde dwa kryteria i ocenia, o ile jedno jest ważniejsze od drugiego, na **skali Saaty'ego 1–9**:
   - 1 – równie ważne,
   - 3 – umiarkowanie ważniejsze,
   - 5 – silnie ważniejsze,
   - 7 – bardzo silnie ważniejsze,
   - 9 – absolutnie ważniejsze,
   - wartości 2, 4, 6, 8 – pośrednie.

   Powstaje macierz A, w której a_ij to ważność i-tego kryterium względem j-tego. Na przekątnej są jedynki, a macierz jest **odwrotnie symetryczna**: a_ji = 1/a_ij.

2. **Wyznaczenie wag.** Wagi to składowe **głównego wektora własnego** macierzy A (odpowiadającego największej wartości własnej λ_max), znormalizowane tak, by sumowały się do 1. W praktyce stosuje się też przybliżenie: **normalizuje się każdą kolumnę** (dzieli przez jej sumę), a następnie **uśrednia się wiersze**. Można także użyć średniej geometrycznej wierszy.

3. **Sprawdzenie spójności ocen.** Oceny mogą być sprzeczne (A ważniejsze od B, B od C, a C od A). Oblicza się:
   - **wskaźnik spójności** CI = (λ_max − n) / (n − 1),
   - **współczynnik spójności** CR = CI / RI, gdzie RI to losowy wskaźnik zależny od liczby kryteriów n.

   Przyjmuje się, że ocena jest akceptowalna, gdy **CR < 0,1**. W przeciwnym razie decydent powinien poprawić porównania.

**Przykład dla trzech kryteriów** (macierz z ocenami 1→2: 3, 1→3: 5, 2→3: 2): wagi wynoszą ok. **0,65; 0,23; 0,12**, λ_max ≈ 3,004, CR ≈ 0,003, więc oceny są spójne.

Tę samą procedurę stosuje się potem do porównania wariantów względem każdego kryterium. Wagi kryteriów i oceny wariantów łączy się w końcowy ranking sumą ważoną.

## Podsumowanie

- Wagi kryteriów w AHP wyznacza się z **macierzy porównań parami** ocenianych w **skali Saaty'ego 1–9** ($a_{ji}=1/a_{ij}$).
- Wagi = **znormalizowany główny wektor własny** macierzy ($\mathbf{A}\mathbf{w}=\lambda_{\max}\mathbf{w}$, $\sum w=1$); w praktyce przybliża się je normalizacją kolumn lub **średnią geometryczną wierszy**.
- Spójność ocen sprawdza się wskaźnikami $CI=\frac{\lambda_{\max}-n}{n-1}$ i $CR=CI/RI$; akceptowalne $CR<0{,}1$.
- Końcowy ranking: $P_i=\sum_k w_kp_{ik}$.

---
[⬅️ Poprzedni temat](2_Metoda_leksykograficzna.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Warianty_optymalne_w_sensie_Pareto.md)