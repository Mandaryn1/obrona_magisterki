# Podstawowe zagrożenia występujące w lokalnych sieciach komputerowych i ich wpływ na bezpieczeństwo infrastruktury

> Temat nie jest pokryty dostarczonymi materiałami. Opracowanie z własnej wiedzy ***(uzupełnienie)***; w tle: model CIA i klasyfikacja ataków z poprzedniego przedmiotu (*Bezpieczeństwo w sieciach komputerowych*, temat 4).

## Dlaczego LAN jest szczególnie narażona

- **Zaufanie wewnątrz segmentu** – protokoły L2/L3 (ARP, DHCP, STP) powstały bez uwierzytelniania; zapory perymetru nie widzą ruchu **wewnątrz** LAN,
- **fizyczna dostępność** – gniazda w salach, biurach, korytarzach; podpięcie obcego urządzenia,
- **wiele typów urządzeń** (stacje, serwery, drukarki, IoT, BYOD) o różnym poziomie zabezpieczeń,
- **ruch boczny** – przejęcie jednego hosta daje punkt wyjścia do dalszych (APT: *lateral movement*),
- **czynnik ludzki** (phishing, hasła, nieautoryzowane urządzenia, shadow IT).

## Klasyfikacja zagrożeń LAN

### 1. Zagrożenia fizyczne

| Zagrożenie | Skutek |
| :--- | :--- |
| **podpięcie obcego urządzenia** (laptop, mini-komputer, *rogue device*, keylogger sprzętowy) do wolnego gniazda | podsłuch, ataki L2, wejście do sieci |
| kradzież sprzętu, nośników | utrata danych |
| sabotaż, uszkodzenie kabli i szaf | utrata dostępności |
| awarie zasilania, klimatyzacji, katastrofy | przestoje |
| dostęp do portów konsolowych/zarządzania | przejęcie urządzenia |

### 2. Zagrożenia na warstwie 2 i 3 (zob. temat 2)

MAC flooding, **ARP spoofing**, **rogue DHCP**, DHCP starvation, ataki na STP, VLAN hopping, IP spoofing, ICMP nadużycia, ataki na routing.

### 3. Podsłuch i przechwytywanie (poufność)

- **sniffing** w segmencie (huby, SPAN, MITM po ARP spoofingu), podsłuch niezaszyfrowanych protokołów (Telnet, FTP, HTTP, SNMPv2c),
- **MITM** (ARP/DNS spoofing, fałszywy serwer DHCP, SSL stripping),
- kradzież haseł i tokenów sesji, przechwytywanie ruchu Wi-Fi.

### 4. Ataki na dostępność

- **DoS/DDoS** wewnętrzne i zewnętrzne (SYN flood, ICMP flood, burze rozgłoszeniowe, pętle L2 bez STP),
- wyczerpanie zasobów (pula DHCP, tablica CAM, połączenia),
- awaria pojedynczego punktu (SPOF), niewłaściwa konfiguracja STP,
- ransomware szyfrujący zasoby sieciowe.

### 5. Złośliwe oprogramowanie i ataki na hosty

- wirusy, **robaki** (rozprzestrzenianie przez SMB/RDP – **WannaCry/EternalBlue**), trojany (RAT), ransomware, spyware, rootkity, botnety,
- exploity niezałatanych podatności (zero-day), **fileless malware**,
- zainfekowane nośniki USB (*baiting*), makra w dokumentach.

### 6. Ataki na tożsamość i dostęp

- **brute force**, password spraying, credential stuffing, słabe i domyślne hasła,
- kradzież poświadczeń (**pass-the-hash**, Kerberoasting), eskalacja uprawnień,
- **nadmierne uprawnienia**, osierocone konta, współdzielone konta,
- obejście kontroli dostępu przez podszywanie się (MAC/IP spoofing).

### 7. Socjotechnika

