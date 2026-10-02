# Wyjaśnij, co oznacza skrót RISC. Krótko opisz to pojęcie.

## Rozwinięcie skrótu

**RISC** – *Reduced Instruction Set Computer*, czyli **komputer o zredukowanej (uproszczonej) liście rozkazów**. To architektura (ISA), w której lista instrukcji jest **mała, regularna i złożona z prostych rozkazów, z których każdy wykonuje się bardzo szybko** (w idealnym przypadku w jednym cyklu zegara).

**Cechy:**

- **Mało prostych instrukcji o stałej długości**, wykonywanych zwykle w **jednym cyklu zegara**.
- Architektura **load/store**: tylko instrukcje ładowania i zapisu odwołują się do pamięci, a obliczenia wykonywane są na **rejestrach**. Dlatego RISC ma **dużo rejestrów ogólnego przeznaczenia**.
- **Prosta jednostka sterująca**, zwykle sprzętowa (bez mikroprogramu), więc układ jest mniejszy, tańszy i pobiera mniej energii.
- Prostota instrukcji ułatwia **potokowanie (pipelining)** i wykonywanie wielu instrukcji równolegle, co zwiększa wydajność.
- **Dłuższy kod**, bo złożone operacje trzeba złożyć z kilku prostych instrukcji. Większe wymagania stawia się kompilatorowi, który optymalizuje kod.

**Przykłady:** **ARM** (smartfony, mikrokontrolery STM32, Raspberry Pi), **RISC-V**, AVR, MIPS, PowerPC.

**Zastosowanie:** dzięki niskiemu poborowi mocy RISC dominuje w urządzeniach mobilnych, systemach wbudowanych i IoT.

**Różnica względem CISC:** RISC stawia na prostotę i szybkość pojedynczych instrukcji, a CISC na złożone instrukcje i zwarty kod.

## Podsumowanie

- RISC = **prosta, mała lista rozkazów** o stałej długości, architektura load/store, wiele rejestrów, potokowanie, sterowanie sprzętowe.
- Dąży do **jednego cyklu na rozkaz** i wysokiego taktowania; złożoność przeniesiona do kompilatora.
- Zalety: energooszczędność, prosty rdzeń, wydajność – stąd dominacja ARM w urządzeniach mobilnych i IoT.
- Wada: dłuższy kod.

---
[⬅️ Poprzedni temat](4_Architektura_CISC.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_UART_i_USRT.md)