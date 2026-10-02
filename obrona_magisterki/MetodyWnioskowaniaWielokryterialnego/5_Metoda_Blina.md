# Metoda Blina

> **⚠️ Uwaga o źródłach – sprawdź z własnymi materiałami.**
> Nie znalazłem w literaturze opisu metody o dokładnie tej nazwie („metoda Blina" w wielokryterialnym wspomaganiu decyzji), więc **nie mogę potwierdzić, że poniższy opis jest tym, czego uczono na zajęciach**. To jest najbardziej prawdopodobna interpretacja, złożona z: (1) tego, co faktycznie wiadomo o pracach J. M. Blina, oraz (2) znanej procedury porządkowania wariantów regułą większości kryteriów, opisywanej w polskich wykładach z optymalizacji wielokryterialnej. Jeżeli masz slajdy/notatki z tego tematu, wyślij je – dopasuję opracowanie.

## Co wiadomo o Blinie

**J. M. Blin** (często z **A. B. Whinstonem**) – autor prac z początku lat 70. z **teorii wyboru społecznego i decyzji grupowych**:

- *Fuzzy sets and social choice* (J. Cybernetics, 1973),
- *Fuzzy relations in group decision theory* (J. Cybernetics, 1974),
- *Majority-rule under transitivity constraints* (Management Science, 1974).

Wspólny wątek: **agregowanie preferencji wielu „głosujących" za pomocą reguły większości**, relacje preferencji (także rozmyte) i problem **przechodniości** (tranzytywności) wyniku. W analizie wielokryterialnej rolę „głosujących" pełnią **kryteria** – każde „głosuje" na wariant, który jest według niego lepszy. Stąd wiąże się z metodą Blina **porządkowanie wariantów regułą większościową**.

## Idea (interpretacja robocza)

Warianty porównujemy **parami**. Wariant $r$ jest uznany za lepszy od $v$, jeżeli jest **lepszy według większej liczby kryteriów niż gorszy**. Metoda nie wymaga wag ani wartościowych różnic – wystarczy wiedzieć, na którym kryterium który wariant wygrywa (skala porządkowa).

## Procedura

Dane: warianty $r,v$; kryteria $i=1,\dots,m$; wartości $K_{ri}$ (kryteria maksymalizowane); progi nierozróżnialności $d_i\ge0$ (mogą być $0$).

1. **Porównanie na każdym kryterium**: $r$ jest lepszy od $v$ w sensie kryterium $i$, gdy
   $$K_{ri}-K_{vi}>d_i$$
   (analogicznie $v$ lepszy od $r$, gdy $K_{vi}-K_{ri}>d_i$; w przeciwnym razie – remis).
2. **Zliczenie**: $l(r,v)$ – liczba kryteriów, na których $r$ jest lepszy od $v$; $g(r,v)$ – liczba kryteriów, na których $r$ jest gorszy od $v$ (zawsze $g(r,v)=l(v,r)$).
3. **Reguła większości**: 
   $$r\succ v\ \iff\ l(r,v)>g(r,v)$$
   (wersja z wagami: porównujemy sumy wag kryteriów, na których każdy z wariantów wygrywa).
4. **Zbudowanie relacji** (macierz/graf „lepszy od") dla wszystkich par.
5. **Wybór najlepszych**: warianty, których **nie pokonuje żaden inny** (rdzeń/jądro relacji), albo ranking według liczby wygranych (np. **wynik Copelanda**: liczba wariantów pokonanych minus liczba wariantów, które pokonały dany).

## Przykład

Trzy warianty, trzy kryteria (maksymalizowane), progi $d_i=0$:

| | $K_1$ | $K_2$ | $K_3$ |
| :--- | :-: | :-: | :-: |
| X | 7 | 5 | 3 |
| Y | 5 | 3 | 7 |
| Z | 3 | 7 | 5 |

| Para | $l$ | $g$ | Wynik |
| :--- | :-: | :-: | :--- |
| X vs Y | 2 | 1 | X ≻ Y |
| Y vs Z | 2 | 1 | Y ≻ Z |
| Z vs X | 2 | 1 | Z ≻ X |

Powstaje **cykl** $X\succ Y\succ Z\succ X$ – wszystkie warianty są sobie równoważne w sensie tej relacji i rdzeń jest pusty.

## Główny problem: brak przechodniości (paradoks Condorcet'a)

Reguła większości **nie gwarantuje przechodniości** – jak w przykładzie, relacja „lepszy od" może mieć cykle. Wtedy nie powstaje jednoznaczny ranking. Sposoby radzenia sobie:

- **wynik Copelanda** (wygrane − przegrane), zamiast szukać zwycięzcy bezpośrednio,
- **zbiór Schwartza** / jądro relacji (najmniejszy zbiór niepokonany przez zewnętrzne warianty),
- wprowadzenie **wag kryteriów** (rozbija remisy i niektóre cykle),
- **progi** większości (np. wygrywa, gdy zgodne kryteria mają co najmniej 60% wag) i progi dla różnic – rozwiązanie rozwinięte w metodach **ELECTRE** (indeks zgodności i niezgodności),
- wymuszenie przechodniości (domknięcie relacji) – zagadnienie bliskie tematyce prac Blina i Whinstona o regule większości przy ograniczeniach przechodniości.

## Własności

- **Zgodna z dominacją Pareto**: jeśli $r$ dominuje $v$ (nie gorszy wszędzie, lepszy gdzieś), to $g(r,v)=0$, $l(r,v)\ge1$, więc $r\succ v$. Wariant zdominowany jest więc zawsze pokonany przez swojego dominatora, a zatem ewentualny wariant niepokonany (zwycięzca) nigdy nie jest zdominowany w sensie Pareto.
- **Niekompensacyjna**: liczy się tylko, **na ilu** kryteriach wariant wygrywa, a nie o ile. Ogromna przewaga na jednym kryterium waży tyle, co symboliczna na innym.
- **Nie wymaga normalizacji** ani liczbowych wag (wystarczy skala porządkowa), więc nadaje się do kryteriów jakościowych.
- Wynik zależy od **progów** $d_i$: im większe, tym więcej remisów.

## Zalety i wady

| Zalety | Wady |
| :--- | :--- |
| prosta, intuicyjna (reguła większości) | **brak przechodniości** – cykle, brak jednoznacznego rankingu |
| nie wymaga wag i normalizacji | ignoruje wielkość różnic (poza progami) |
| odpowiednia dla danych porządkowych i jakościowych | wynik wrażliwy na progi i na parzystą liczbę kryteriów (remisy) |
| zgodna z dominacją Pareto | wszystkie kryteria traktowane równo (bez wag) lub sztucznie ważone |
| zasada „demokratyczna", łatwa do wyjaśnienia | podatna na manipulację doborem kryteriów |

## Podsumowanie

- Blin (z Whinstonem) – wyniki z teorii wyboru społecznego i decyzji grupowych: agregacja preferencji regułą większości, problem przechodniości.
- W wielokryterialnym porządkowaniu wariantów: kryteria „głosują"; wariant $r$ jest lepszy od $v$, gdy $l(r,v)>g(r,v)$.
- Zalety: prostota i niekonieczność wag; wady: możliwe cykle (paradoks Condorcet'a), ignorowanie wielkości różnic.
- **Pamiętaj: opis jest moją interpretacją; zweryfikuj z materiałami z zajęć.**

---
[⬅️ Poprzedni temat](4_Warianty_optymalne_w_sensie_Pareto.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../InternetRzeczy/InternetRzeczy_tytul.md)