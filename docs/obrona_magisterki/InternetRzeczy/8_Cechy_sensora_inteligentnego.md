# Jakie cechy powinien posiadać sensor inteligentny?

**Sensor inteligentny (smart sensor)** to czujnik, który oprócz samego pomiaru ma wbudowany **mikrokontroler lub układ przetwarzający** oraz interfejs komunikacyjny. Nie tylko mierzy wielkość fizyczną, ale także przetwarza dane, komunikuje się i potrafi się dostosować.

**Cechy sensora inteligentnego:**

- **Pomiar i przetwarzanie sygnału na miejscu.** Wzmacnia, filtruje i przetwarza sygnał z elementu pomiarowego i **zamienia go na postać cyfrową** (przetwornik ADC).
- **Wbudowana jednostka obliczeniowa**, która wykonuje obliczenia, np. skalowanie, linearyzację, uśrednianie i wstępną analizę danych (*edge computing*).
- **Kalibracja i kompensacja.** Sensor sam koryguje błędy, np. wpływ temperatury, dryft, nieliniowość, offset.
- **Samodiagnostyka.** Wykrywa własne uszkodzenia i sygnalizuje błędy.
- **Komunikacja cyfrowa.** Ma standardowy interfejs (I²C, SPI, UART, CAN, a w IoT Bluetooth, Wi-Fi, Zigbee, LoRa) i potrafi wymieniać dane z innymi urządzeniami i siecią.
- **Konfigurowalność.** Można zmieniać jego parametry, np. zakres, czułość, częstotliwość próbkowania.
- **Możliwość zdalnego sterowania i aktualizacji.**
- **Pamięć** do przechowywania danych kalibracyjnych, parametrów i identyfikatora urządzenia (często zgodnego z normą plug-and-play).
- **Niski pobór energii**, np. tryby uśpienia, co jest ważne przy zasilaniu bateryjnym.
- **Mała wielkość i niski koszt**.
- **Bezpieczeństwo** komunikacji (szyfrowanie, uwierzytelnianie), szczególnie w IoT.

**Przykład:** cyfrowy czujnik temperatury i wilgotności z wbudowanym przetwornikiem, kalibracją fabryczną i interfejsem I²C (np. SHT31, BME280), w przeciwieństwie do zwykłego termistora, który daje tylko sygnał analogowy.

## Podsumowanie

- Sensor inteligentny = **element pomiarowy + kondycjonowanie + ADC + mikrokontroler + interfejs cyfrowy** w jednej obudowie.
- Kluczowe cechy: przetwarzanie lokalne, wyjście cyfrowe i dwukierunkowa komunikacja, **samokalibracja i kompensacja**, **autodiagnostyka**, **samoidentyfikacja (TEDS)**, konfigurowalność, niski pobór mocy, fuzja danych i detekcja zdarzeń.
- W IoT zmniejsza ruch w sieci, skraca czas reakcji i upraszcza wdrożenie.

---
[⬅️ Poprzedni temat](7_Pojemność_pasożytnicza.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️️](9_Aktuatory.md)