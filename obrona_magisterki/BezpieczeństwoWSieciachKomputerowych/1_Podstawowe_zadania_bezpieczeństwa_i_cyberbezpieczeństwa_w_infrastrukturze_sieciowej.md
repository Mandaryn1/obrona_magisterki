# Podstawowe zadania bezpieczeństwa i cyberbezpieczeństwa w infrastrukturze sieciowej

## Bezpieczeństwo a cyberbezpieczeństwo

- **Bezpieczeństwo informacji** – ochrona informacji niezależnie od formy (papier, głowy ludzi, systemy IT).
- **Bezpieczeństwo systemów/sieci komputerowych** – ochrona sprzętu, oprogramowania, danych i usług w sieci (wykład, W1, slajdy 38–41).
- **Cyberbezpieczeństwo** – ochrona systemów, sieci i danych w **cyberprzestrzeni** przed atakami (cyberprzestępcy, hacktywiści, państwa, insiderzy, APT); obejmuje też reagowanie, odporność i współpracę (CERT/CSIRT, regulacje: NIS2, RODO).

Cel: zapewnić **CIA** (wykład, W1, slajdy 26–29) oraz **uwierzytelnianie, autoryzację, rozliczalność i niezaprzeczalność** (*uzupełnienie*).

| Cecha | Znaczenie w sieci | Naruszenie (przykład) |
| :--- | :--- | :--- |
| **Poufność** | dane widzą tylko uprawnieni | podsłuch ruchu, MITM, sniffing |
| **Integralność** | dane nie są zmieniane bez zgody | ARP/DNS poisoning, modyfikacja pakietów |
| **Dostępność** | usługi działają, gdy są potrzebne | DoS/DDoS, awaria, ransomware |

Rozszerzenia modelu: **autentyczność, rozliczalność (accountability), niezaprzeczalność** (wykład, slajd 30).

## Podstawowe zadania w infrastrukturze sieciowej

| Zadanie | Opis | Mechanizmy |
| :--- | :--- | :--- |
| **1. Inwentaryzacja i klasyfikacja zasobów** | wiedza, co chronimy (urządzenia, usługi, dane); klasyfikacja wg wrażliwości | CMDB, skanowanie sieci, klasyfikacja danych |
| **2. Analiza ryzyka** | identyfikacja zagrożeń i podatności, ocena, wybór sposobu postępowania (akceptacja, unikanie, redukcja, transfer) | metodyki, normy ISO 27001/27005, NIST |
| **3. Ochrona perymetru i kontrola ruchu** | rozdzielenie sieci o różnym zaufaniu | **zapory** (filtry pakietów, stanowe, aplikacyjne, NGFW), proxy, DMZ |
| **4. Segmentacja sieci** | ograniczenie ruchu bocznego i skutków włamania | VLAN, podsieci, strefy, mikrosegmentacja |
| **5. Kontrola dostępu** | tylko uprawnione osoby i urządzenia | 802.1X/NAC, ACL, RBAC, MFA, AAA (RADIUS/TACACS+) |
| **6. Ochrona poufności i integralności transmisji** | szyfrowanie i uwierzytelnianie połączeń | TLS, IPsec, VPN, SSH, WPA2/3 |
| **7. Utwardzanie urządzeń i usług** | usunięcie zbędnych usług, bezpieczna konfiguracja | CIS Benchmarks, wyłączenie Telnet/HTTP, silne hasła |
| **8. Zarządzanie podatnościami i poprawkami** | skanowanie, łatanie, aktualizacje firmware | Nessus/OpenVAS, WSUS, patch management |
| **9. Monitorowanie i wykrywanie** | ciągła obserwacja ruchu i zdarzeń | **IDS/IPS**, NetFlow, SIEM, logi (syslog), SOC |
| **10. Reagowanie na incydenty** | procedury, izolacja, analiza, wnioski | plan IR, playbooki, SOAR, kopie dowodów |
| **11. Ciągłość i odtwarzanie** | odporność na awarie i ataki | redundancja, backup, DR (RPO/RTO) |
| **12. Zgodność i audyt** | spełnienie wymagań prawnych i norm | audyty, testy penetracyjne, ISO 27001, NIS2, RODO |
| **13. Świadomość użytkowników** | człowiek jako najsłabsze ogniwo | szkolenia, symulacje phishingu |
| **14. Zabezpieczenie fizyczne** | dostęp do szaf, kabli, portów | zamki, monitoring, blokowanie portów |

