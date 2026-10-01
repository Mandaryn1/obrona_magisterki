# Mikrokontroler i mikroprocesor – charakterystyka i różnice

## Mikroprocesor (MPU, µP)

**Mikroprocesor** to **układ scalony zawierający jednostkę centralną (CPU)** – jednostkę arytmetyczno-logiczną (ALU), rejestry i układy sterujące – który **wykonuje program i przetwarza dane**, ale **sam nie jest kompletnym systemem**. Do pracy potrzebuje układów zewnętrznych:

- **pamięci** operacyjnej (RAM) i programu/danych (ROM, Flash, dysk),
- **układów wejścia/wyjścia** (kontrolery, porty),
- zegara, układów zasilania i resetu,
- zewnętrznych **magistral** (adresowej, danych, sterującej), którymi komunikuje się z resztą.

Cechy: duża moc obliczeniowa i wysokie taktowanie (setki MHz–GHz), architektura 32/64-bitowa, pamięć podręczna (cache), jednostka zarządzania pamięcią (**MMU**) i **system operacyjny ogólnego przeznaczenia** (Linux, Windows). Zastosowania: komputery PC, serwery, smartfony, komputery jednopłytkowe.

Przykłady: Intel Core, AMD Ryzen, **ARM Cortex-A** (w SoC Raspberry Pi), Motorola 68000, Zilog Z80, Intel 8086.

## Mikrokontroler (MCU, µC)

**Mikrokontroler** to **kompletny „komputer w jednym układzie scalonym"**, przeznaczony do **sterowania** urządzeniami i procesami. Na jednym chipie zawiera:

- **rdzeń CPU** (zwykle prostszy niż w mikroprocesorze),
- **pamięć programu** (Flash, ROM) i **pamięć danych** (SRAM; często także EEPROM),
- **układy peryferyjne**: porty **GPIO**, **timery/liczniki**, generatory **PWM**, przetworniki **ADC** (czasem DAC), interfejsy komunikacyjne **UART, SPI, I²C, CAN, USB**, kontroler przerwań, watchdog, zegar (oscylator), często komparatory i czujnik temperatury; w wersjach IoT także **radio Wi-Fi/BLE**.

Cechy: **niski koszt**, **mały pobór mocy** (tryby uśpienia, µA), niewielki rozmiar, praca w **czasie rzeczywistym** (deterministyczne reakcje na zdarzenia), taktowanie zwykle od kilkuset kHz do kilkuset MHz, niewielkie zasoby pamięci (kB–MB), program zwykle w **firmware** bez systemu operacyjnego lub z **RTOS**.

Przykłady: **ATmega328P** (Arduino Uno; 8-bit AVR, 16 MHz, 32 kB Flash, 2 kB SRAM), **STM32** (ARM Cortex-M), **ESP32** (dwurdzeniowy Xtensa, Wi-Fi + BLE), PIC, MSP430, Intel 8051, Raspberry Pi **Pico** (RP2040).

## Różnice

| Cecha | Mikroprocesor | Mikrokontroler |
| :--- | :--- | :--- |
| **Zawartość układu** | tylko CPU (+ cache, MMU) | CPU + pamięć (Flash, RAM) + peryferia (GPIO, ADC, UART, timery…) |
| **Pamięć** | zewnętrzna (RAM, dysk) | wbudowana, ograniczona (kB–MB) |
| **Układy I/O** | zewnętrzne kontrolery | zintegrowane w chipie |
| **Taktowanie** | setki MHz – GHz | kHz – setki MHz |
| **Moc obliczeniowa** | wysoka (wielordzeniowe, 64-bit) | umiarkowana (8/16/32-bit) |
| **System operacyjny** | ogólnego przeznaczenia (Linux, Windows) | brak (bare-metal) lub RTOS |
| **Pobór mocy** | duży (W–setki W) | mały (mW, µA w uśpieniu) |
| **Koszt systemu** | wyższy (trzeba dodać pamięć i peryferia) | niski (pojedynczy układ) |
| **Czas rzeczywisty** | trudniejszy do zagwarantowania | typowo deterministyczny |
| **Zastosowania** | komputery, serwery, telefony, zaawansowane SBC | sterowanie, urządzenia wbudowane, czujniki, AGD, motoryzacja, IoT |

Na poziomie architektury mikrokontrolery często mają **architekturę Harvarda** (osobna pamięć i magistrale programu oraz danych, np. AVR, PIC), a mikroprocesory ogólnego przeznaczenia — Von Neumanna (z podziałem cache na instrukcje i dane, tzw. zmodyfikowana Harvarda).

## Pojęcia pokrewne

- **SoC (System on Chip)** – jeszcze wyższy poziom integracji: CPU(+GPU), pamięć, peryferia, często modem radiowy w jednym układzie (np. ESP32, układy w smartfonach, Raspberry Pi).
- **DSP** – procesor sygnałowy zoptymalizowany pod obliczenia (mnożenie z akumulacją).
- **FPGA** – układ programowalny; **SBC** (komputer jednopłytkowy) – płytka z mikroprocesorem (Raspberry Pi, BeagleBone), działa pod Linuxem.
- Rodziny ARM: **Cortex-M** (mikrokontrolery), **Cortex-R** (czas rzeczywisty), **Cortex-A** (aplikacyjne, mikroprocesory).

## Dlaczego w IoT dominują mikrokontrolery

- niski koszt i pobór energii (zasilanie bateryjne przez lata),
- wszystko w jednym układzie, mały rozmiar,
- wbudowane interfejsy do czujników i aktuatorów,
- deterministyczna praca w czasie rzeczywistym,
- układy z radiem (ESP32, nRF52, STM32WB) bezpośrednio łączą się z siecią.

Mikroprocesor/SBC wybiera się, gdy potrzebny jest system operacyjny, złożona analityka, obsługa grafiki, brama IoT lub edge computing (np. rozpoznawanie obrazu).

## Podsumowanie

- **Mikroprocesor** = sama jednostka obliczeniowa; wymaga zewnętrznej pamięci i układów I/O; wysoka moc, system operacyjny, komputery.
- **Mikrokontroler** = CPU + pamięć + peryferia w jednym układzie; tani, energooszczędny, do sterowania w czasie rzeczywistym, urządzenia wbudowane i IoT.
- Granica się zaciera: SoC integrują coraz więcej, a mikrokontrolery zyskują rdzenie 32-bitowe i radio.
