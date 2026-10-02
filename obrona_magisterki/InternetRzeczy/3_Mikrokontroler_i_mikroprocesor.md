# Scharakteryzuj pojęcie mikrokontrolera i mikroprocesora. Podaj różnice między tymi pojęciami.

**Mikroprocesor (CPU)** to układ scalony zawierający **tylko jednostkę centralną**: jednostkę arytmetyczno-logiczną (ALU), rejestry i układ sterowania. Sam nie tworzy kompletnego systemu. Do pracy potrzebuje **zewnętrznej pamięci (RAM, ROM/Flash) i układów wejścia/wyjścia**, połączonych magistralami. Jest nastawiony na **wysoką moc obliczeniową** i uniwersalność. Przykłady: Intel Core, AMD Ryzen, procesory ARM Cortex-A w telefonach i komputerach.

**Mikrokontroler (MCU)** to **kompletny mały komputer w jednym układzie**. Zawiera rdzeń procesora, **pamięć programu (Flash), pamięć danych (RAM)** oraz **peryferia**: porty GPIO, liczniki i timery, przetworniki ADC/DAC, generator PWM, interfejsy UART, SPI, I²C, CAN. Jest przeznaczony do **konkretnych zadań sterujących** w systemach wbudowanych. Przykłady: AVR (ATmega328 w Arduino), STM32, ESP32, PIC, RP2040.

**Różnice:**

- **Budowa:** mikroprocesor to sama jednostka obliczeniowa, mikrokontroler ma w jednym układzie procesor, pamięć i peryferia.
- **Wydajność:** mikroprocesor jest znacznie szybszy (GHz, wiele rdzeni), mikrokontroler wolniejszy (kilka do kilkuset MHz).
- **Pamięć:** mikroprocesor używa zewnętrznych dużych pamięci (GB), mikrokontroler ma małe pamięci wbudowane (kB–MB).
- **Zastosowanie:** mikroprocesor to komputery i smartfony z systemem operacyjnym, mikrokontroler to układy sterowania (pralki, czujniki, urządzenia IoT), często bez systemu operacyjnego lub z systemem czasu rzeczywistego (RTOS).
- **Pobór mocy i koszt:** mikrokontroler jest tańszy, mniejszy i bardziej energooszczędny. Mikroprocesor pobiera więcej mocy i wymaga więcej elementów zewnętrznych.
- **Elastyczność:** mikroprocesor nadaje się do uniwersalnych zadań, mikrokontroler do wyspecjalizowanych.

W IoT dominują mikrokontrolery, bo są tanie, energooszczędne i mają wszystko potrzebne do obsługi czujników i komunikacji.

## Podsumowanie

- **Mikroprocesor** = sama jednostka obliczeniowa; wymaga zewnętrznej pamięci i układów I/O; wysoka moc, system operacyjny, komputery.
- **Mikrokontroler** = CPU + pamięć + peryferia w jednym układzie; tani, energooszczędny, do sterowania w czasie rzeczywistym, urządzenia wbudowane i IoT.
- Granica się zaciera: SoC integrują coraz więcej, a mikrokontrolery zyskują rdzenie 32-bitowe i radio.

---
[⬅️ Poprzedni temat](2_Magistrala_interfejs_protokół.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Architektura_CISC.md)