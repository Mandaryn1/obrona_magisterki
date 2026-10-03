# Krótko opisz zagadnienie Internetu Rzeczy

**Internet Rzeczy (IoT)** to koncepcja, w której fizyczne obiekty, takie jak urządzenia, maszyny, pojazdy czy budynki, są wyposażone w **czujniki, elementy wykonawcze, jednostkę obliczeniową i łączność sieciową**. Dzięki temu mogą zbierać dane, wymieniać je między sobą i z chmurą oraz reagować na nie bez ciągłego udziału człowieka. Termin spopularyzował Kevin Ashton w 1999 r. w kontekście znaczników RFID. Od zwykłego Internetu różni się tym, że głównymi uczestnikami są **urządzenia (komunikacja M2M)**, często tanie, zasilane bateryjnie i o ograniczonych zasobach.

**Działanie to pętla:** czujniki mierzą wielkości fizyczne, mikrokontroler wstępnie przetwarza dane, sieć przesyła je do bramy i chmury, tam następuje analiza i decyzja, a komenda wraca do **aktuatora** (np. zaworu, silnika) lub do użytkownika.

**Architektura ma cztery warstwy:**

- percepcji (czujniki i aktuatory),
- sieciowa (Wi-Fi, BLE, Zigbee, LoRaWAN, NB-IoT, 5G),
- przetwarzania (brama, chmura),
- aplikacji.

Typowe protokoły aplikacyjne to **MQTT** i **CoAP**. Przekrojowo działają bezpieczeństwo i zarządzanie urządzeniami.

**Zastosowania:** dom inteligentny, inteligentne miasta, przemysł (IIoT i predykcyjne utrzymanie ruchu), rolnictwo precyzyjne, ochrona zdrowia, logistyka, energetyka (smart grid) i motoryzacja.

**Zalety:** automatyzacja, oszczędność, dane w czasie rzeczywistym, zdalny monitoring i sterowanie.

**Wyzwania:** **bezpieczeństwo** (słabe hasła domyślne, brak aktualizacji, botnety jak Mirai), prywatność danych (RODO), interoperacyjność standardów, ograniczone zasilanie i skalowalność.

## Podsumowanie

- IoT = sieć fizycznych obiektów z czujnikami, aktuatorami, mocą obliczeniową i łącznością, wymieniających dane i reagujących na nie (głównie M2M).
- Pętla: pomiar → przetwarzanie lokalne → komunikacja → analiza w chmurze/na brzegu → akcja.
- Architektura warstwowa: urządzenia – sieć – platforma – aplikacje, z bezpieczeństwem jako zagadnieniem przekrojowym.
- Główne wyzwania: bezpieczeństwo i prywatność, interoperacyjność, energia, skalowalność.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Magistrala_interfejs_protokół.md)