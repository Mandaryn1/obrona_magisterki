# Internet Rzeczy – wprowadzenie i ściągawka pojęć

## Anatomia węzła IoT (urządzenia końcowego)

```
 ŚRODOWISKO            WĘZEŁ IoT                                  SIEĆ / CHMURA
 ┌────────┐   ┌─────────────────────────────────────────┐   ┌──────────────────┐
 │ wielkość│──▶│ SENSOR ─▶ kondycjonowanie ─▶ ADC ─┐     │   │  brama (gateway) │
 │ fizyczna│   │                                    ▼     │   │  broker MQTT     │
 └────────┘   │                         MIKROKONTROLER    │◀─▶│  platforma chmur.│
      ▲        │                       (CPU+pamięć+periferia)│   │  analityka, UI   │
      │        │                                    │     │   └──────────────────┘
      │        │ AKTUATOR ◀─ driver (MOSFET, mostek H) ◀──┘     moduł komunikacyjny
      └────────│                                          │     (Wi-Fi, BLE, LoRa…)
               │ zasilanie: bateria / sieć / energy harvesting │
               └─────────────────────────────────────────┘
```

Elementy: **sensory** (pomiar), **mikrokontroler** (przetwarzanie, sterowanie), **aktuatory** (oddziaływanie na otoczenie), **moduł komunikacyjny**, **zasilanie** (często bateryjne – priorytetem jest niski pobór mocy).

## Ściągawka pojęć i skrótów

| Skrót / pojęcie | Znaczenie |
| :--- | :--- |
| **IoT / IIoT** | Internet of Things / Industrial IoT (przemysłowy) |
| **MCU / MPU** | Microcontroller Unit / Microprocessor Unit |
| **SoC** | System on Chip – cały system (CPU, pamięć, peryferia, często radio) w jednym układzie |
| **SBC** | Single Board Computer (np. Raspberry Pi) |
| **ISA** | Instruction Set Architecture – lista rozkazów procesora (CISC, RISC) |
| **GPIO** | General Purpose Input/Output – uniwersalne wyprowadzenia cyfrowe |
| **ADC / DAC** | przetwornik analogowo-cyfrowy / cyfrowo-analogowy |
| **PWM** | Pulse Width Modulation – modulacja szerokości impulsów |
| **UART/USART** | asynchroniczny / uniwersalny synchroniczno-asynchroniczny nadajnik-odbiornik szeregowy |
| **SPI, I²C, 1-Wire, CAN** | popularne magistrale/interfejsy szeregowe w systemach wbudowanych |
| **MEMS** | Micro-Electro-Mechanical Systems – mikroukłady (czujniki, mikroaktuatory) |
| **MQTT, CoAP, LwM2M** | lekkie protokoły aplikacyjne IoT |
| **BLE, Zigbee, LoRaWAN, NB-IoT** | technologie łączności bezprzewodowej IoT |
| **Edge / Fog / Cloud** | przetwarzanie na brzegu / w warstwie pośredniej / w chmurze |
| **OTA** | Over-The-Air – zdalna aktualizacja oprogramowania |
| **RTOS** | Real-Time Operating System (np. FreeRTOS, Zephyr) |

## Mapa zagadnień

1. Czym jest IoT (architektura, zastosowania, wyzwania).
2. Jak urządzenia „rozmawiają" na poziomie sprzętu i oprogramowania – **magistrala, interfejs, protokół** (2), **UART/USRT** (6).
3. Co jest „mózgiem" węzła – **mikrokontroler vs mikroprocesor** (3), architektury **CISC** (4) i **RISC** (5).
4. Czym węzeł „czuje" i „działa" – **sensory inteligentne** (8), **aktuatory** (9), sterowanie mocą przez **PWM** (10).
5. Zjawiska fizyczne wpływające na działanie układów – **pojemność pasożytnicza** (7).

---
[⬅️ Poprzedni temat](InternetRzeczy_tytul.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](1_Zagadnienie_Internetu_Rzeczy.md)