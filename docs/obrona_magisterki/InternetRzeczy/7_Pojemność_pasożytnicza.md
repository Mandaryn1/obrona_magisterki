# Czym jest i co powoduje pojemność pasożytnicza?

**Pojemność pasożytnicza** to **niezamierzona pojemność elektryczna** występująca w obwodzie między elementami, które nie miały tworzyć kondensatora. Powstaje wszędzie tam, gdzie dwa przewodniki, np. dwie ścieżki na płytce, przewody, piny złącza, wyprowadzenia układu albo obudowa i podłoże, są oddzielone izolatorem (powietrzem, laminatem). Taki układ zachowuje się jak mały kondensator, a jego wartość rośnie wraz z **powierzchnią przewodników**, a maleje wraz z ich **odległością**. Wynosi zwykle od pojedynczych do setek pikofaradów.

**Co powoduje:**

- **Ograniczenie szybkości sygnałów.** Pojemność trzeba naładować i rozładować przy każdej zmianie napięcia, więc zbocza się wydłużają (stała czasowa RC). Ogranicza to maksymalną częstotliwość pracy magistrali i linii danych.
- **Przesłuchy (crosstalk).** Sygnał z jednej ścieżki przenika do sąsiedniej, co powoduje zakłócenia.
- **Zniekształcenia sygnału i szum**, a w układach analogowych także przesunięcia fazowe i niestabilność.
- **Większy pobór mocy**, bo energia jest tracona na ładowanie i rozładowywanie pojemności przy każdym przełączeniu.
- **Ograniczenia w magistralach**, np. dla I²C maksymalna pojemność linii (ok. 400 pF) ogranicza długość przewodów i liczbę urządzeń. Powoduje też, że sygnały stają się zbyt wolne.
- **Wpływ na czujniki pojemnościowe**, np. ekrany dotykowe i przyciski dotykowe, gdzie pojemność pasożytnicza może zaburzać pomiar.

**Jak ją ograniczać:** krótkie ścieżki i przewody, większe odstępy między liniami, ekranowanie, odpowiedni projekt płytki (płaszczyzny masy), niższe rezystancje podciągające, bufory i wzmacniacze linii, mniejsze obudowy elementów.

## Podsumowanie

- Pojemność pasożytnicza to **niezamierzona pojemność** między dowolnymi przewodzącymi elementami układu rozdzielonymi dielektrykiem; zależy od powierzchni, odległości i materiału ($C=\varepsilon A/d$).
- Powstaje na PCB, w kablach, w tranzystorach (np. pojemność Millera), cewkach i złączach.
- Powoduje **opóźnienia (RC) i ograniczenie prędkości** (np. magistrali I²C), **przesłuchy**, zniekształcenia, **straty energii** ($CV^2f$), błędy czujników i niestabilność wzmacniaczy.
- Ograniczamy ją odpowiednim **projektem PCB** (krótkie ścieżki, odstępy, masa, ekranowanie), **małymi rezystancjami** w torze sygnału, terminacją i odsprzęganiem.

---
[⬅️ Poprzedni temat](6_UART_i_USRT.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Cechy_sensora_inteligentnego.md)