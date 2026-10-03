# Podstawowe zagrożenia występujące w lokalnych sieciach komputerowych i ich wpływ na bezpieczeństwo infrastruktury

**Lokalna sieć (LAN)** jest zagrożona zarówno z zewnątrz, jak i od środka. Atakujący wewnątrz LAN omija zabezpieczenia granicy sieci, a wiele protokołów lokalnych nie ma uwierzytelniania ani szyfrowania.

**Podstawowe zagrożenia:**

- **Fizyczne:** podpięcie obcego urządzenia do wolnego gniazda, kradzież sprzętu.
- **Ataki na warstwę 2 i 3:** MAC flooding, ARP spoofing, fałszywy serwer DHCP, ataki na STP, VLAN hopping, IP spoofing.
- **Podsłuch i przechwycenie ruchu (sniffing, MITM):** szczególnie przy protokołach jawnych (Telnet, FTP, HTTP, SNMPv1/v2c).
- **DoS/DDoS:** zalewanie sieci ruchem i wyczerpywanie zasobów.
- **Złośliwe oprogramowanie:** wirusy, robaki, ransomware, rozprzestrzeniające się między hostami (np. WannaCry przez SMBv1).
- **Ataki na hasła:** słabe i domyślne hasła, brute force.
- **Socjotechnika:** phishing i wyłudzanie danych.
- **Niezabezpieczone urządzenia:** IoT, drukarki, **fałszywe punkty dostępowe (rogue AP)**.
- **Zagrożenia wewnętrzne (insiderzy)** i **łańcuch dostaw** (skompromitowany sprzęt lub oprogramowanie).

**Wpływ na bezpieczeństwo (triada CIA):**

- **poufność:** podsłuch, kradzież danych i poświadczeń,
- **integralność:** modyfikacja danych w tranzycie, podszywanie się pod urządzenia,
- **dostępność:** przestoje i niedostępność usług przez ataki DoS lub ransomware.

**Skutki biznesowe:** straty finansowe, przestoje, utrata zaufania, kary regulacyjne (np. RODO), koszty odtwarzania. Skompromitowany jeden host może posłużyć do **ruchu bocznego** i przejęcia całej infrastruktury.

**Obrona:** segmentacja, 802.1X/NAC, zabezpieczenia przełączników (port security, DAI, DHCP snooping), szyfrowanie, aktualizacje, monitoring i szkolenia użytkowników.

## Podsumowanie

- Główne zagrożenia LAN: **fizyczne**, **ataki L2/L3**, **podsłuch/MITM**, **DoS**, **malware**, **ataki na hasła i uprawnienia**, **socjotechnika**, **niezabezpieczone urządzenia (IoT, drukarki, rogue AP)**, **insider**, **łańcuch dostaw**.
- Wpływ: naruszenie **CIA**, rozprzestrzenianie się incydentu (ruch boczny), straty finansowe, prawne i wizerunkowe.
- Ochrona wymaga podejścia wielowarstwowego.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Podatności_Ethernetu_i_protokołów_lokalnych_sieci_komputerowych.md)