# Wyjaśnij, co oznacza skrót CISC. Krótko opisz ten termin.

## Rozwinięcie skrótu

**CISC** – *Complex Instruction Set Computer*, czyli **komputer o złożonej (rozbudowanej) liście rozkazów**. To architektura (ISA) procesora, w której lista instrukcji zawiera **dużo rozkazów, w tym złożonych**, wykonujących w jednym rozkazie wiele elementarnych operacji.

**Cechy:**

- **Dużo instrukcji** (setki), o zmiennej długości i różnym czasie wykonania (wiele cykli zegara).
- **Wiele trybów adresowania** i możliwość operowania bezpośrednio na pamięci.
- **Zwarty kod programu**, bo jedna instrukcja zastępuje kilka prostszych. Oszczędza to pamięć i upraszcza pisanie kompilatorów oraz programów w asemblerze.
- **Złożona jednostka sterująca**, często zrealizowana przez **mikroprogram**, co wymaga więcej tranzystorów i zwiększa pobór energii.
- Trudniejsze jest **potokowanie** (pipelining) z powodu różnej długości i czasu instrukcji.

**Przykłady:** rodzina **x86 i x86-64** (Intel, AMD), Motorola 68000, a także część mikrokontrolerów (np. 8051).

**Uwaga:** współczesne procesory x86 są wewnętrznie podobne do RISC. Złożone instrukcje są **rozkładane na proste mikrooperacje**, które wykonuje szybki rdzeń.

**Przeciwieństwo** to architektura **RISC**: mało prostych instrukcji o stałej długości, wykonywanych zwykle w jednym cyklu.

## Podsumowanie

- CISC = architektura z **rozbudowaną listą złożonych rozkazów** o zmiennej długości, wieloma trybami adresowania i operacjami na pamięci.
- Cel historyczny: krótkie programy i bliskość do języków wysokiego poziomu.
- Wady: skomplikowany sprzęt, zmienny czas rozkazów, trudne potokowanie.
- Przykład: x86; współcześnie realizowane z wewnętrznym rdzeniem RISC-podobnym.

---
[⬅️ Poprzedni temat](3_Mikrokontroler_i_mikroprocesor.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Architektura_RISC.md)