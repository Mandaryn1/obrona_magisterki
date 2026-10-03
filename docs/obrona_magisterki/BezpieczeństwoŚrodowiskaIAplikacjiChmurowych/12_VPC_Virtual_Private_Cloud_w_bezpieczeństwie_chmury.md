# Do czego służą Virtual Private Clouds (VPC) w kontekście bezpieczeństwa chmury?

**VPC (Virtual Private Cloud)** to **logicznie wydzielona, prywatna sieć wirtualna** w chmurze publicznej, dostępna tylko dla danego klienta. Klient sam definiuje jej zakres adresów IP, podsieci, trasowanie i zasady ruchu, jak we własnej sieci lokalnej, ale na infrastrukturze dostawcy.

**Do czego służy w kontekście bezpieczeństwa:**

- **Izolacja.** Zasoby są odseparowane od innych klientów i od Internetu. Tylko to, co jawnie udostępnimy, jest widoczne z zewnątrz.
- **Segmentacja (podział na podsieci).** Dzielimy sieć na **podsieci publiczne** (np. serwery WWW, load balancery) i **prywatne** (aplikacje, bazy danych), które nie mają bezpośredniego dostępu z Internetu. Ogranicza to ruch boczny po włamaniu.
- **Kontrola ruchu.** Wykorzystuje się:
  - **Security Groups**: stanowe reguły przypisane do instancji,
  - **Network ACL**: bezstanowe reguły na poziomie podsieci,
  - tablice routingu oraz bramy: Internet Gateway, **NAT Gateway** (dla wyjścia z podsieci prywatnej) i VPN.
- **Bezpieczne połączenia.** Połączenie z siecią lokalną przez **VPN lub łącza dedykowane** (np. Direct Connect), a z usługami chmury przez **prywatne punkty końcowe (endpoints)**, bez ruchu przez publiczny Internet.
- **Mikrosegmentacja i zasada najmniejszych uprawnień** w komunikacji między usługami.
- **Monitoring i audyt.** Logi przepływu ruchu (flow logs) do wykrywania anomalii.
- **Zgodność** z regulacjami wymagającymi izolacji danych.

VPC realizuje więc **obronę w głąb** na poziomie sieci: ograniczenie powierzchni ataku, kontrola ruchu i separacja środowisk (produkcja, test, dev).

## Podsumowanie

- **VPC** = prywatna, logicznie izolowana sieć wirtualna klienta w chmurze publicznej (podsieci, trasy, bramy, reguły ruchu).
- Służy do **izolacji, segmentacji i kontroli ruchu**; razem z **Security Groups, NACL i firewallami** realizuje zasadę najmniejszych uprawnień w sieci.
- Podsieci prywatne dla danych i aplikacji, publiczne tylko dla brzegu; mikrosegmentacja ogranicza ruch boczny.

---
[⬅️ Poprzedni temat](11_RTO_Recovery_Time_Objective.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](13_TDE_Transparent_Data_Encryption.md)