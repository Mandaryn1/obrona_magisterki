# RISC (Reduced Instruction Set Computer)

## Rozwinięcie skrótu

**RISC** – *Reduced Instruction Set Computer*, czyli **komputer o zredukowanej (uproszczonej) liście rozkazów**. To architektura (ISA), w której lista instrukcji jest **mała, regularna i złożona z prostych rozkazów, z których każdy wykonuje się bardzo szybko** (w idealnym przypadku w jednym cyklu zegara).

## Idea i geneza

Początek lat 80. XX w.: badania (IBM 801 – J. Cocke; Berkeley RISC – D. Patterson; Stanford MIPS – J. Hennessy) pokazały, że:

- kompilatory używają głównie **prostych rozkazów** (przesłania, dodawanie, porównania, skoki), a złożone rozkazy CISC są rzadko wykorzystywane,
- uproszczenie procesora pozwala **podnieść częstotliwość zegara**, wprowadzić **potokowość** i przeznaczyć powierzchnię układu na **rejestry** i **cache** zamiast na mikrokod.

Złożoność przeniesiono ze sprzętu do **kompilatora**.

## Cechy architektury RISC

| Cecha | Opis |
| :--- | :--- |
| **Mała, prosta lista rozkazów** | kilkadziesiąt prostych instrukcji |
| **Stała długość rozkazu** | np. 32 bity (ARM A32, MIPS) – łatwe dekodowanie i pobieranie |
| **Architektura load/store** | tylko rozkazy `LOAD` i `STORE` mają dostęp do pamięci; operacje arytmetyczne wykonuje się **na rejestrach** |
| **Duża liczba rejestrów ogólnego przeznaczenia** | np. 16–32 (a nawet więcej), mniej odwołań do pamięci |
| **Jeden rozkaz = jeden cykl** (w założeniu) | prosty, przewidywalny czas wykonania |
| **Sterowanie sprzętowe** (hardwired) | bez mikrokodu → krótszy czas dekodowania |
| **Mało trybów adresowania** | najczęściej rejestr + przesunięcie |
| **Potokowość (pipelining)** | wiele rozkazów przetwarzanych jednocześnie w kolejnych etapach (pobranie, dekodowanie, wykonanie, dostęp do pamięci, zapis wyniku) |
| **Optymalizacje w kompilatorze** | kompilator planuje rozkazy i alokuje rejestry |

## Zalety i wady

| Zalety | Wady |
| :--- | :--- |
| prostszy, **tańszy i mniejszy rdzeń** | **dłuższy kod** (więcej rozkazów, większe zużycie pamięci programu) |
| **niski pobór mocy** (kluczowy w urządzeniach mobilnych i IoT) | większe wymagania wobec **kompilatora** |
| łatwe **potokowanie** i szybkie zegary, przewidywalna wydajność | większy ruch pamięci instrukcji (mitygowany cache'ami) |
| wydajna realizacja (większa liczba rozkazów na sekundę) | trudniejsze programowanie w asemblerze |
| dobra skalowalność (wiele rdzeni) | zgodność wsteczna z CISC wymaga emulacji/tłumaczenia |

## Przykłady architektur RISC

- **ARM** (Advanced RISC Machine; Cortex-M, Cortex-A) – dominuje w smartfonach i mikrokontrolerach IoT,
- **RISC-V** – **otwarta** architektura (Berkeley, 2010), rosnąca popularność (m.in. ESP32-C3, mikrokontrolery),
- **MIPS**, **PowerPC**, **SPARC**, Alpha,
- **AVR** (Arduino/ATmega) i **PIC** – 8-bitowe mikrokontrolery o cechach RISC.

## Porównanie CISC i RISC

| Cecha | CISC | RISC |
| :--- | :--- | :--- |
| Liczba rozkazów | duża, złożone | mała, proste |
| Długość rozkazu | zmienna | stała |
| Czas wykonania rozkazu | wiele cykli, zmienny | zwykle 1 cykl (potok) |
| Dostęp do pamięci | wiele rozkazów (pamięć–pamięć) | tylko LOAD/STORE |
| Rejestry | mało | dużo |
| Tryby adresowania | wiele | mało |
| Sterowanie | mikroprogramowane | sprzętowe |
| Potokowanie | trudne | łatwe |
| Rozmiar kodu | **mniejszy** | **większy** |
| Złożoność | w sprzęcie | w kompilatorze |
| Pobór mocy | zwykle większy | zwykle mniejszy |
| Przykłady | x86, 68k, VAX, 8051 | ARM, RISC-V, MIPS, AVR |

**Równanie wydajności**: $T=\dfrac{N_{instr}\cdot CPI}{f}$. CISC minimalizuje $N_{instr}$; RISC minimalizuje $CPI$ i umożliwia wzrost $f$ (większa liczba prostszych rozkazów wykonywana szybciej).

Współcześnie podział jest rozmyty: procesory x86 wewnętrznie tłumaczą rozkazy na mikrooperacje RISC-podobne, a ARM ma rozszerzenia o dość złożonych rozkazach (NEON, Thumb-2 ze zmienną długością).

## Podsumowanie

- RISC = **prosta, mała lista rozkazów** o stałej długości, architektura load/store, wiele rejestrów, potokowanie, sterowanie sprzętowe.
- Dąży do **jednego cyklu na rozkaz** i wysokiego taktowania; złożoność przeniesiona do kompilatora.
- Zalety: energooszczędność, prosty rdzeń, wydajność – stąd dominacja ARM w urządzeniach mobilnych i IoT.
- Wada: dłuższy kod.

---
[⬅️ Poprzedni temat](4_Architektura_CISC.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_UART_i_USRT.md)