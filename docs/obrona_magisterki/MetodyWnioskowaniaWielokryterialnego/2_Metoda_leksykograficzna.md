# Na czym polega metoda leksykograficzna?

**Metoda leksykograficzna** to metoda wielokryterialna, w której kryteria **porządkuje się według ważności**, od najważniejszego do najmniej ważnego, i rozpatruje **po kolei**. Działa jak układanie słów w słowniku: o kolejności decyduje pierwsza litera, a druga dopiero przy remisie.

**Przebieg:**

- Decydent ustala **hierarchię kryteriów**: K₁ jest ważniejsze od K₂, K₂ od K₃ itd.
- Rozwiązuje się zadanie optymalizacji **tylko dla K₁** i wybiera warianty najlepsze według tego kryterium.
- Jeśli jest **jeden** najlepszy wariant, to on jest rozwiązaniem i na tym koniec.
- Jeśli jest **remis** (kilka wariantów równorzędnych), spośród nich wybiera się najlepsze według **K₂**, potem w razie remisu według K₃ itd., aż do wyłonienia jednego wariantu lub wyczerpania kryteriów.

**Własności:**

- Kryterium ważniejsze **całkowicie dominuje** nad mniej ważnymi. Nawet znikomo lepsza wartość K₁ wygrywa z ogromną przewagą na K₂. Nie ma kompensacji i nie stosuje się wag liczbowych, tylko **kolejność ważności**.
- Wynik jest **optymalny w sensie Pareto**.
- Metoda jest prosta, a decydent podaje tylko porządek kryteriów.

**Wady:** sztywność (remisy na pierwszym kryterium zdarzają się rzadko, więc kolejne kryteria często w ogóle nie są brane pod uwagę) oraz brak możliwości wyrównywania wad jednego kryterium zaletami innego. Dlatego stosuje się czasem **wersję z progami tolerancji**: warianty zbliżone do najlepszego w granicach ustalonej tolerancji też przechodzą do następnego kroku.

**Przykład:** wybór samochodu. Najważniejsza jest cena, potem spalanie, potem pojemność bagażnika. Najpierw wybieramy najtańsze samochody, a dopiero gdy kilka kosztuje tyle samo, porównujemy ich spalanie.

## Podsumowanie

- Kryteria porządkuje się od najważniejszego; kolejno **maksymalizuje się** każde z nich na zbiorze rozwiązań optymalnych dla poprzednich.
- Odpowiada porządkowi słownikowemu; brak kompensacji i wag.
- Wersja z tolerancją (współczynniki odstępstwa $d_k$) łagodzi surowość metody.
- Rozwiązanie jest Pareto-optymalne, ale wynik zależy wyłącznie od kolejności i bardzo wrażliwy na drobne różnice na kryteriach najważniejszych.

---
[⬅️ Poprzedni temat](1_Główne_zadania_normalizacji_wartości_kryteriów_optymalizacji.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](3_Wyznaczanie_wag_ważności_kryteriów_w_metodzie_AHP.md)