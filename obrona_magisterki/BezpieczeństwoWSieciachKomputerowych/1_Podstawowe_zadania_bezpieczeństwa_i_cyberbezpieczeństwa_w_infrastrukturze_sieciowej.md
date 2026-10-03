# Podstawowe zadania bezpieczeństwa i cyberbezpieczeństwa w infrastrukturze sieciowej

## Bezpieczeństwo a cyberbezpieczeństwo

- **Bezpieczeństwo informacji** – ochrona informacji niezależnie od formy (papier, głowy ludzi, systemy IT).
- **Bezpieczeństwo systemów/sieci komputerowych** – ochrona sprzętu, oprogramowania, danych i usług w sieci.
- **Cyberbezpieczeństwo** – ochrona systemów, sieci i danych w **cyberprzestrzeni** przed atakami (cyberprzestępcy, hacktywiści, państwa, insiderzy, APT); obejmuje też reagowanie, odporność i współpracę (CERT/CSIRT, regulacje: NIS2, RODO).

**Bezpieczeństwo** w infrastrukturze sieciowej to ochrona sieci i przesyłanych danych tak, żeby zapewnić trzy podstawowe cele, czyli **triadę CIA**:

- **poufność:** dane widzą tylko uprawnieni,
- **integralność:** dane nie są modyfikowane przez nieuprawnionych,
- **dostępność:** usługi działają, gdy są potrzebne.

Do tego dochodzi model **AAA**: uwierzytelnianie, autoryzacja i rozliczalność (kto, co i kiedy zrobił). **Cyberbezpieczeństwo** to ujęcie szersze: ochrona systemów, sieci i danych przed zagrożeniami z cyberprzestrzeni, razem z wykrywaniem ataków i reagowaniem na nie.

## Podstawowe zadania w infrastrukturze sieciowej

- **Inwentaryzacja zasobów i analiza ryzyka:** trzeba wiedzieć, co chronimy i jakie są zagrożenia.
- **Ochrona granicy sieci (perymetru):** zapory, IPS, kontrola ruchu z zewnątrz i na zewnątrz, VPN, strefa DMZ.
- **Segmentacja:** podział na strefy (VLAN, podsieci), by ograniczyć ruch boczny po włamaniu.
- **Kontrola dostępu:** silne uwierzytelnianie, **MFA**, najmniejsze uprawnienia, 802.1X/NAC.
- **Szyfrowanie:** dane w tranzycie (TLS, IPsec, SSH, WPA3) i w spoczynku.
- **Utwardzanie urządzeń:** wyłączanie zbędnych usług, zmiana domyślnych haseł, bezpieczna konfiguracja.
- **Zarządzanie podatnościami i poprawkami:** skanowanie, aktualizacje, testy penetracyjne.
- **Monitoring i wykrywanie zagrożeń:** logi, IDS/IPS, SIEM, analiza ruchu.
- **Reagowanie na incydenty** i odtwarzanie.
- **Ciągłość działania:** kopie zapasowe, redundancja, plany awaryjne.
- **Zgodność z regulacjami i politykami:** ISO 27001, RODO, NIS2.
- **Edukacja użytkowników** i **ochrona fizyczna** urządzeń.

**Zasada nadrzędna:** **obrona w głąb**, czyli wiele niezależnych warstw zabezpieczeń, żeby awaria jednej nie dawała pełnego dostępu atakującemu. Wszystkie zadania opierają się na zasadach **najmniejszych uprawnień** i **domyślnej odmowy**.

## Rodzaje kontroli bezpieczeństwa

| Klasyfikacja funkcjonalna | Implementacyjna |
| :--- | :--- |
| **zapobiegawcze** (preventive) – np. zapora, MFA | **administracyjne** – polityki, procedury, szkolenia |
| **wykrywające** (detective) – IDS, logi, SIEM | **techniczne** – sprzęt, oprogramowanie, kryptografia |
| **korygujące** (corrective) – odtworzenie, łatki | **fizyczne** – zamki, kontrola dostępu, klimatyzacja |

## Zagrożenia, którym przeciwdziała sieciowa ochrona

- **wewnętrzne** (insider: złośliwi i nieświadomi pracownicy), **zewnętrzne** (cyberprzestępcy, hacktywiści, państwa, script kiddies, zorganizowana przestępczość), **fizyczne**,
- **malware** (wirusy, robaki, trojany, ransomware, spyware, rootkity, botnety),
- **ataki sieciowe** (DoS/DDoS, sniffing, MITM, spoofing, ARP/DNS poisoning),
- **ataki na aplikacje webowe** (SQLi, XSS, CSRF, buffer overflow, session hijacking),
- **socjotechnika** (phishing, spear phishing, pretexting, baiting, tailgating),
- **APT** – długotrwałe kampanie: rozpoznanie → pierwsze naruszenie → utrwalenie → eskalacja uprawnień → ruch boczny → eksfiltracja danych.

## Zasady przewodnie

- **najmniejsze uprawnienia** (least privilege), **separacja obowiązków**,
- **domyślna odmowa** (default deny) w zaporach,
- **Zero Trust** – „nigdy nie ufaj, zawsze weryfikuj", zakładanie naruszenia (*assume breach*),
- **bezpieczeństwo od projektu** (security by design) i domyślnie,
- prostota i minimalizacja powierzchni ataku,
- ciągłe doskonalenie (**PDCA**).

## Podsumowanie

- Cel: zapewnić **CIA** (+ uwierzytelnianie, rozliczalność, niezaprzeczalność) infrastruktury sieciowej.
- Zadania: inwentaryzacja, analiza ryzyka, ochrona perymetru, segmentacja, kontrola dostępu, szyfrowanie transmisji, utwardzanie, zarządzanie podatnościami, monitoring (IDS/IPS, SIEM), reagowanie, ciągłość działania, zgodność, edukacja, ochrona fizyczna.
- Realizacja przez **obronę w głąb** i kontrole **zapobiegawcze, wykrywające, korygujące**.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Rola_systemów_Windows_i_Linux_w_analizie_bezpieczeństwa_sieci.md)