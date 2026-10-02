# Adaptacyjne urządzenia zabezpieczające i ich rola w sieciach korporacyjnych

> Temat częściowo pokryty wykładem *Zapory i IDS*: **Cisco ASA** (slajd 25), NGFW (slajd 9), UTM (slajd 32), **Fortinet Security Fabric** (slajd 16). Opis ASA i pojęcia „adaptacyjnego bezpieczeństwa" – z własnej wiedzy ***(uzupełnienie)***.

## Znaczenie pojęcia

W przedmiocie nazwa najpewniej dotyczy rodziny **Cisco ASA – Adaptive Security Appliance** (adaptacyjne urządzenia zabezpieczające). Szerzej „adaptacyjne zabezpieczenia" to podejście, w którym urządzenia **dostosowują się** do zmieniającego się ruchu i zagrożeń (dynamiczne stany połączeń, aktualizowane reguły i sygnatury, polityki zależne od kontekstu). W opracowaniu: (A) **Cisco ASA** i jego następcy, (B) idea **adaptacyjnej architektury bezpieczeństwa**.

# A. Cisco ASA (Adaptive Security Appliance)

## Czym jest

**Cisco ASA** – wielofunkcyjne urządzenie sieciowe, które w jednym systemie łączy: **zaporę stanową**, **VPN**, **inspekcję aplikacji** i (z modułem **Firepower/FirePOWER Services**) funkcje **NGIPS i ochrony przed malware**. Wykład (slajd 25): *„sprzętowa zapora korporacyjna, obsługuje inspekcję stanową, VPN, klastrowanie i wysoką dostępność (HA); rozszerzona o moduł Cisco Firepower z funkcjami IPS i ochrony przed malware"*. Następca linii PIX (wykład, slajd 7: Cisco PIX jako zapora stanowa).

## „Adaptacyjność" – Adaptive Security Algorithm (ASA)

**Adaptive Security Algorithm** – rdzeń działania: **stanowa inspekcja** (tabela połączeń) i **poziomy bezpieczeństwa (security levels)**:

- każdy interfejs ma **nazwę (nameif)** i **poziom zaufania 0–100** (np. *outside* = 0, *dmz* = 50, *inside* = 100),
- **domyślnie ruch z wyższego poziomu do niższego jest dozwolony** (inside → outside), a **z niższego do wyższego zabroniony**, dopóki jawnie nie zezwolą na to ACL,
- ruch **powrotny** dla połączeń zainicjowanych z zaufanej strony jest przepuszczany **adaptacyjnie** (na podstawie stanu sesji),
- algorytm zarządza stanami TCP/UDP, losowaniem numerów sekwencyjnych, kontrolą anomalii.

## Funkcje ASA

