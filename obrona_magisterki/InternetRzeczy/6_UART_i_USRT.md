# UART i USRT – opis i różnice

## Transmisja szeregowa

W **transmisji szeregowej** bity informacji są wysyłane **jeden po drugim** jedną linią (w przeciwieństwie do równoległej, gdzie wiele bitów idzie jednocześnie wieloma liniami). Wymaga mniej przewodów, lepiej nadaje się na większe odległości. Odbiorca musi wiedzieć, **kiedy próbkować** kolejne bity – sposób rozwiązania tego problemu odróżnia transmisję **asynchroniczną** od **synchronicznej**.

## UART (Universal Asynchronous Receiver/Transmitter)

**UART** to układ (moduł peryferyjny) realizujący **asynchroniczną** transmisję szeregową: zamienia dane **równoległe** (bajty z procesora) na strumień **szeregowy** przy nadawaniu oraz odwrotnie przy odbiorze.

### Zasada działania

- **Brak wspólnej linii zegara** – nadajnik i odbiornik mają **własne zegary** o uzgodnionej **szybkości transmisji (baud rate)**, np. 9600, 19200, 115200 bit/s. Odbiornik synchronizuje się **na początku każdego znaku** (zbocze bitu START) i próbkuje bity w ich środku (zwykle z **nadpróbkowaniem 16×**).
- Dane wysyłane są w **ramkach znakowych** (zazwyczaj bajt po bajcie), oddzielonych przerwami (linia w spoczynku jest w stanie wysokim).

### Format ramki

```
 linia:   spoczynek | START | D0 D1 D2 D3 D4 D5 D6 D7 | [P] | STOP | spoczynek
 poziom:    1 (wys.) |   0   |   dane, LSB pierwszy     |  –  |  1   |  1 (wys.)
```

| Element | Opis |
| :--- | :--- |
| **Bit startu** | zawsze 1 bit, stan niski (0) – sygnalizuje początek znaku i synchronizuje odbiornik |
| **Bity danych** | 5–9 (najczęściej **8**), **najmłodszy bit (LSB) pierwszy** |
| **Bit parzystości** (opcjonalny) | even (parzystość), odd (nieparzystość), none, mark/space – prosta kontrola błędów |
| **Bit(y) stopu** | 1, 1,5 lub 2 bity, stan wysoki (1) – koniec znaku, zapewnia przerwę przed następnym |

Zapis konfiguracji: **8N1** (8 bitów danych, brak parzystości, 1 bit stopu) – najpopularniejszy; 8E1, 7E2 itd.

**Narzut ramki**: dla 8N1 na bajt użyteczny przypada 10 bitów (1 start + 8 danych + 1 stop). Przy 115 200 bit/s przepływność użyteczna to ok. 11 520 bajtów/s (efektywność 80%).

### Linie i warianty elektryczne

- Minimalnie dwie linie sygnałowe: **TX** (nadawanie) i **RX** (odbiór) + masa (GND), **połączone „na krzyż"** (TX jednego do RX drugiego). Transmisja **pełnodupleksowa**, połączenie **punkt–punkt**.
- Opcjonalnie sterowanie przepływem: sprzętowe **RTS/CTS** lub programowe **XON/XOFF**.
- Poziomy logiczne:
  - **TTL/CMOS** (0/3,3 V lub 0/5 V; stan spoczynkowy = wysoki) – typowe dla mikrokontrolerów,
  - **RS-232** (napięcia ±3…±15 V, logika odwrócona; wymaga konwertera, np. MAX232),
  - **RS-422/RS-485** – sygnał różnicowy, długie linie, odporność na zakłócenia, RS-485 w układzie multi-drop (np. Modbus RTU).

### Błędy transmisji

- **Framing error** – brak bitu stopu we właściwym miejscu (zła szybkość, zakłócenia),
- **Parity error** – niezgodność bitu parzystości,
- **Overrun error** – nowy znak nadszedł, zanim poprzedni został odczytany z bufora (stąd bufory FIFO, przerwania, DMA),
- **Break** – dłuższy stan niski.

### Tolerancja szybkości

