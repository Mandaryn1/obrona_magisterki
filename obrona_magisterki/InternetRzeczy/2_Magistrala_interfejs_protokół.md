# Magistrala, interfejs, protokół – charakterystyka i różnice

## Magistrala (bus)

**Magistrala** to **wspólny zestaw linii (przewodów, ścieżek)**, którym **wiele urządzeń** wymienia dane lub jest zasilanych. Jest „drogą" łączącą bloki systemu (procesor, pamięć, układy wejścia/wyjścia, czujniki).

- Typowe magistrale systemowe: **adresowa** (wskazuje, z kim/gdzie), **danych** (przenosi informacje), **sterująca** (odczyt/zapis, zegar, przerwania, zezwolenia).
- Cechy: **szerokość** (liczba linii; np. 8/16/32 bity), **częstotliwość taktowania**, **przepustowość**, liczba urządzeń, długość, sposób dostępu (**arbitraż** – kto w danej chwili może nadawać; układ master/slave lub multi-master).
- Podziały: równoległa / szeregowa; synchroniczna (wspólny zegar) / asynchroniczna; simpleks / półdupleks / pełny dupleks; wewnętrzna (w procesorze, systemowa na płycie) / zewnętrzna (łącząca urządzenia).
- Przykłady: magistrala systemowa mikroprocesora, **I²C**, SPI (magistrala z liniami wyboru), **CAN**, **1-Wire**, RS-485, PCIe, USB (magistrala szeregowa topologii gwiazdy), Modbus.

Magistrala to więc **medium i organizacja połączeń** (co łączy i jak jest dzielone).

## Interfejs (interface)

**Interfejs** to **granica (punkt styku) między dwoma urządzeniami lub blokami systemu wraz z pełnym opisem warunków połączenia**: co, jak i na jakich zasadach wolno przez nią przekazywać. Określa wszystko, co trzeba zapewnić, by dwa elementy mogły ze sobą współpracować, **bez opisu ich wnętrza**.

Obejmuje zwykle:

- **warstwę mechaniczną** – złącza, rozmieszczenie wyprowadzeń, kable,
- **warstwę elektryczną** – poziomy napięć, prądy, impedancje, rodzaj wyjść (push-pull, otwarty kolektor), terminacja,
- **warstwę czasową** – zegar, czasy narastania, zależności czasowe sygnałów,
- **warstwę funkcjonalną** – znaczenie poszczególnych linii (TX, RX, CLK, CS, SDA...).

Rodzaje: sprzętowe (UART, SPI, I²C, USB, Ethernet PHY, GPIO), programowe (**API** – interfejs programisty, ABI), użytkownika (UI).

Przykłady: **interfejs szeregowy RS-232** (złącze, poziomy ±3…±15 V, linie TXD/RXD/RTS/CTS), interfejs UART w poziomach TTL (3,3 V/5 V), interfejs SPI (SCK, MOSI, MISO, CS).

## Protokół (protocol)

**Protokół** to **zbiór reguł komunikacji**: ustala **format komunikatów, ich kolejność, znaczenie (semantykę), adresowanie, synchronizację, kontrolę błędów i sterowanie przepływem**. Protokół mówi, **jak interpretować** przesyłane bity i **jak się zachować** w danej sytuacji (np. po otrzymaniu błędnej ramki), a nie – jak je fizycznie przesłać.

Typowe elementy: struktura ramki/pakietu (nagłówek, dane, suma kontrolna), polecenia i odpowiedzi, potwierdzenia (ACK/NACK), retransmisje, adresacja, negocjacja parametrów, obsługa błędów, timeouty.

Przykłady: **TCP/IP**, HTTP, **MQTT**, **CoAP**, **Modbus RTU/TCP**, Bluetooth LE (GATT), Zigbee, LoRaWAN, protokół I²C (warunek START, adres 7-bitowy, bit R/W, ACK, STOP), protokół komunikacyjny czujnika (ramka komend).

## Różnice