## Obrona w głąb (Defense in Depth) – wykład, slajd 54

Zasada: **każda warstwa zapewnia niezależną ochronę** – jeśli jedna zostanie przełamana, pozostałe nadal chronią zasoby.

```
 Dane           ── szyfrowanie, DLP, klasyfikacja
 Aplikacje      ── bezpieczne kodowanie, WAF, testy
 Stacje końcowe ── antywirus, EDR, kontrola urządzeń
 Sieć           ── segmentacja, monitoring, szyfrowanie
 Perymetr       ── zapory, IPS, proxy
 Tożsamość      ── MFA, PAM, governance tożsamości
 (+ fizyczna, polityki i ludzie)
```

## Rodzaje kontroli bezpieczeństwa (wykład, slajd 53)

| Klasyfikacja funkcjonalna | Implementacyjna |
| :--- | :--- |
| **zapobiegawcze** (preventive) – np. zapora, MFA | **administracyjne** – polityki, procedury, szkolenia |
| **wykrywające** (detective) – IDS, logi, SIEM | **techniczne** – sprzęt, oprogramowanie, kryptografia |
| **korygujące** (corrective) – odtworzenie, łatki | **fizyczne** – zamki, kontrola dostępu, klimatyzacja |

## Zagrożenia, którym przeciwdziała sieciowa ochrona (wykład, W1, slajdy 15–37)

- **wewnętrzne** (insider: złośliwi i nieświadomi pracownicy), **zewnętrzne** (cyberprzestępcy, hacktywiści, państwa, script kiddies, zorganizowana przestępczość), **fizyczne**,
- **malware** (wirusy, robaki, trojany, ransomware, spyware, rootkity, botnety),
- **ataki sieciowe** (DoS/DDoS, sniffing, MITM, spoofing, ARP/DNS poisoning),
- **ataki na aplikacje webowe** (SQLi, XSS, CSRF, buffer overflow, session hijacking),
- **socjotechnika** (phishing, spear phishing, pretexting, baiting, tailgating),
- **APT** – długotrwałe kampanie: rozpoznanie → pierwsze naruszenie → utrwalenie → eskalacja uprawnień → ruch boczny → eksfiltracja danych.

## Zasady przewodnie

- **najmniejsze uprawnienia** (least privilege), **separacja obowiązków**,
- **domyślna odmowa** (default deny) w zaporach,
- **Zero Trust** – „nigdy nie ufaj, zawsze weryfikuj" (wykład, W7, slajd 19), zakładanie naruszenia (*assume breach*),
- **bezpieczeństwo od projektu** (security by design) i domyślnie,
- prostota i minimalizacja powierzchni ataku,
- ciągłe doskonalenie (**PDCA**).

*(uzupełnienie)* **NIST Cybersecurity Framework 2.0**: Govern (zarządzaj), Identify (identyfikuj), Protect (chroń), Detect (wykrywaj), Respond (reaguj), Recover (odtwarzaj) – wykład, slajd 46, opisuje wcześniejszą wersję (5 funkcji bez *Govern*).

## Podsumowanie

- Cel: zapewnić **CIA** (+ uwierzytelnianie, rozliczalność, niezaprzeczalność) infrastruktury sieciowej.
- Zadania: inwentaryzacja, analiza ryzyka, ochrona perymetru, segmentacja, kontrola dostępu, szyfrowanie transmisji, utwardzanie, zarządzanie podatnościami, monitoring (IDS/IPS, SIEM), reagowanie, ciągłość działania, zgodność, edukacja, ochrona fizyczna.
- Realizacja przez **obronę w głąb** i kontrole **zapobiegawcze, wykrywające, korygujące**.
