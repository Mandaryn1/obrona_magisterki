# Internet Rzeczy (IoT) – krótki opis zagadnienia

## Definicja

**Internet Rzeczy (Internet of Things, IoT)** to koncepcja, w której **fizyczne obiekty ("rzeczy")** – urządzenia, maszyny, pojazdy, budynki, przedmioty codziennego użytku – są wyposażone w **czujniki, elementy wykonawcze, jednostkę obliczeniową i łączność sieciową**, dzięki czemu mogą **zbierać dane, wymieniać je ze sobą i z systemami w chmurze oraz reagować na ich podstawie bez ciągłego udziału człowieka**.

- Termin spopularyzował **Kevin Ashton** (1999), początkowo w kontekście znaczników RFID w łańcuchach dostaw.
- Według definicji ITU (Y.2060): globalna infrastruktura społeczeństwa informacyjnego umożliwiająca zaawansowane usługi przez łączenie (fizycznych i wirtualnych) rzeczy na bazie istniejących i rozwijających się technologii informacyjno-komunikacyjnych.
- Różnica względem „zwykłego" Internetu: tu głównymi uczestnikami nie są ludzie, lecz **urządzenia (komunikacja M2M – machine-to-machine)**, często tanie, o ograniczonych zasobach i zasilane bateryjnie.

## Idea działania – pętla IoT

1. **Sensing** – czujniki mierzą wielkości fizyczne (temperatura, ruch, światło, położenie, zużycie energii).
2. **Przetwarzanie lokalne** – mikrokontroler filtruje, kalibruje, agreguje dane (*edge computing*).
3. **Komunikacja** – dane są wysyłane przez sieć (przewodową lub bezprzewodową) do bramy i chmury.
4. **Analiza i decyzja** – w chmurze lub na brzegu: analityka, uczenie maszynowe, reguły.
5. **Akcja** – komendy wracają do **aktuatorów** (zawór, silnik, przekaźnik) lub do użytkownika (aplikacja, alert).

## Architektura (warstwy)

| Warstwa | Zadanie | Przykłady |
| :--- | :--- | :--- |
| **Percepcji / urządzeń** | pomiary i działanie | czujniki, aktuatory, mikrokontrolery, tagi RFID |
| **Sieciowa / transportowa** | przesył danych | Wi-Fi, BLE, Zigbee, Thread, LoRaWAN, NB-IoT/LTE-M, 5G, Ethernet; IPv6/6LoWPAN |
| **Przetwarzania / platformy** | gromadzenie, analiza, zarządzanie urządzeniami | brama, edge/fog, platformy chmurowe (AWS IoT, Azure IoT, ...), bazy szeregów czasowych |
| **Aplikacji** | usługi dla użytkownika | aplikacje mobilne, dashboardy, integracje biznesowe |

Przekrojowo: **bezpieczeństwo** (uwierzytelnianie, szyfrowanie, aktualizacje) i **zarządzanie** (provisioning, monitorowanie, OTA).

### Protokoły i technologie

| Obszar | Rozwiązania |
| :--- | :--- |
| Aplikacyjne | **MQTT** (publish/subscribe, broker), **CoAP** (REST dla urządzeń ograniczonych, UDP), HTTP/REST, AMQP, LwM2M, OPC UA (przemysł) |
| Transportowe/sieciowe | TCP/UDP, IPv6, 6LoWPAN |
| Łączność | krótki zasięg: BLE, Wi-Fi, Zigbee, Thread, NFC; **LPWAN** (daleki zasięg, niski pobór): LoRaWAN, Sigfox, NB-IoT, LTE-M; komórkowe: LTE, 5G |
| Interfejsy lokalne czujników | I²C, SPI, UART, 1-Wire, CAN, Modbus |

Dobór łączności to kompromis: **zasięg – przepustowość – pobór energii – koszt**.

## Zastosowania

- **Dom inteligentny** (oświetlenie, ogrzewanie, zamki, alarmy),
- **Inteligentne miasta** (oświetlenie uliczne, parkowanie, monitoring jakości powietrza, gospodarka odpadami),
- **Przemysł 4.0 / IIoT** (monitoring maszyn, **predykcyjne utrzymanie ruchu**, śledzenie produkcji),
- **Rolnictwo precyzyjne** (czujniki wilgotności gleby, nawadnianie),
- **Ochrona zdrowia** (urządzenia ubieralne, zdalny monitoring pacjentów),
- **Logistyka i transport** (śledzenie przesyłek i floty, łańcuch chłodniczy),
- **Energetyka** (inteligentne liczniki, sieci smart grid),
- **Motoryzacja** (pojazdy połączone).

## Zalety

- automatyzacja i oszczędność (energia, czas, koszty), lepsza efektywność procesów,
- dostęp do danych w czasie rzeczywistym i decyzje oparte na danych,
- zdalny monitoring i sterowanie, wczesne wykrywanie awarii,
- nowe usługi i modele biznesowe.

## Wyzwania i zagrożenia

| Obszar | Problem |
| :--- | :--- |
| **Bezpieczeństwo** | słabe hasła domyślne, brak aktualizacji, podatne urządzenia (botnety, np. Mirai), ataki na łączność, fizyczny dostęp do urządzeń |
| **Prywatność** | zbieranie danych osobowych i behawioralnych (RODO) |
| **Interoperacyjność** | wiele standardów i producentów (inicjatywy: Matter, Thread) |
| **Zasilanie** | ograniczona energia baterii → energooszczędność, *energy harvesting* |
| **Skalowalność** | zarządzanie tysiącami–milionami urządzeń, adresacja (IPv6) |
| **Ilość danych i opóźnienia** | przetwarzanie na brzegu zamiast wysyłania wszystkiego do chmury |
| **Niezawodność i regulacje** | wymagania prawne i certyfikacyjne dla urządzeń podłączonych do sieci |

## Podsumowanie

- IoT = sieć fizycznych obiektów z czujnikami, aktuatorami, mocą obliczeniową i łącznością, wymieniających dane i reagujących na nie (głównie M2M).
- Pętla: pomiar → przetwarzanie lokalne → komunikacja → analiza w chmurze/na brzegu → akcja.
- Architektura warstwowa: urządzenia – sieć – platforma – aplikacje, z bezpieczeństwem jako zagadnieniem przekrojowym.
- Główne wyzwania: bezpieczeństwo i prywatność, interoperacyjność, energia, skalowalność.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Magistrala_interfejs_protokół.md)