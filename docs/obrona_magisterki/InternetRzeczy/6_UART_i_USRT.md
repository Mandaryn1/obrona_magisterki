# Opisz i podaj różnice pomiędzy UART a USRT.

**UART (Universal Asynchronous Receiver/Transmitter)** to układ, który realizuje **asynchroniczną transmisję szeregową**: dane przesyłane są bit po bicie, **bez wspólnego sygnału zegarowego**. Obie strony muszą mieć z góry ustawioną tę samą **prędkość transmisji (baud rate)**, np. 9600 czy 115200 bit/s. Do połączenia wystarczą linie **TX, RX i masa** (opcjonalnie RTS/CTS do sterowania przepływem).

Każdy znak jest wysyłany w ramce: **bit startu**, 5–9 **bitów danych**, opcjonalny **bit parzystości** do wykrywania błędów i 1–2 **bity stopu**. Bit startu synchronizuje odbiornik na czas jednej ramki. Zegary obu stron mogą się nieznacznie różnić, ale tolerancja jest niewielka (ok. kilka procent).

**USRT (Universal Synchronous Receiver/Transmitter)** to układ realizujący **synchroniczną transmisję szeregową**. Oprócz linii danych występuje **osobna linia zegarowa (clock)**, którą generuje jedna ze stron (master). Odbiornik próbkuje dane zgodnie z tym zegarem, więc **nie są potrzebne bity startu i stopu**. Dane wysyła się w blokach (ramkach) poprzedzonych znakami synchronizacji.

**Różnice:**

- **Zegar:** UART nie ma linii zegara i synchronizuje się bitami startu i stopu, USRT ma wspólny zegar na osobnej linii.
- **Liczba przewodów:** UART wymaga mniej (TX, RX, GND), USRT jest o jeden przewód zegarowy droższy.
- **Narzut:** w UART każdy znak ma bity startu, stopu i parzystości, więc część przepustowości się marnuje. USRT ma mały narzut, więc jest **szybszy i wydajniejszy** przy dużych blokach danych.
- **Wymagania czasowe:** UART wymaga zgodności prędkości obu stron, USRT jest odporniejszy na różnice zegarów, bo zegar jest dostarczany.
- **Zastosowanie:** UART to proste łącza (konsola, GPS, moduły Bluetooth i GSM, RS-232/RS-485), USRT to szybsza komunikacja blokowa.

W praktyce częściej spotyka się **USART** (Universal Synchronous/Asynchronous Receiver/Transmitter), czyli układ obsługujący **oba tryby**, np. w mikrokontrolerach AVR i STM32.

## Podsumowanie

- **UART** – transmisja szeregowa **asynchroniczna**: bez linii zegara, znak oprawiony bitem START i STOP (opcjonalnie parzystość), z uzgodnioną prędkością (baud rate); prosty, ale z narzutem; popularny: 8N1.
- **USRT** – transmisja **synchroniczna**: zegar wspólny/odtwarzany, dane w blokach z synchronizacją SYNC/flagami, mały narzut i duża przepływność, większa złożoność.
- **USART** obsługuje oba tryby (w praktyce najczęściej spotykany moduł w mikrokontrolerach).

---
[⬅️ Poprzedni temat](5_Architektura_RISC.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Pojemność_pasożytnicza.md)