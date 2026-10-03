# Zagrożenia związane z komunikacją bezprzewodową oraz sposoby jej zabezpieczania

**Komunikacja bezprzewodowa** jest szczególnie podatna na ataki, bo sygnał rozchodzi się w powietrzu i można go przechwycić **bez fizycznego dostępu** do sieci. Zasięg często wychodzi poza budynek.

**Zagrożenia:**

- **Podsłuch (sniffing)** niezaszyfrowanego lub słabo szyfrowanego ruchu.
- **Słabe zabezpieczenia:**
  - **WEP** jest całkowicie złamany,
  - **WPA/WPA2-PSK** podatne na atak słownikowy offline po przechwyceniu uzgodnienia (handshake) przy słabym haśle,
  - **KRACK** (błąd w uzgadnianiu WPA2),
  - **WPS** podatne na brute force PIN-u (Reaver).
- **Fałszywe punkty dostępowe:** **rogue AP** (nielegalny AP w sieci firmowej) i **evil twin** (kopia legalnej sieci, która przechwytuje ruch i hasła, MITM).
- **Ataki DoS:** **deauth flood** (wymuszanie rozłączeń) i **jamming** (zagłuszanie).
- **Rozpoznanie:** **war-driving** (wyszukiwanie sieci).
- **Inne technologie:** Bluetooth (np. BlueBorne), Zigbee, NFC.

**Sposoby zabezpieczania:**

- **WPA3-Personal:** uwierzytelnianie SAE (odporne na ataki słownikowe offline), **forward secrecy** i ochrona ramek zarządzających **802.11w (PMF)**, co zabezpiecza przed deauth.
- **WPA2/WPA3-Enterprise:** **802.1X** z serwerem **RADIUS** i metodami EAP (**EAP-TLS**, PEAP), osobne poświadczenia dla użytkowników.
- **Wyłączenie WEP i WPS**, silne długie hasła i regularna ich zmiana (dla PSK).
- **Segmentacja SSID i VLAN:** osobne sieci dla gości, IoT i pracowników, z kontrolą zaporą.
- **WIDS/WIPS:** wykrywanie i blokowanie rogue AP, evil twin i deauth.
- **VPN** w niezaufanych sieciach (hotspoty), weryfikacja certyfikatu sieci.
- **Zarządzanie AP:** aktualizacje firmware, zmiana domyślnych haseł, wyłączenie zarządzania przez sieć bezprzewodową, kontrola mocy i zasięgu sygnału.
- **Przegląd (audyt) środowiska radiowego** i szkolenie użytkowników.

**Porównanie zabezpieczeń:** WEP (złamany) → WPA2-PSK (dobry przy silnym haśle) → WPA2-Enterprise (lepszy, uwierzytelnianie indywidualne) → **WPA3 (najlepszy)**.

## Podsumowanie

- Zagrożenia WLAN: **podsłuch**, **łamanie WEP/WPA-PSK (atak słownikowy offline)**, **rogue AP**, **evil twin**, **deauth/jamming**, **MITM**, atak na **WPS**, **KRACK**; także Bluetooth, Zigbee, RFID.
- Zabezpieczenia: **WPA3 / WPA2-Enterprise (802.1X)**, wyłączenie WEP/WPS, silne hasła, **PMF (802.11w)**, **segmentacja** (goście, IoT), **WIDS/WIPS**, kontroler i zarządzanie AP, VPN, szkolenia.

---
[⬅️ Poprzedni temat](5_Zasady_projektowania_bezpiecznej_infrastruktury_sieci_lokalnej_i_zabezpieczanie_urządzeń.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Rola_wielowarstwowej_ochrony_w_zabezpieczaniu_lokalnych_sieci_komputerowych.md)