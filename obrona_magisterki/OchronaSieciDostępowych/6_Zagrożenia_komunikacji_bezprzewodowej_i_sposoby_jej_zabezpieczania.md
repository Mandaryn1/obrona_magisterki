# Zagrożenia związane z komunikacją bezprzewodową oraz sposoby jej zabezpieczania

> Temat nie jest pokryty dostarczonymi materiałami (jedyne odniesienia: WPA3, WPS i gościnna sieć Wi-Fi w wykładzie IoT; „kontrola dostępu bezprzewodowego" w CIS Controls i *war-driving* w kursie Cisco). Opracowanie z własnej wiedzy ***(uzupełnienie)***.

## Specyfika medium bezprzewodowego

Fale radiowe **rozchodzą się poza fizyczne granice budynku** – brak „kabla" do ochrony. Dostęp do medium ma każdy w zasięgu: podsłuch pasywny jest niewykrywalny, a przeciwnik nie potrzebuje wejścia do budynku (parking, ulica). Stąd szyfrowanie i uwierzytelnianie są obowiązkowe.

## Zagrożenia

### Ataki pasywne

| Zagrożenie | Opis |
| :--- | :--- |
| **Podsłuch (eavesdropping/sniffing)** | przechwytywanie ruchu w sieciach otwartych lub słabo szyfrowanych |
| **War-driving / war-walking / war-chalking** | wyszukiwanie sieci Wi-Fi z samochodu/pieszo (kurs Cisco wymienia je jako starsze techniki testowe) |
| **Analiza ruchu i rozpoznanie** | zbieranie SSID, MAC, producentów, wzorców |

### Ataki na uwierzytelnienie i szyfrowanie

| Zagrożenie | Opis |
| :--- | :--- |
| **Łamanie WEP** | WEP (RC4 + krótki IV) łamany w minutach – całkowicie niezalecany |
| **Atak na WPA/WPA2-PSK** | przechwycenie 4-way handshake i **atak słownikowy/brute force offline** (słabe hasło) |
| **KRACK** (2017) | ponowne użycie nonce w 4-way handshake WPA2 |
| **Atak na WPS** | brute force PIN-u WPS (Reaver) – WPS należy wyłączyć |
| **Dragonblood** | luki we wczesnych implementacjach WPA3-SAE |
| **Słabe hasła i współdzielone hasło PSK** | wyciek hasła = dostęp wszystkich |

### Ataki aktywne

| Zagrożenie | Opis | 
| :--- | :--- |
| **Rogue AP** | nieautoryzowany AP podłączony do sieci firmowej (przez pracownika lub atakującego) – tylne drzwi omijające zaporę |
| **Evil twin** | fałszywy AP z tym samym SSID i silniejszym sygnałem; użytkownicy łączą się do atakującego → **MITM**, kradzież haseł (portal przechwytujący) |
| **Deauth / disassociation flood** | fałszywe ramki zarządzania rozłączają klientów (DoS, wymuszenie ponownego handshake'u) |
| **Jamming (zagłuszanie)** | zakłócanie pasma radiowego – DoS |
| **MITM, ARP spoofing w WLAN** | przechwytywanie i modyfikacja ruchu |
| **Fałszywe portale (captive portal phishing)** | wyłudzanie danych |
| **Ataki na klientów** | Karma/Pineapple (odpowiadanie na probe requests), ataki na niezałatanych klientów |
| **Przełamanie izolacji klientów, ruch boczny** w jednej sieci Wi-Fi | |
| **Kradzież tożsamości** (MAC spoofing) | |
| **Wi-Fi jako wektor do sieci wewnętrznej** | przejęty klient/AP = dostęp do LAN |
| **Wi-Fi w IoT i BYOD** | słabo zabezpieczone urządzenia w sieci |

### Inne technologie bezprzewodowe

**Bluetooth** (BlueBorne, bluesnarfing, bluejacking, słabe parowanie – wykład IoT: kluczowe właściwe parowanie, BLE 5.2 *LE Secure Connections*), **Zigbee** (zarządzanie kluczami, key-compromise; AES-128), **NFC/RFID** (klonowanie, relay), **LoRaWAN**, **sieci komórkowe** (IMSI catcher, fałszywa stacja bazowa).

## Zabezpieczanie komunikacji bezprzewodowej

### 1. Szyfrowanie i uwierzytelnianie

| Standard | Uwagi |
| :--- | :--- |
| **WEP** | niezabezpieczony – **nie używać** |
| **WPA (TKIP)** | przestarzały |
| **WPA2-Personal (PSK, AES-CCMP)** | akceptowalny przy bardzo silnym haśle; podatny na ataki słownikowe offline |
| **WPA2-Enterprise / WPA3-Enterprise (802.1X, EAP)** | **zalecany w organizacjach**: uwierzytelnianie indywidualne (RADIUS, certyfikaty EAP-TLS, PEAP), unikalne klucze per sesja, możliwość odwołania |
| **WPA3-Personal (SAE – Simultaneous Authentication of Equals)** | zastępuje PSK; **odporny na ataki słownikowe offline**, **forward secrecy**; WPA3-Enterprise 192-bit; obowiązkowy dla Wi-Fi 6 (wykład IoT) |
| **OWE / Enhanced Open** | szyfrowanie sieci otwartych (bez uwierzytelnienia) |
| **802.11w (PMF)** | ochrona ramek zarządzania (deauth) – obowiązkowa w WPA3 |

### 2. Konfiguracja i architektura

- **wyłączyć WEP, WPS**, ukrywanie SSID nie jest zabezpieczeniem (można je wykryć),
- **silne, długie hasła** (≥ 14–20 znaków) lub certyfikaty,
- **segmentacja**: osobne SSID/VLAN dla pracowników, **gości** (izolacja, tylko Internet), **IoT** (wykład IoT), urządzeń zarządzających; **izolacja klientów** (client isolation) na gościnnej,
- **kontrola dostępu do sieci**: 802.1X/NAC, ocena postury, MAC-filtering tylko jako uzupełnienie,
- zarządzanie AP: silne hasła, HTTPS/SSH, wyłączenie zdalnego dostępu z zewnątrz, aktualizacje firmware,
- **kontroler WLAN**, centralne zarządzanie i polityki,
- **moc nadajnika i zasięg** dopasowane do budynku (ograniczenie wycieku sygnału), rozmieszczenie AP,
- **VPN** dla dostępu z niezaufanych sieci (hotspoty), **TLS/HTTPS** wszędzie.

### 3. Wykrywanie i reakcja

- **WIDS/WIPS** (Wireless IDS/IPS): wykrywanie **rogue AP, evil twin, deauth flood**, nietypowych urządzeń, automatyczne ograniczanie (containment),
- **skanowanie radiowe** (spektrum, mapy zasięgu – Kismet, Aircrack-ng w audycie), regularne **wardriving audit** własnego terenu,
- monitoring logów kontrolera, SIEM,
- procedury usuwania nieautoryzowanych AP (lokalizacja, odłączenie portu).

### 4. Zasady dla użytkowników i BYOD

- nie łączyć się z nieznanymi sieciami/otwartymi hotspotami bez VPN; wyłączyć automatyczne łączenie z otwartymi sieciami,
- **MDM** i wymuszone profile Wi-Fi, certyfikaty urządzeń,
- aktualizacje systemów (KRACK, itp.), weryfikacja certyfikatu sieci (EAP) – nie akceptować ślepo,
- szkolenia (evil twin, captive portal phishing).

### 5. Inne technologie

- **Bluetooth:** wyłączanie, tryb niewykrywalny, bezpieczne parowanie (LE Secure Connections), aktualizacje,
- **Zigbee/Thread/LoRaWAN:** unikalne klucze, bezpieczny join, segmentacja bramy,
- **RFID/NFC:** szyfrowane karty (MIFARE DESFire), ekranowanie.

## Porównanie standardów

| Cecha | WEP | WPA2-PSK | WPA2/WPA3-Enterprise | WPA3-SAE |
| :--- | :-: | :-: | :-: | :-: |
| Szyfrowanie | RC4 (słabe) | AES-CCMP | AES-CCMP/GCMP | AES-GCMP |
| Uwierzytelnianie | brak/PSK | wspólny PSK | **indywidualne (802.1X)** | SAE (hasło) |
| Odporność na słownik offline | nie | **nie** | tak (EAP-TLS) | **tak** |
| Forward secrecy | nie | nie | tak | **tak** |
| Zalecenie | nie używać | małe sieci, silne hasło | **organizacje** | domowe/SOHO i nowe wdrożenia |

## Podsumowanie

- Zagrożenia WLAN: **podsłuch**, **łamanie WEP/WPA-PSK (atak słownikowy offline)**, **rogue AP**, **evil twin**, **deauth/jamming**, **MITM**, atak na **WPS**, **KRACK**; także Bluetooth, Zigbee, RFID.
- Zabezpieczenia: **WPA3 / WPA2-Enterprise (802.1X)**, wyłączenie WEP/WPS, silne hasła, **PMF (802.11w)**, **segmentacja** (goście, IoT), **WIDS/WIPS**, kontroler i zarządzanie AP, VPN, szkolenia.

---
[⬅️ Poprzedni temat](5_Zasady_projektowania_bezpiecznej_infrastruktury_sieci_lokalnej_i_zabezpieczanie_urządzeń.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Rola_wielowarstwowej_ochrony_w_zabezpieczaniu_lokalnych_sieci_komputerowych.md)