# Virtual Private Clouds (VPC) w kontekście bezpieczeństwa chmury

## Definicja

**VPC (Virtual Private Cloud)** to **logicznie izolowana, prywatna sieć wirtualna** tworzona przez klienta wewnątrz publicznej chmury. Klient sam definiuje zakres adresów IP, podsieci, trasowanie i reguły ruchu, dzięki czemu zasoby (maszyny, kontenery, bazy danych) działają w **wydzielonym, kontrolowanym środowisku sieciowym**, podobnym do własnej sieci w centrum danych.

Nazwy: **VPC** (AWS, Google Cloud), **VNet – Virtual Network** (Azure).

## Rola VPC w bezpieczeństwie (wykład)

- **Segmentacja sieci** dzieli sieć na mniejsze, izolowane segmenty dla **lepszej kontroli ruchu i redukcji ryzyka** (slajd 27).
- W chmurze segmentację realizuje się przez **VPC, Security Groups, Network ACL (NACL) i firewalle**.
- **VPC izolują zasoby na poziomie sieci**, natomiast **Security Groups i NACL kontrolują ruch na poziomie instancji i podsieci**.
- **Mikrosegmentacja** – bardzo szczegółowa kontrola ruchu między aplikacjami; szczególnie ważna w **mikroserwisach**, gdzie każda usługa może mieć własne reguły.
- Segmentacja powinna opierać się na zasadzie **najmniejszych uprawnień** – dozwolony tylko niezbędny ruch.
- Sieci wirtualne (VPC/VNet) zapewniają **izolację i segmentację** oraz kontrolę przepływu ruchu (slajd 331); **VPN i gatewaye** zapewniają bezpieczne połączenia z środowiskiem lokalnym.
- W chmurze (np. **AWS RDS**) bezpieczeństwo bazy wzmacniają **VPC, firewalle i role IAM**, ale aplikacja musi sama zarządzać dostępem wewnętrznym, z minimalnymi uprawnieniami (slajd 299).

## Elementy VPC

| Element | Funkcja bezpieczeństwa |
| :--- | :--- |
| **Zakres adresów (CIDR)** | prywatna przestrzeń adresowa (np. 10.0.0.0/16), brak nakładania z siecią lokalną (potrzebne dla VPN) |
| **Podsieci (subnets)** | **publiczne** (z trasą do Internetu, np. load balancer, bastion) i **prywatne** (aplikacje, bazy – bez bezpośredniego dostępu z Internetu) |
| **Tablice tras (route tables)** | decydują, dokąd kierowany jest ruch (do Internetu, do NAT, do peeringu) |
| **Internet Gateway** | wyjście/wejście do Internetu dla podsieci publicznych |
| **NAT Gateway** | pozwala zasobom w podsieciach **prywatnych** wychodzić do Internetu (np. aktualizacje) **bez wystawiania ich na zewnątrz** |
| **Security Group (SG)** | wirtualny **firewall na poziomie instancji/interfejsu**; **stanowy** (stateful) – odpowiedzi automatycznie dozwolone; tylko reguły **zezwalające** (domyślnie wszystko zablokowane przychodzące) |
| **Network ACL (NACL)** | firewall na poziomie **podsieci**; **bezstanowy** (stateless); reguły zezwalające i blokujące, numerowane |
| **VPC peering / Transit Gateway** | kontrolowane połączenia między VPC (bez przechodzenia przez Internet) |
| **VPN / Direct Connect / ExpressRoute** | bezpieczne (szyfrowane lub prywatne) połączenie z siecią lokalną (hybryda) |
| **VPC Endpoints / PrivateLink** | dostęp do usług chmurowych (np. S3, baza) **przez sieć prywatną**, bez wychodzenia do Internetu |
| **Flow Logs** | rejestr ruchu sieciowego do analizy, SIEM, wykrywania anomalii |

## Typowa architektura trójwarstwowa

```
                    Internet
                       │
             ┌─────────▼─────────┐   podsieć PUBLICZNA
             │ Load Balancer / WAF│   (SG: 443 z Internetu)
             └─────────┬─────────┘
             ┌─────────▼─────────┐   podsieć PRYWATNA (aplikacje)
             │  serwery aplikacji │   (SG: ruch tylko z load balancera)
             └─────────┬─────────┘
             ┌─────────▼─────────┐   podsieć PRYWATNA (dane)
             │      baza danych   │   (SG: port 5432 tylko z aplikacji)
             └───────────────────┘
```

Atakujący, który przejmie serwer aplikacji, nie ma bezpośredniej drogi do innych segmentów – i odwrotnie, baza nie jest dostępna z Internetu.

## Security Group vs Network ACL

| Cecha | Security Group | Network ACL |
| :--- | :--- | :--- |
| Poziom | instancja / interfejs sieciowy | **podsieć** |
| Stanowość | **stanowa** | bezstanowa |
| Typ reguł | tylko zezwalające | zezwalające i blokujące |
| Ocena reguł | wszystkie reguły łącznie | kolejno wg numeru |
| Zastosowanie | precyzyjna kontrola ruchu per usługa | dodatkowa, zgrubna warstwa (obrona w głąb) |

## Co VPC daje w kontekście bezpieczeństwa

1. **Izolacja** od innych klientów chmury (logiczna) i od Internetu (podsieci prywatne).
2. **Mniejsza powierzchnia ataku** – tylko wybrane usługi publiczne.
3. **Kontrola ruchu** (north–south i east–west) regułami SG/NACL/firewalli.
4. **Segmentacja i mikrosegmentacja** ograniczają **ruch boczny** (lateral movement) po włamaniu.
5. **Bezpieczne połączenia hybrydowe** (VPN, łącza dedykowane).
6. **Widoczność** (flow logs, monitoring ruchu).
7. Zgodność z regulacjami (oddzielenie środowisk: produkcja/test, dane PCI).

W Kubernetes odpowiednikiem są **NetworkPolicies** i segmentacja przez przestrzenie nazw (zob. wykład, slajd 208), w service mesh – **mTLS i AuthorizationPolicy**.

## Dobre praktyki *(uzupełnienie)*

- **Bazy danych i aplikacje w podsieciach prywatnych**, publiczne tylko brzegi (load balancer, WAF),
- domyślnie **odmowa** – minimalne reguły SG, **brak `0.0.0.0/0`** dla SSH/RDP i baz,
- dostęp administracyjny przez **bastion/VPN/SSM** (nie przez otwarty SSH),
- **osobne VPC/konta** dla środowisk (prod/test) i dla danych wrażliwych,
- **PrivateLink/endpoints** dla usług zarządzanych,
- włączone **flow logs** i alerty,
- zarządzanie jako kod (Terraform) + **skanowanie IaC** i przegląd reguł.

## Podsumowanie

- **VPC** = prywatna, logicznie izolowana sieć wirtualna klienta w chmurze publicznej (podsieci, trasy, bramy, reguły ruchu).
- Służy do **izolacji, segmentacji i kontroli ruchu**; razem z **Security Groups, NACL i firewallami** realizuje zasadę najmniejszych uprawnień w sieci.
- Podsieci prywatne dla danych i aplikacji, publiczne tylko dla brzegu; mikrosegmentacja ogranicza ruch boczny.
