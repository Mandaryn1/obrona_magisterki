# CISC (Complex Instruction Set Computer)

## Rozwinięcie skrótu

**CISC** – *Complex Instruction Set Computer*, czyli **komputer o złożonej (rozbudowanej) liście rozkazów**. To architektura (ISA) procesora, w której lista instrukcji zawiera **dużo rozkazów, w tym złożonych**, wykonujących w jednym rozkazie wiele elementarnych operacji.

## Idea i geneza

Powstała w latach 60. i 70. XX w., gdy:

- pamięć była **droga i wolna** – krótkie programy (mało rozkazów) oszczędzały pamięć i liczbę odwołań do niej,
- **kompilatory były prymitywne** – programiści pisali w asemblerze, więc procesor miał realizować „wysokopoziomowe" operacje jednym rozkazem,
- dążono do zmniejszenia tzw. **luki semantycznej** między językami wysokiego poziomu a sprzętem.

Rozwiązanie: **jeden rozkaz = wiele elementarnych kroków**, np. „pobierz dwa argumenty z pamięci, dodaj, zapisz wynik w pamięci" albo kopiowanie całych bloków danych.

## Cechy architektury CISC

| Cecha | Opis |
| :--- | :--- |
| **Duża liczba rozkazów** | setki instrukcji, w tym specjalizowane (łańcuchowe, BCD, wielokrotne) |
| **Złożone rozkazy** | jeden rozkaz wykonuje operację wieloetapową (np. `MOVS`, `LOOP`, `MUL`, `DIV`) |
| **Rozkazy o zmiennej długości** | np. w x86 od 1 do 15 bajtów |
| **Wiele trybów adresowania** | bezpośrednie, pośrednie, indeksowe, bazowe, z przesunięciem itd. |
| **Operacje „pamięć–pamięć"** | rozkazy arytmetyczne mogą bezpośrednio używać operandów z pamięci |
| **Mało rejestrów ogólnego przeznaczenia** | historycznie kilka–kilkanaście, więcej odwołań do pamięci |
| **Mikroprogramowanie** | sterowanie rozkazami za pomocą mikrokodu (pamięć mikrorozkazów) |
| **Wiele cykli na rozkaz** | czas wykonania różny dla różnych rozkazów (kilka–kilkadziesiąt cykli) |
| **Trudniejsze potokowanie** | zmienna długość i czas wykonania komplikują potok |

## Zalety i wady

| Zalety | Wady |
| :--- | :--- |
| **zwarty kod** – krótszy program, mniej pamięci | złożona i **kosztowna realizacja sprzętowa** (duży rdzeń, większe zużycie energii) |
| wygodne programowanie w asemblerze (potężne rozkazy) | wiele rzadko używanych rozkazów (niewielka część listy odpowiada za większość wykonywanego kodu) |
| mniej odwołań do pamięci instrukcji | **zmienny czas wykonania** utrudnia potokowanie i szybkie zegary |
| zgodność wsteczna (stare oprogramowanie nadal działa – x86) | trudne zrównoleglenie na poziomie instrukcji, wolniejsza ewolucja |
| mniejszy wysiłek kompilatora (rozkazy bliskie konstrukcjom języków) | mikrokod wolniejszy od logiki „zadrutowanej" |

## Przykłady procesorów CISC

- **Intel x86 / x86-64** (8086 … Core, AMD Ryzen) – najważniejszy współcześnie przykład,
- Motorola 68000 (68k), IBM System/360, DEC VAX,
- Intel **8051** (mikrokontroler; klasyczna lista rozkazów CISC), Zilog Z80, MOS 6502.

## CISC dzisiaj

Współczesne procesory x86 są **hybrydami**: zewnętrznie przyjmują rozkazy CISC, ale wewnątrz **dekodują je na proste mikrooperacje (µops)** wykonywane przez rdzeń przypominający RISC (potok, superskalarność, wykonywanie poza kolejnością). Dzięki temu zachowują zgodność z ogromnym zasobem oprogramowania i jednocześnie osiągają wysoką wydajność.

## Równanie wydajności

$$T_{prog}=N_{instr}\cdot CPI\cdot T_{zegara}=\frac{N_{instr}\cdot CPI}{f}$$

CISC dąży do **zmniejszenia liczby instrukcji $N_{instr}$** (złożone rozkazy), kosztem **większego $CPI$** (średnia liczba cykli na rozkaz). RISC postępuje odwrotnie (zob. temat 5).

## Podsumowanie

- CISC = architektura z **rozbudowaną listą złożonych rozkazów** o zmiennej długości, wieloma trybami adresowania i operacjami na pamięci.
- Cel historyczny: krótkie programy i bliskość do języków wysokiego poziomu.
- Wady: skomplikowany sprzęt, zmienny czas rozkazów, trudne potokowanie.
- Przykład: x86; współcześnie realizowane z wewnętrznym rdzeniem RISC-podobnym.

---
[⬅️ Poprzedni temat](3_Mikrokontroler_i_mikroprocesor.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Architektura_RISC.md)