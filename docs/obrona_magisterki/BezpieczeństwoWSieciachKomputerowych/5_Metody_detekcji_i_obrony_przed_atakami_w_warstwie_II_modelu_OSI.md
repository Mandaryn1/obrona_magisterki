# Metody detekcji i obrony przed atakami w warstwie II modelu OSI

Warstwa II (łącza danych) jest zagrożona, bo przełączniki domyślnie ufają urządzeniom w sieci. Ataki L2 zwykle wykonuje ktoś już **wewnątrz sieci lokalnej**, więc obrona opiera się na zabezpieczeniach przełączników.

**Ataki i obrona:**

- **MAC flooding (przepełnienie tablicy CAM):** atakujący zalewa przełącznik fałszywymi adresami MAC, a ten zaczyna rozsyłać ramki do wszystkich portów (jak hub), co umożliwia podsłuch. Obrona: **Port Security**: limit adresów MAC na porcie, adresy „sticky" i reakcja (shutdown, restrict).
- **ARP spoofing / poisoning (MITM):** fałszywe odpowiedzi ARP podszywają się pod bramę lub inny host. Obrona: **DAI (Dynamic ARP Inspection)**, która weryfikuje pakiety ARP w tabeli powiązań DHCP snooping, oraz statyczne wpisy ARP dla krytycznych hostów.
- **Rogue DHCP i DHCP starvation:** fałszywy serwer DHCP podaje własną bramę i DNS albo wyczerpuje pulę adresów. Obrona: **DHCP snooping** (porty zaufane i niezaufane, limity żądań) oraz **IP Source Guard**.
- **Ataki na STP:** fałszywe BPDU przejmuje rolę root bridge i przekierowuje ruch. Obrona: **BPDU Guard**, **Root Guard**, PortFast tylko na portach dostępowych.
- **VLAN hopping** (switch spoofing, podwójne znakowanie 802.1Q): atakujący dostaje się do innego VLAN. Obrona: porty dostępowe na stałe w trybie access, **wyłączenie DTP**, zmiana natywnego VLAN-u (niewykorzystywanego), nieużywane porty wyłączone.
- **MAC spoofing:** podszycie się pod cudzy adres. Obrona: port security, **802.1X**.

**Ogólne zabezpieczenia:**

- **802.1X (NAC)** uwierzytelnia urządzenia przed dostępem do sieci,
- segmentacja VLAN,
- wyłączenie nieużywanych portów i protokołów (CDP, DTP),
- **MACsec** (szyfrowanie L2).

**Detekcja:**

- **logi i alerty przełączników** (naruszenia port security, DAI, DHCP snooping, zmiany topologii STP),
- **SIEM** z korelacją tych zdarzeń,
- **IDS/NDR**, Wireshark i narzędzia monitorujące (np. arpwatch wykrywa zmiany par IP–MAC),
- monitoring **nietypowego ruchu**: skok liczby adresów MAC, powtórzone odpowiedzi ARP, podejrzane DHCP Offer.

Skuteczna obrona łączy konfigurację przełączników, kontrolę dostępu 802.1X i monitoring.

## Podsumowanie

- Ataki L2 (**MAC flooding, ARP spoofing, rogue DHCP/starvation, STP, VLAN hopping, MAC spoofing**) wykorzystują zaufanie w segmencie i omijają zapory.
- Obrona to **mechanizmy przełączników**: **Port Security, DHCP snooping, DAI, IP Source Guard, BPDU/Root Guard**, poprawna konfiguracja VLAN/trunków, **802.1X/NAC**, storm control, ochrona fizyczna.
- Detekcja: logi przełączników, **arpwatch**, analiza ruchu (Wireshark, NIDS na SPAN/TAP), SIEM.

---
[⬅️ Poprzedni temat](4_Klasyfikacja_najważniejszych_rodzajów_ataków_sieciowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_Metody_detekcji_i_obrony_przed_atakami_w_warstwie_III_modelu_OSI.md)