# Jakie są główne zadania normalizacji wartości analizowanych kryteriów optymalizacji?

**Normalizacja** to przekształcenie wartości kryteriów na wspólną skalę. Kryteria mają różne jednostki i rzędy wielkości (cena w złotych, waga w kg, ocena 1–5), więc bez normalizacji nie można ich sensownie porównywać ani łączyć.

**Główne zadania normalizacji:**

- **Sprowadzenie do wspólnej, bezwymiarowej skali:** usuwa wpływ jednostek miary.
- **Ujednolicenie kierunku preferencji:** stymulanty (więcej = lepiej) i destymulanty (mniej = lepiej) przekształca się tak, by dla wszystkich kryteriów „większa wartość = lepiej".
- **Ograniczenie wartości do ustalonego przedziału**, najczęściej [0, 1]. Wartość znormalizowana wyraża wtedy stopień spełnienia kryterium.
- **Wyrównanie wpływu kryteriów:** kryterium o dużych liczbowo wartościach (np. cena w tysiącach) nie zdominuje kryterium o małych wartościach (np. waga w kg). Wagi mają wyrażać ważność kryterium, a nie jego skalę liczbową.
- **Umożliwienie stosowania wag i metod agregacji**, takich jak suma ważona czy TOPSIS.
- **Ujednolicenie typu danych:** kryteria jakościowe przekłada się na liczby.
- **Poprawa stabilności numerycznej** algorytmów optymalizacji.

Normalizacja **nie zmienia kolejności wariantów w obrębie jednego kryterium**, ale zmienia proporcje między kryteriami, więc może wpłynąć na końcowy ranking.

**Przykładowe metody:** min–max (unitaryzacja zerowana), ilorazowa względem maksimum, względem sumy (AHP), wektorowa (TOPSIS) i standaryzacja z-score. Dla stymulanty z=(x−min)/(max−min), a dla destymulanty z=(max−x)/(max−min).

## Podsumowanie

- Normalizacja przekształca kryteria na wspólną, bezwymiarową skalę (zwykle $[0,1]$) i ujednolica kierunek preferencji.
- Główne zadania: porównywalność kryteriów, eliminacja wpływu jednostek, wyrównanie wpływu kryteriów, możliwość agregacji z wagami, zamiana destymulant na stymulanty, ujednolicenie danych jakościowych i ilościowych.
- Metody: unitaryzacja zerowana (min–max), ilorazowa, względem sumy, wektorowa (TOPSIS), z-score, względem ideału/nadiru, funkcje użyteczności.
- Wybór metody może wpłynąć na ranking – warto zrobić analizę wrażliwości.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Metoda_leksykograficzna.md)