| Cecha | Magistrala | Interfejs | Protokół |
| :--- | :--- | :--- | :--- |
| **Czym jest** | wspólne linie łączące wiele urządzeń | zdefiniowany punkt styku (zasady połączenia) | zestaw reguł wymiany informacji |
| **Poziom** | głównie fizyczny/sprzętowy (topologia, dostęp) | sprzętowy (+ programowy) | logiczny (oprogramowanie, układy sterujące) |
| **Odpowiada na pytanie** | *którędy i z kim dzielone jest łącze?* | *jak fizycznie i elektrycznie się połączyć?* | *w jakim formacie i w jakiej kolejności rozmawiać?* |
| **Określa** | linie (adres, dane, sterowanie), arbitraż, przepustowość | złącza, napięcia, czasy, znaczenie sygnałów | ramki, polecenia, potwierdzenia, obsługę błędów |
| **Przykład** | magistrala systemowa, CAN | RS-232, USB (warstwa fizyczna), SPI | Modbus, MQTT, TCP/IP |

Zależności:

- bez **interfejsu** (wspólnych parametrów fizycznych) nie ma „łącza",
- bez **protokołu** same bity nie mają znaczenia (podobnie jak telefon bez wspólnego języka),
- **magistrala** to zwykle fizyczna realizacja, na której działa interfejs i protokół.
- Te pojęcia często **występują razem** w jednej nazwie standardu: np. „magistrala I²C" to i linie (SDA, SCL), i interfejs (otwarty dren, rezystory podciągające, poziomy), i protokół (ramki z adresem i ACK).

### Przykład: I²C

| Element | Opis |
| :--- | :--- |
| **Magistrala** | dwie linie współdzielone przez wiele układów: **SDA** (dane) i **SCL** (zegar); master/slave, możliwy multi-master |
| **Interfejs** | wyjścia z **otwartym drenem**, rezystory podciągające do $V_{DD}$, poziomy logiczne, ograniczenie pojemności magistrali (maks. ok. 400 pF), prędkości 100 kb/s (Standard), 400 kb/s (Fast), 1 Mb/s (Fast+), 3,4 Mb/s (High-speed) |
| **Protokół** | START → adres slave'a (7 bitów) + bit R/W → ACK → bajty danych (każdy z ACK/NACK) → STOP; arbitraż przy kolizji |

### Przykład: UART + RS-485 + Modbus RTU

- **Interfejs** elektryczny: RS-485 (sygnał różnicowy, półdupleks, terminacja),
- **Magistrala**: wspólna para skrętki, do 32 węzłów (wg specyfikacji),
- **Protokół**: Modbus RTU (adres urządzenia, kod funkcji, dane, CRC).

## Porównanie popularnych magistral i interfejsów w IoT

| Nazwa | Linie | Tryb | Typowa prędkość | Uwagi |
| :--- | :--- | :--- | :--- | :--- |
| **UART** | TX, RX (+GND) | asynchroniczny, pełny dupleks, punkt–punkt | do ok. 1 Mb/s (typowo 9600–115200 b/s) | prosty, bez zegara (zob. temat 6) |
| **SPI** | SCK, MOSI, MISO, CS | synchroniczny, pełny dupleks, master/slave | kilka–kilkadziesiąt Mb/s | szybki, osobna linia CS dla każdego slave'a |
| **I²C** | SDA, SCL | synchroniczny, półdupleks, adresowany | 100 kb/s – 3,4 Mb/s | wiele urządzeń na 2 liniach |
| **1-Wire** | 1 linia danych (+GND) | półdupleks, adresowanie 64-bit ID | ok. 16 kb/s | np. czujnik DS18B20 |
| **CAN** | CAN_H, CAN_L (różnicowo) | multi-master, priorytety, CRC | do 1 Mb/s (CAN FD – kilka Mb/s) | motoryzacja, przemysł |
| **RS-485** | A, B (różnicowo) | półdupleks, multi-drop | do 10 Mb/s (krótkie odcinki), do ok. 1200 m | długie linie, odporny na zakłócenia |
| **USB** | D+, D−, VBUS, GND | hostowy, hierarchiczny | 12 Mb/s – kilka–kilkadziesiąt Gb/s (zależnie od wersji) | zasilanie + dane |

## Podsumowanie

- **Magistrala** – wspólne linie i sposób ich współdzielenia przez wiele urządzeń.
- **Interfejs** – dokładnie opisany punkt styku (mechanika, elektryka, czasy, znaczenie sygnałów).
- **Protokół** – reguły wymiany informacji (format, kolejność, adresacja, potwierdzenia, błędy).
- Razem tworzą system komunikacji: magistrala to „droga", interfejs – „zasady połączenia z drogą", protokół – „język i etykieta rozmowy".
