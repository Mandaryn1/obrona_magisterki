# Co to jest magistrala, interfejs, protokół? Scharakteryzuj i opisz różnice.

**Magistrala** to **fizyczny zestaw wspólnych linii** (przewodów, ścieżek), którymi urządzenia wymieniają dane, adresy i sygnały sterujące. Zwykle składa się z magistrali danych, adresowej i sterującej. Może być wspólna dla wielu urządzeń, np. I²C, SPI, CAN, a w komputerze PCIe czy USB.

**Interfejs** to **punkt styku i zbiór reguł połączenia** dwóch elementów. Określa warstwę fizyczną i elektryczną: złącza, poziomy napięć, liczbę linii, taktowanie i sygnały. Przykłady to UART/RS-232, SPI, I²C, USB, Ethernet. W praktyce interfejs często korzysta z jakiejś magistrali albo ją tworzy.

**Protokół** to **zbiór zasad komunikacji na poziomie logicznym**: format ramek, adresowanie, kolejność wymiany komunikatów, potwierdzenia, obsługa błędów. Nie mówi nic o przewodach i napięciach. Przykłady to I²C (protokół na magistrali dwuprzewodowej), Modbus, MQTT, CoAP, TCP/IP, HTTP.

**Różnice w skrócie:**

- **Magistrala** odpowiada na pytanie *którędy* płyną dane (medium i linie).
- **Interfejs** odpowiada na pytanie *jak się fizycznie i elektrycznie podłączyć*.
- **Protokół** odpowiada na pytanie *w jakim języku i według jakich reguł się porozumieć*.

**Przykład z I²C:** magistralę tworzą dwie linie, SDA i SCL. Interfejs określa poziomy napięć i rezystory podciągające. Protokół definiuje adres urządzenia, bit startu, bity potwierdzenia ACK i bit stopu.

Te pojęcia są ze sobą powiązane i bywają używane zamiennie, np. „interfejs SPI" i „magistrala SPI", ale oznaczają różne poziomy opisu komunikacji.

## Podsumowanie

- **Magistrala** – wspólne linie i sposób ich współdzielenia przez wiele urządzeń.
- **Interfejs** – dokładnie opisany punkt styku (mechanika, elektryka, czasy, znaczenie sygnałów).
- **Protokół** – reguły wymiany informacji (format, kolejność, adresacja, potwierdzenia, błędy).
- Razem tworzą system komunikacji: magistrala to „droga", interfejs – „zasady połączenia z drogą", protokół – „język i etykieta rozmowy".

---
[⬅️ Poprzedni temat](1_Zagadnienie_Internetu_Rzeczy.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](3_Mikrokontroler_i_mikroprocesor.md)