phishing, spear phishing, pretexting, **tailgating** (wejście „na ogon"), baiting, vishing – skuteczne, bo omijają zabezpieczenia techniczne.

### 8. Zagrożenia związane z urządzeniami i usługami

- **drukarki, kamery, urządzenia IoT** z domyślnymi hasłami i brakiem aktualizacji (botnet **Mirai**),
- **rogue AP** i nieautoryzowane urządzenia bezprzewodowe,
- przestarzałe usługi i protokoły (SMBv1, Telnet, SNMPv1/2c, NetBIOS/LLMNR),
- błędne konfiguracje urządzeń sieciowych (domyślne hasła, nieużywane porty, otwarte usługi zarządzania),
- niezałatane systemy i firmware.

### 9. Zagrożenia wewnętrzne (insider)

Złośliwi pracownicy (kradzież danych, sabotaż), **nieświadomi** użytkownicy (błędy, kliknięcia, złe praktyki), uprzywilejowani administratorzy z nadużyciami; **shadow IT** (nieautoryzowane usługi chmurowe i urządzenia).

### 10. Zagrożenia łańcucha dostaw i zewnętrzne

podatne oprogramowanie i sprzęt, kompromitacja dostawcy (NotPetya przez aktualizację MeDoc), wycieki z usług zewnętrznych.

## Wpływ na bezpieczeństwo infrastruktury

| Zagrożenie | Naruszona własność | Wpływ na infrastrukturę |
| :--- | :--- | :--- |
| podsłuch, MITM, kradzież haseł | **poufność** | wyciek danych, przejęcie kont, dalsze ataki |
| spoofing, modyfikacja ruchu, malware | **integralność** | zafałszowane dane i konfiguracje, nieufny ruch, ukryte backdoory |
| DoS, ransomware, pętle L2, awarie | **dostępność** | przestoje usług i produkcji, utrata danych |
| podszywanie, słabe hasła | **uwierzytelnienie/autoryzacja** | nieuprawniony dostęp, eskalacja uprawnień |
| ruch boczny, brak segmentacji | wszystkie | **rozprzestrzenianie się incydentu** na całą sieć |
| brak widoczności i logów | rozliczalność | późne wykrycie, trudna analiza powłamaniowa |

**Skutki biznesowe:** straty finansowe (przestoje, okupy, naprawa), kary (RODO, NIS2), utrata reputacji i zaufania, ujawnienie własności intelektualnej, zagrożenie ciągłości działania. Przykład: **WannaCry** (2017) rozszerzył się przez nieposegmentowane sieci wewnętrzne dzięki niezałatanemu SMBv1.

## Czynniki zwiększające ryzyko

nieposegmentowana „płaska" sieć, brak aktualizacji, domyślne konfiguracje, brak 802.1X/NAC, niewłaściwe uprawnienia, brak monitoringu, BYOD bez kontroli, IoT bez zabezpieczeń, brak szkoleń.

## Zasady ograniczania zagrożeń (skrót – szczegóły w tematach 5–7)

segmentacja (VLAN, strefy), **kontrola dostępu do portów (802.1X, port security)**, zabezpieczenia L2 (DHCP snooping, DAI), szyfrowanie, zarządzanie podatnościami i poprawkami, MFA i least privilege, **monitoring i alerty**, kopie zapasowe, szkolenia, polityki (ISMS).

## Podsumowanie

- Główne zagrożenia LAN: **fizyczne**, **ataki L2/L3**, **podsłuch/MITM**, **DoS**, **malware**, **ataki na hasła i uprawnienia**, **socjotechnika**, **niezabezpieczone urządzenia (IoT, drukarki, rogue AP)**, **insider**, **łańcuch dostaw**.
- Wpływ: naruszenie **CIA**, rozprzestrzenianie się incydentu (ruch boczny), straty finansowe, prawne i wizerunkowe.
- Ochrona wymaga podejścia wielowarstwowego (temat 7).

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Podatności_Ethernetu_i_protokołów_lokalnych_sieci_komputerowych.md)