# Opisz warianty należące do zbioru wariantów optymalnych w sensie Pareto.

**Wariant optymalny w sensie Pareto** (niezdominowany, efektywny) to taki wariant, którego **nie da się poprawić pod względem żadnego kryterium bez pogorszenia go pod względem przynajmniej jednego innego kryterium**.

**Pojęcie dominacji:** wariant A **dominuje** wariant B, jeśli jest co najmniej tak samo dobry jak B według wszystkich kryteriów i **ściśle lepszy według co najmniej jednego**. Zbiór wariantów Pareto-optymalnych (**front Pareto**) tworzą wszystkie warianty, które **nie są zdominowane przez żaden inny wariant**.

**Cechy zbioru:**

- Warianty zdominowane można **odrzucić** bez straty, bo istnieje wariant co najmniej tak dobry i w czymś lepszy.
- Warianty w zbiorze Pareto są **niepodporządkowane**: poprawa jednego kryterium oznacza kompromis (pogorszenie innego), więc nie da się ich uporządkować bez preferencji decydenta.
- Zbiór **zawiera wszystkie rozsądne rozwiązania kompromisowe**. Rozwiązanie wybrane metodą wielokryterialną (np. suma ważona, metoda leksykograficzna) leży w tym zbiorze, a ostateczny wybór zależy od preferencji decydenta (wag, kolejności kryteriów).
- W zadaniach z dwoma kryteriami zbiór ten tworzy **krzywą (front)**, a z trzema i więcej powierzchnię.

**Rodzaje:**

- **Silnie optymalne w sensie Pareto** (efektywne): nie istnieje wariant, który byłby co najmniej tak dobry we wszystkich kryteriach i lepszy w jednym.
- **Słabo optymalne w sensie Pareto:** nie istnieje wariant ściśle lepszy we **wszystkich** kryteriach. Jest to szerszy zbiór, który zawiera zbiór silnie optymalny.

**Przykład:** dla samochodów porównywanych według ceny (mniej = lepiej) i mocy (więcej = lepiej) wariant 100 tys. zł i 150 KM jest zdominowany przez 90 tys. zł i 160 KM. Natomiast 90 tys. zł i 160 KM oraz 120 tys. zł i 200 KM należą do zbioru Pareto, bo żaden nie jest lepszy w obu kryteriach naraz.

## Podsumowanie

- Wariant jest **Pareto-optymalny**, gdy żaden inny wariant go nie dominuje: nie da się poprawić jednego kryterium bez pogorszenia innego.
- Dominacja: nie gorszy na wszystkich kryteriach i lepszy na co najmniej jednym.
- Warianty Pareto są wzajemnie nieporównywalne; reprezentują **kompromisy** między kryteriami. Zbiór może zawierać jeden wariant (kryteria zgodne) albo wszystkie (kryteria przeciwstawne).
- Pierwszy krok analizy wielokryterialnej: **odrzucić warianty zdominowane**, potem wybrać jeden z pozostałych według preferencji decydenta.

---
[⬅️ Poprzedni temat](3_Wyznaczanie_wag_ważności_kryteriów_w_metodzie_AHP.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Metoda_Blina.md)