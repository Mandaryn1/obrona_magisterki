# Scharakteryzuj metodę Blina.

**Metoda Blina:** J. M. Blin (wspólnie z Whinstonem) pisał na początku lat 70. o agregowaniu preferencji **regułą większości**. W wielokryterialnym porządkowaniu wariantów rolę „głosujących" pełnią **kryteria**.

**Idea:** warianty porównujemy parami. Wariant r jest lepszy od v, jeśli jest **lepszy według większej liczby kryteriów, niż jest gorszy**. Dla każdej pary zliczamy: l(r, v), czyli liczbę kryteriów, w których r wygrywa, oraz g(r, v), czyli liczbę kryteriów, w których przegrywa. Wtedy r ≻ v, gdy l(r, v) > g(r, v). Opcjonalnie dodaje się progi nierozróżnialności lub wagi kryteriów. Najlepsze są warianty, których nie pokonuje żaden inny, albo wybiera się je według wyniku Copelanda (liczba pokonanych minus liczba tych, przez które został pokonany).

**Własności:**

- **Nie wymaga wag ani normalizacji**, wystarczy skala porządkowa, więc nadaje się do kryteriów jakościowych.
- **Jest niekompensacyjna:** liczy się tylko to, na ilu kryteriach wariant wygrywa, a nie o ile.
- **Jest zgodna z dominacją Pareto**, więc wariant zdominowany zawsze zostaje pokonany.

**Główna wada:** reguła większości **nie gwarantuje przechodniości**. Mogą powstać cykle (X ≻ Y ≻ Z ≻ X, paradoks Condorcet'a), a wtedy nie ma jednoznacznego rankingu. Ogranicza się to wynikiem Copelanda, wagami albo progami większości (jak w metodach ELECTRE).

Jeśli masz slajdy lub notatki z tego tematu, wyślij je. Wtedy dopasuję opracowanie i odpowiedź do tego, czego uczono na zajęciach.

## Podsumowanie

- Blin (z Whinstonem) – wyniki z teorii wyboru społecznego i decyzji grupowych: agregacja preferencji regułą większości, problem przechodniości.
- W wielokryterialnym porządkowaniu wariantów: kryteria „głosują"; wariant $r$ jest lepszy od $v$, gdy $l(r,v)>g(r,v)$.
- Zalety: prostota i niekonieczność wag; wady: możliwe cykle (paradoks Condorcet'a), ignorowanie wielkości różnic.
- **Pamiętaj: opis jest moją interpretacją; zweryfikuj z materiałami z zajęć.**

---
[⬅️ Poprzedni temat](4_Warianty_optymalne_w_sensie_Pareto.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../InternetRzeczy/InternetRzeczy_tytul.md)