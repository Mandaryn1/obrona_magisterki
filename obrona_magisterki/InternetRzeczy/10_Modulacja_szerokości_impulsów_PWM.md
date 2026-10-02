# Opisz zasadę działania modulacji szerokości impulsów PWM (ang. pulse width modulation).

**PWM (Pulse Width Modulation)** to technika sterowania mocą, w której sygnał cyfrowy przełącza się **bardzo szybko między stanem wysokim (włączony) i niskim (wyłączony)**, a wartość średnia zależy od tego, jak długo sygnał jest w stanie wysokim.

**Zasada działania:**

- Sygnał ma stałą **częstotliwość (okres T)**.
- Zmienia się **szerokość impulsu**, czyli czas t_on, przez który sygnał jest włączony w obrębie okresu.
- Podstawowym parametrem jest **współczynnik wypełnienia (duty cycle)**: D = t_on / T · 100%.
- **Średnie napięcie** wynosi U_śr = D · U_max. Na przykład przy zasilaniu 5 V i wypełnieniu 50% średnia wynosi 2,5 V, a przy 25% 1,25 V.

Odbiornik (silnik, dioda LED, grzałka) ze względu na swoją **bezwładność** albo bezwładność ludzkiego oka uśrednia szybkie przełączanie. Dzięki temu efekt jest taki, jakby podawać napięcie o zmiennej wartości, choć układ jest tylko włączany i wyłączany. Do odzyskania napięcia analogowego można użyć prostego **filtru dolnoprzepustowego RC**.

**Zalety:**

- **Wysoka sprawność.** Element sterujący (tranzystor) pracuje w stanie pełnego włączenia lub wyłączenia, więc ma małe straty mocy, w odróżnieniu od regulacji analogowej.
- Prosta realizacja cyfrowa w mikrokontrolerze (wbudowane timery i generatory PWM) i **brak potrzeby przetwornika DAC**.

**Zastosowania:** regulacja jasności diod LED, **prędkości silników DC**, sterowanie serwomechanizmami (położenie zależy od szerokości impulsu, zwykle 1–2 ms w okresie 20 ms), zasilacze impulsowe i przetwornice, regulacja mocy grzałek, generowanie dźwięku.

**Uwaga:** częstotliwość PWM musi być dobrana do zastosowania. Dla LED i silników zwykle jest to kilkaset Hz do kilkadziesiąt kHz, aby uniknąć migotania lub słyszalnego pisku.

## Podsumowanie

- PWM = przebieg prostokątny o **stałym okresie** i **zmiennym współczynniku wypełnienia** $D=t_{on}/T$; **średnia wartość** sygnału $V_{sr}=D\cdot V_{max}$.
- Generowany sprzętowo przez timer i komparator; parametry: częstotliwość $f=f_{clk}/(N(TOP+1))$, rozdzielczość $n$ bitów.
- Pozwala w sposób **energooszczędny** regulować moc: jasność LED, prędkość silników, temperaturę, położenie serwa (impuls 1–2 ms co 20 ms), a także realizować „DAC" z filtrem RC i zasilacze impulsowe.
- Wady: zakłócenia EMI, tętnienia, kompromis rozdzielczość–częstotliwość.

---
[⬅️ Poprzedni temat](9_Aktuatory.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../BarieryWPrzestrzeniCyfrowej/BarieryWPrzestrzeniCyfrowej_tytul.md)