Zegary nadajnika i odbiornika mogą się nieznacznie różnić; błąd względny nie może przekroczyć kilku procent (praktycznie ok. 2–3%), bo narasta w trakcie jednej ramki (10 bitów) – przy większym odchyleniu próbka „wyjdzie" poza środek bitu. Dlatego obie strony muszą mieć **tę samą nastawę** baud rate, a rezonator kwarcowy zapewnia wymaganą dokładność.

## USRT (Universal Synchronous Receiver/Transmitter)

**USRT** to układ realizujący **synchroniczną** transmisję szeregową: bity są przesyłane w **ścisłej synchronizacji z sygnałem zegarowym**, a dane wysyła się w **blokach (ramkach)** bez bitów start/stop przy każdym znaku.

### Zasada działania

- **Zegar** (taktujący próbkowanie bitów) jest **dostarczany wspólną linią** (CLK; nadaje master) albo **odtwarzany z samego strumienia danych** (kodowanie samotaktujące, np. Manchester, NRZI, i pętla PLL/DPLL).
- Odbiornik musi znaleźć **początek ramki**: stosuje się **znaki synchronizacji** (np. bajt SYN w protokole Bisync) lub **flagi** (np. `01111110` w HDLC/SDLC wraz z wstawianiem bitów, *bit stuffing*).
- Po synchronizacji dane płyną **ciągle**, bez narzutu na każdy znak; kontrola błędów zwykle przez **CRC** całego bloku.

### Cechy

- **wyższa przepływność** i lepsze wykorzystanie łącza (mały narzut) niż w UART,
- konieczność **synchronizacji zegarów** i bardziej złożony sprzęt/protokół,
- dobre do szybkich lub ciągłych strumieni danych (telekomunikacja, sieci HDLC/SDLC, transmisja blokowa).

Przykłady synchronicznych interfejsów szeregowych: **SPI** (SCK), **I²C** (SCL), linie zegarowe w HDLC, I²S.

## USART (Universal Synchronous/Asynchronous Receiver/Transmitter)

Moduł **USART** (np. Intel 8251, USART w AVR i STM32) potrafi pracować **w obu trybach** – asynchronicznym (jak UART) i synchronicznym (z linią zegara). Dlatego w mikrokontrolerach spotyka się najczęściej UART/USART; nazwa **USRT** oznacza wersję wyłącznie synchroniczną.

## Porównanie UART i USRT

| Cecha | **UART** (asynchroniczny) | **USRT** (synchroniczny) |
| :--- | :--- | :--- |
| Zegar | brak wspólnego; każda strona ma własny | wspólna linia zegara lub zegar odtwarzany ze strumienia |
| Synchronizacja | **bit START** każdego znaku | znaki **SYNC**/flagi na początku bloku, synchronizacja zegara |
| Jednostka danych | pojedynczy znak (5–9 bitów) | blok / ramka (wiele znaków) |
| Bity start/stop | **tak**, przy każdym znaku | **nie** |
| Narzut | duży (np. 20% dla 8N1) | mały |
| Przepływność | niższa (do ok. 1 Mb/s typowo) | wyższa |
| Przerwy między znakami | dowolne | ciągły strumień danych |
| Kontrola błędów | bit parzystości | zwykle CRC bloku |
| Złożoność | prosta, tania | większa (synchronizacja, protokół) |
| Odporność na rozbieżność zegarów | mała tolerancja (kilka %) | brak problemu (wspólny zegar) lub PLL |
| Liczba linii | TX, RX (+GND) | + CLK (lub kodowanie samotaktujące) |
| Zastosowania | komunikacja z modułami (GPS, GSM, BLE), konsola debug, Modbus RTU | sieci HDLC/SDLC, szybkie transmisje blokowe, telekomunikacja |

## Podsumowanie

- **UART** – transmisja szeregowa **asynchroniczna**: bez linii zegara, znak oprawiony bitem START i STOP (opcjonalnie parzystość), z uzgodnioną prędkością (baud rate); prosty, ale z narzutem; popularny: 8N1.
- **USRT** – transmisja **synchroniczna**: zegar wspólny/odtwarzany, dane w blokach z synchronizacją SYNC/flagami, mały narzut i duża przepływność, większa złożoność.
- **USART** obsługuje oba tryby (w praktyce najczęściej spotykany moduł w mikrokontrolerach).