| Funkcja | Opis |
| :--- | :--- |
| **Zapora stanowa** | ACL, strefy/interfejsy z poziomami zaufania, **NAT/PAT**, śledzenie stanu |
| **Tryby pracy** | **routed** (L3) i **transparent** (L2, „bump in the wire" – bez zmiany adresacji) |
| **Kontekst wielokrotny (multiple context)** | wiele **wirtualnych zapór** na jednym urządzeniu (dla działów, klientów) |
| **Modular Policy Framework (MPF)** | `class-map → policy-map → service-policy`: selektywne zastosowanie inspekcji, limitów połączeń, QoS dla klas ruchu |
| **Inspekcja aplikacyjna** | silniki dla FTP, DNS, SIP, HTTP, SMTP, ICMP itp. (obsługa dynamicznych portów, filtrowanie poleceń) |
| **VPN** | **IPsec site-to-site**, **remote access** (IKEv1/IKEv2), **SSL VPN – AnyConnect** (klient), clientless |
| **Wysoka dostępność** | **failover** Active/Standby i Active/Active (stateful – sesje przejmowane), **klastrowanie** (skalowanie) |
| **Firepower Services / FTD** | **NGIPS**, **AMP – Advanced Malware Protection**, kontrola aplikacji (AVC), filtrowanie URL, reputacja, inspekcja SSL; **FTD – Firepower Threat Defense** to zunifikowany obraz łączący ASA i Firepower |
| **Integracja z tożsamością** | AAA (RADIUS/TACACS+ – temat 5), AD, **ISE (TrustSec – SGT)** |
| **Zarządzanie** | CLI, **ASDM** (GUI), **FMC (Firepower Management Center)**, REST API, syslog/NetFlow |
| **Anty-DoS i anty-spoofing** | **TCP SYN cookies / intercept**, limity połączeń, uRPF, **threat detection** (skanowanie, ataki) |

## Wdrożenia

- **brzeg sieci** – zapora Internet ↔ sieć firmowa (z DMZ),
- **zapora wewnętrzna/centrum danych** – segmentacja,
- **koncentrator VPN** – dla pracowników zdalnych (AnyConnect) i oddziałów,
- **para HA** w trybie Active/Standby; **wirtualne (ASAv)**, w chmurze (AWS, Azure) i w kontenerach.

### Przykład konfiguracji ASA *(uzupełnienie)*

```text
interface GigabitEthernet0/0
 nameif outside
 security-level 0
 ip address 203.0.113.2 255.255.255.248
interface GigabitEthernet0/1
 nameif inside
 security-level 100
 ip address 10.0.0.1 255.255.255.0
interface GigabitEthernet0/2
 nameif dmz
 security-level 50
 ip address 172.16.0.1 255.255.255.0
!
access-list OUT-IN extended permit tcp any host 172.16.0.10 eq 443
access-group OUT-IN in interface outside
!
object network INSIDE-NET
 subnet 10.0.0.0 255.255.255.0
 nat (inside,outside) dynamic interface
!
class-map inspection_default
 match default-inspection-traffic
policy-map global_policy
 class inspection_default
  inspect dns
  inspect http
  inspect ftp
service-policy global_policy global
```

## Kierunek rozwoju

Rodzina ASA ewoluuje w stronę **Cisco Secure Firewall** (sprzęt Firepower/Secure Firewall z oprogramowaniem **ASA** lub **FTD**, zarządzanie FMC) i usług chmurowych. Kierunek rynkowy: **konwergencja zapory, IPS, VPN i kontroli aplikacji** (NGFW).

# B. Adaptacyjna architektura bezpieczeństwa i NGFW/UTM

## Idea adaptacyjnego bezpieczeństwa

Tradycyjna ochrona zakłada statyczny perymetr i stałe reguły. **Adaptacyjna architektura bezpieczeństwa** (ang. *Adaptive Security Architecture*, model analityków) to ciągły cykl:

1. **Zapobieganie (Prevent)** – zmniejszanie powierzchni ataku, blokowanie znanych zagrożeń,
2. **Wykrywanie (Detect)** – monitoring, telemetria, analityka (IDS, NDR, SIEM),
3. **Reagowanie (Respond)** – automatyczne i ręczne ograniczanie (izolacja, blokada, zmiana polityki),
4. **Przewidywanie (Predict)** – threat intelligence, uczenie maszynowe, proaktywne wzmacnianie.

Cecha „adaptacyjna": **polityka dostosowuje się do kontekstu i ryzyka** (tożsamość, postura, lokalizacja, zachowanie) oraz do **bieżących informacji o zagrożeniach** (aktualizacje sygnatur i reputacji w czasie rzeczywistym, reakcje automatyczne). Zbliżone do **Zero Trust** i **dostępu adaptacyjnego**.

## Cechy urządzeń „adaptacyjnych" (NGFW/UTM)

| Cecha | Opis (wykład) |
| :--- | :--- |
| **Konsolidacja funkcji** | zapora + IPS + antywirus + VPN + kontrola aplikacji + filtrowanie WWW + DLP (UTM – slajd 32; NGFW – slajd 9) |
| **Świadomość aplikacji i użytkownika** | polityki per aplikacja/użytkownik, nie tylko port/IP |
| **Threat intelligence i aktualizacje** | integracja z platformami (Talos itp.), automatyczne sygnatury (slajdy 4, 16) |
| **Inspekcja TLS** | analiza ruchu szyfrowanego (slajd 9) |
| **Automatyzacja i integracja** | **Fortinet Security Fabric** łączący FortiGate, FortiAnalyzer, FortiSIEM; **SIEM/SOAR/EDR** (slajd 16) |
| **Uczenie maszynowe/AI** | detekcja anomalii (slajd 4: NGFW z AI, DPI, threat intelligence) |
| **Wysoka dostępność i skalowanie** | klastrowanie, HA (ASA) |
| **Centralne zarządzanie** | Panorama, FortiManager, FMC |

## Rola w sieciach korporacyjnych

| Rola | Opis |
| :--- | :--- |
| **Ochrona perymetru** | brama Internet–firma, kontrola ruchu przychodzącego i wychodzącego (temat 3) |
| **Segmentacja wewnętrzna** | zapory wewnętrzne między strefami: użytkownicy, serwery, dane, OT/IoT |
| **VPN** | koncentrator dla pracowników zdalnych i oddziałów (temat 9) |
| **Wykrywanie i blokowanie zagrożeń** | IPS, antymalware, filtrowanie URL, sandboxing |
| **Kontrola aplikacji i danych** | zarządzanie ruchem aplikacyjnym, **DLP**, shadow IT |
| **Integracja tożsamości** | polityki oparte na użytkowniku/grupie (ISE/AD – temat 5) |
| **Źródło logów i alertów** | do SIEM/SOC (temat 7, 11) |
| **Zgodność** | wymuszanie polityk (ISO 27001, NIS2, PCI DSS – temat 2) |
| **Elastyczność wdrożenia** | sprzęt, wirtualne, chmurowe, w oddziałach i centrum danych |

## Zalety i ograniczenia

| Zalety | Ograniczenia |
| :--- | :--- |
| wiele funkcji w jednym urządzeniu, uproszczone zarządzanie, niższy TCO | **pojedynczy punkt awarii** (HA!), spadek wydajności przy pełnej inspekcji |
| polityki kontekstowe, szybka reakcja, integracja z threat intelligence | złożoność konfiguracji (fragmentacja polityk), **uzależnienie od dostawcy** |
| widoczność i centralne zarządzanie | inspekcja TLS – prywatność i prawo; wymaga licencji subskrypcyjnych |
| skalowalność (klaster, wirtualizacja) | urządzenia brzegowe jako **cel ataków** – aktualizacje i utwardzanie |

## Dobre praktyki

utwardzenie urządzenia i **zarządzanie tylko z sieci zarządzania (MFA)**, **szybkie aktualizacje**, **HA** (failover/klaster), segmentacja i **domyślna odmowa**, wąskie reguły z profilami IPS/AV/URL, logowanie do SIEM, przegląd polityk, kopie konfiguracji, testy (skan i pentest brzegu), zgodność z normami.

## Podsumowanie

- **Cisco ASA (Adaptive Security Appliance)** – korporacyjne urządzenie: **zapora stanowa + VPN (IPsec, AnyConnect) + inspekcja aplikacji + HA/klastrowanie**, z **Firepower/FTD** – IPS i antymalware; „adaptacyjność" = **Adaptive Security Algorithm** (stany połączeń, poziomy zaufania 0–100).
- Szerzej: **adaptacyjna architektura bezpieczeństwa** (zapobiegaj – wykrywaj – reaguj – przewiduj), **NGFW/UTM** z AI, threat intelligence, integracją (Fortinet Security Fabric) i polityką kontekstową.
- W sieci korporacyjnej: **ochrona perymetru, segmentacja, koncentrator VPN, IPS/malware, kontrola aplikacji, integracja tożsamości, źródło logów** – przy zapewnieniu HA i utwardzenia.

---
[⬅️ Poprzedni temat](7_Szczegółowa_inspekcja_pakietów_oraz_systemy_IDS_i_IPS_zadania_i_ograniczenia.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](9_Zastosowanie_sieci_VPN_w_bezpiecznej_komunikacji_oraz_wybrane_technologie_VPN.md)