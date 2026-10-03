# Adaptacyjne urządzenia zabezpieczające i ich rola w sieciach korporacyjnych

**Adaptacyjne urządzenia zabezpieczające** to najpewniej **Cisco ASA (Adaptive Security Appliance)** i pokrewne zapory nowej generacji. Ich „adaptacyjność" polega na dopasowywaniu działania do stanu połączeń i bieżącego ruchu. Szerzej oznacza to **adaptacyjną architekturę bezpieczeństwa**, w której polityki zmieniają się w zależności od kontekstu i informacji o zagrożeniach.

**Cisco ASA** to urządzenie, które w jednym systemie łączy:

- **zaporę stanową** (tabela połączeń, ACL, NAT/PAT),
- **VPN** (IPsec site-to-site, remote access i SSL VPN, np. AnyConnect),
- **inspekcję aplikacji** (FTP, DNS, SIP, HTTP),
- **wysoką dostępność** (failover Active/Standby i Active/Active, klastrowanie),
- po dodaniu modułu **Firepower/FTD** także **NGIPS, ochronę przed malware (AMP)**, kontrolę aplikacji i filtrowanie URL.

**Adaptive Security Algorithm:** każdy interfejs ma nazwę i **poziom zaufania 0–100** (np. outside = 0, DMZ = 50, inside = 100). Domyślnie ruch z wyższego poziomu do niższego jest dozwolony, a w odwrotną stronę blokowany, dopóki ACL jawnie na to nie pozwoli. Ruch powrotny jest przepuszczany na podstawie stanu sesji.

**Inne funkcje:** tryb routed i transparent, **konteksty wirtualne** (wiele wirtualnych zapór na jednym urządzeniu), **Modular Policy Framework** (selektywna inspekcja i limity dla klas ruchu), ochrona przed DoS (SYN cookies, threat detection), integracja z AAA i ISE oraz zarządzanie przez ASDM lub FMC. Rodzina ewoluuje w stronę Cisco Secure Firewall.

**Adaptacyjna architektura bezpieczeństwa** to ciągły cykl: **zapobiegaj, wykrywaj, reaguj, przewiduj**. Cechy nowoczesnych urządzeń (NGFW/UTM):

- konsolidacja funkcji (zapora, IPS, antymalware, VPN, filtrowanie, DLP),
- świadomość aplikacji i użytkownika, **inspekcja TLS**,
- integracja z **threat intelligence** i aktualizacje w czasie rzeczywistym,
- automatyzacja i integracja z SIEM/SOAR (np. Fortinet Security Fabric),
- elementy uczenia maszynowego,
- centralne zarządzanie (Panorama, FortiManager, FMC).

**Rola w sieci korporacyjnej:**

- ochrona **perymetru** (Internet–firma, DMZ),
- **segmentacja wewnętrzna** (zapory między strefami),
- **koncentrator VPN** dla pracowników zdalnych i oddziałów,
- wykrywanie i blokowanie zagrożeń (IPS, antymalware, sandboxing),
- **kontrola aplikacji i danych (DLP)**,
- integracja z tożsamością (AD, ISE) i polityki per użytkownik,
- źródło **logów i alertów** dla SIEM i SOC,
- wymuszanie **zgodności** (ISO 27001, NIS2, PCI DSS).

**Zalety:** wiele funkcji w jednym urządzeniu, centralne zarządzanie, szybka reakcja. **Ograniczenia:** pojedynczy punkt awarii (stąd HA), spadek wydajności przy pełnej inspekcji, złożoność konfiguracji, uzależnienie od dostawcy i to, że urządzenia brzegowe są częstym celem ataków, więc wymagają szybkich aktualizacji i utwardzenia.

### Dobre praktyki

utwardzenie urządzenia i **zarządzanie tylko z sieci zarządzania (MFA)**, **szybkie aktualizacje**, **HA** (failover/klaster), segmentacja i **domyślna odmowa**, wąskie reguły z profilami IPS/AV/URL, logowanie do SIEM, przegląd polityk, kopie konfiguracji, testy (skan i pentest brzegu), zgodność z normami.

## Podsumowanie

- **Cisco ASA (Adaptive Security Appliance)** – korporacyjne urządzenie: **zapora stanowa + VPN (IPsec, AnyConnect) + inspekcja aplikacji + HA/klastrowanie**, z **Firepower/FTD** – IPS i antymalware; „adaptacyjność" = **Adaptive Security Algorithm** (stany połączeń, poziomy zaufania 0–100).
- Szerzej: **adaptacyjna architektura bezpieczeństwa** (zapobiegaj – wykrywaj – reaguj – przewiduj), **NGFW/UTM** z AI, threat intelligence, integracją (Fortinet Security Fabric) i polityką kontekstową.
- W sieci korporacyjnej: **ochrona perymetru, segmentacja, koncentrator VPN, IPS/malware, kontrola aplikacji, integracja tożsamości, źródło logów** – przy zapewnieniu HA i utwardzenia.

---
[⬅️ Poprzedni temat](7_Szczegółowa_inspekcja_pakietów_oraz_systemy_IDS_i_IPS_zadania_i_ograniczenia.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](9_Zastosowanie_sieci_VPN_w_bezpiecznej_komunikacji_oraz_wybrane_technologie_VPN.md)