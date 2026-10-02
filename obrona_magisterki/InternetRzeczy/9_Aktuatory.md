# Czym są i do czego służą aktuatory?

**Aktuator (element wykonawczy)** to urządzenie, które **zamienia sygnał sterujący (zwykle elektryczny) na działanie fizyczne**, np. ruch, siłę, ciepło, światło lub dźwięk. Jest przeciwieństwem czujnika: sensor mierzy otoczenie, a aktuator na nie **oddziałuje**. Aktuator wymaga zasilania, a często także układu sterującego (sterownika, drivera lub przekaźnika).

**Do czego służą:** wykonują polecenia wydane przez mikrokontroler lub system sterowania. W IoT zamykają pętlę: czujnik mierzy, system analizuje, aktuator działa. Dzięki temu możliwa jest automatyzacja i zdalne sterowanie.

**Rodzaje (według zasady działania):**

- **Elektryczne:** silniki prądu stałego, krokowe i serwomechanizmy, przekaźniki, elektrozawory, elektromagnesy, grzałki.
- **Pneumatyczne i hydrauliczne:** siłowniki napędzane sprężonym powietrzem lub cieczą (duże siły, przemysł).
- **Piezoelektryczne:** precyzyjne, bardzo małe przemieszczenia (drukarki atramentowe, mikropozycjonery).
- **Termiczne i inne:** elementy grzejne, pamięć kształtu.
- **Optyczne i akustyczne:** diody LED, wyświetlacze, buzzery, głośniki (sygnalizacja).

**Przykłady zastosowań:** otwieranie zaworu nawadniania, włączanie oświetlenia, zamek elektromagnetyczny, regulacja temperatury grzejnika, ruch ramienia robota, sterowanie klapą wentylacyjną.

**Sterowanie:** mikrokontroler nie zasili aktuatora bezpośrednio z pinu (za mały prąd), więc używa **tranzystorów, przekaźników lub mostków H**. Prędkością lub mocą steruje się najczęściej sygnałem **PWM**.

## Podsumowanie

- Aktuator zamienia **sygnał sterujący i energię** na **działanie fizyczne** (ruch, siła, ciepło, światło, dźwięk) – wykonuje decyzje układu sterowania.
- Klasyfikacje: według energii (elektryczne, pneumatyczne, hydrauliczne, piezo, termiczne), rodzaju ruchu (obrotowe, liniowe, dwustanowe, ciągłe).
- W IoT najczęściej: silniki DC/BLDC/krokowe, serwa, przekaźniki, elektrozawory, LED, buzzery – sterowane przez **drivery** (MOSFET, mostek H) i **PWM**.
- Wymagają dopasowania mocy, ochrony (dioda zwrotna, separacja) i często sprzężenia zwrotnego (PID).

---
[⬅️ Poprzedni temat](8_Cechy_sensora_inteligentnego.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Modulacja_szerokości_impulsów_PWM.md)