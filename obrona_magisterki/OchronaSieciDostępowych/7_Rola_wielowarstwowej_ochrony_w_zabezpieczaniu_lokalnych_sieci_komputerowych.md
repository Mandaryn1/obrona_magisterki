# Rola wielowarstwowej ochrony w zabezpieczaniu lokalnych sieci komputerowych

> Temat częściowo pokryty: CIS Critical Security Controls (kurs Cisco, Moduł 1), obrona w głąb i Zero Trust (poprzednie wykłady: W1, W7), pięć filarów zarządzania ryzykiem (wykład ryzyko). Resztę opracowałem z własnej wiedzy ***(uzupełnienie)***.

## Idea: obrona w głąb (Defense in Depth)

**Ochrona wielowarstwowa** zakłada, że **żadne pojedyncze zabezpieczenie nie jest niezawodne** – każde można ominąć, błędnie skonfigurować lub zawiedzie. Dlatego stosuje się **wiele niezależnych, uzupełniających się warstw** kontroli; jeśli jedna zostanie przełamana, **pozostałe nadal chronią zasoby** (wykład W1). Pomysł pochodzi z fortyfikacji wojskowych; w IT: nie ma „jednej srebrnej kuli".

Powiązane zasady: **brak pojedynczego punktu awarii**, **różnorodność** zabezpieczeń (różne mechanizmy, nie tylko różni dostawcy tej samej technologii), **założenie naruszenia** (*assume breach* – Zero Trust), **najmniejsze uprawnienia**.

## Warstwy ochrony LAN

```
 ┌─ ludzie, polityki, procedury, szkolenia, ISMS ────────────────────────────┐
 │ ┌─ ochrona fizyczna (szafy, gniazda, kontrola wejść) ──────────────────┐  │
 │ │ ┌─ perymetr (NGFW, IPS, proxy, DMZ, VPN) ──────────────────────────┐ │  │
 │ │ │ ┌─ sieć wewnętrzna (segmentacja, ACL, mikrosegmentacja, IDS) ───┐│ │  │
 │ │ │ │ ┌─ dostęp do sieci (802.1X/NAC, port security, DHCP snoop.) ┐ ││ │  │
 │ │ │ │ │ ┌─ host (hardening, EDR, patch, szyfrowanie dysku) ─────┐ │ ││ │  │
 │ │ │ │ │ │ ┌─ aplikacje (WAF, bezpieczny kod) ─────────────────┐ │ │ ││ │  │
 │ │ │ │ │ │ │   DANE  (szyfrowanie, DLP, klasyfikacja, backup)  │ │ │ ││ │  │
 │ │ │ │ │ │ └───────────────────────────────────────────────────┘ │ │ ││ │  │
 └─┴─┴─┴─┴─┴───────────────────────────────────────────────────────┴─┴─┴┴─┴──┘
        + tożsamość (MFA, IAM, PAM) i monitoring (SIEM/SOC) przekrojowo
```

| Warstwa | Przykładowe środki | Co zatrzymuje |
| :--- | :--- | :--- |
| **Polityki, ludzie, organizacja** | polityki, ISMS, szkolenia, procedury, zarządzanie ryzykiem, audyty | błędy ludzkie, socjotechnika, brak zasad |
| **Fizyczna** | zamknięte szafy i serwerownie, kontrola dostępu, CCTV, wyłączone nieużywane gniazda, blokada USB | podpięcie obcych urządzeń, kradzież, sabotaż |
| **Perymetr** | zapory (stanowe, NGFW), **IPS**, WAF, proxy, DMZ, filtrowanie DNS/poczty, VPN | ataki z zewnątrz, malware, exploity |
| **Sieć wewnętrzna** | **segmentacja (VLAN, strefy)**, ACL, zapory wewnętrzne, mikrosegmentacja, NIDS, inspekcja ruchu wschód–zachód | **ruch boczny**, rozprzestrzenianie malware |
| **Dostęp do sieci (warstwa dostępu)** | **802.1X/NAC**, port security, DHCP snooping, DAI, BPDU Guard, WPA3-Enterprise, ocena postury | rogue devices, ataki L2, nieautoryzowany dostęp |
| **Host / urządzenie końcowe** | hardening, **EDR/AV**, **patch management**, zapora hostowa, szyfrowanie dysku (TPM), whitelisting aplikacji, MDM | malware, exploity, utrata urządzenia |
| **Aplikacje** | bezpieczny kod, WAF, testy SAST/DAST, ograniczone uprawnienia | SQLi, XSS, ataki aplikacyjne |
| **Dane** | szyfrowanie (w spoczynku i w tranzycie), **DLP**, klasyfikacja, **kopie zapasowe (offline)** | wyciek, ransomware |
| **Tożsamość i dostęp (przekrojowo)** | **MFA**, IAM, PAM, RBAC, least privilege, zarządzanie hasłami | przejęcie kont, eskalacja |
| **Monitoring i reagowanie (przekrojowo)** | **SIEM/SOAR**, NetFlow, IDS, SOC, plan reakcji, forensics | niewykryte ataki, wolna reakcja |

## Rodzaje kontroli w każdej warstwie

- **zapobiegawcze** (preventive) – zapora, 802.1X, MFA, szyfrowanie,
- **wykrywające** (detective) – IDS, logi, SIEM, monitoring, audyt,
- **korygujące/odtwarzające** (corrective) – łatanie, backup, izolacja, plan awaryjny,
- **odstraszające/kompensacyjne** – ostrzeżenia, monitoring kamer, dodatkowe kontrole zastępcze.

Z innej perspektywy: **administracyjne** (polityki), **techniczne**, **fizyczne**.

## CIS Critical Security Controls jako warstwowy program (kurs Cisco, Moduł 1)

Dopasowanie do dojrzałości organizacji:

| Poziom | Kontrole |
| :--- | :--- |
| **Podstawowe** (małe zasoby) | inwentaryzacja i kontrola sprzętu i oprogramowania, **ciągłe zarządzanie podatnościami**, kontrolowane uprawnienia administracyjne, **bezpieczne konfiguracje**, **analiza logów audytu** |
| **Fundamentalne** (umiarkowane zasoby) | + zabezpieczenia poczty i przeglądarek, **ochrona przed malware**, **ograniczenie i kontrola portów, protokołów i usług**, odzyskiwanie danych, bezpieczne konfiguracje urządzeń sieciowych, **ochrona granic**, ochrona danych, **kontrola dostępu wg wiedzy koniecznej**, **kontrola dostępu bezprzewodowego**, monitorowanie kont |
| **Organizacyjne** (duże zasoby) | + program świadomości i szkoleń, bezpieczeństwo aplikacji, **reagowanie na incydenty**, **testy penetracyjne i red team** |

## Dlaczego wielowarstwowość jest niezbędna w LAN

1. **Ataki są wieloetapowe** (kill chain/APT: rozpoznanie → wejście → utrwalenie → eskalacja → ruch boczny → eksfiltracja) – różne warstwy zatrzymują różne etapy.
2. **Perymetr nie wystarcza** – urządzenia mobilne, VPN, Wi-Fi, phishing i insiderzy omijają zaporę; zagrożenie **wewnątrz** sieci wymaga kontroli L2/L3, segmentacji, hostowej ochrony.
3. **Ograniczanie skutków** – segmentacja i least privilege zmniejszają „promień rażenia" naruszenia.
4. **Redundancja kontroli** – błąd konfiguracji jednej (np. zapory) nie oznacza kompromitacji całości.
5. **Różne klasy zagrożeń** – sieciowe, aplikacyjne, ludzkie, fizyczne – wymagają różnych środków.
6. **Zgodność z normami** (ISO 27001, NIS2, RODO) oczekuje podejścia warstwowego.
7. **Wykrywanie i reakcja** – nawet przy najlepszej prewencji trzeba mieć monitoring (wykład ryzyko: *pięć filarów* – identyfikacja ryzyka, środki minimalizujące, polityki, **monitoring i audyty**, edukacja).

## Przykład: scenariusz ataku i warstwy

**Atak:** phishing → pobranie malware na stację → próba ruchu bocznego do serwera plików → eksfiltracja.

| Etap | Warstwa, która może zatrzymać |
| :--- | :--- |
| e-mail z linkiem | filtr poczty/SPF-DKIM-DMARC, **szkolenia** |
| uruchomienie malware | **EDR/AV**, whitelisting aplikacji, brak uprawnień administratora |
| kontakt z C2 | filtrowanie DNS, **IPS/NGFW**, proxy |
| ruch boczny (SMB) | **segmentacja**, zapory wewnętrzne, wyłączony SMBv1, least privilege |
| próba kradzieży haseł | **MFA**, Credential Guard, ochrona przed pass-the-hash |
| eksfiltracja | **DLP**, monitoring NetFlow, alerty SIEM |
| szyfrowanie danych (ransomware) | **offline backup**, plan odtworzenia |

## Ograniczenia i pułapki

- **koszt i złożoność** (wiele narzędzi, integracja, zarządzanie polityką),
- wpływ na **wydajność** (inspekcja DPI, szyfrowanie),
- **fałszywe poczucie bezpieczeństwa** przy dużej liczbie narzędzi bez ich dostrojenia,
- **luki między warstwami** (brak integracji, nieobsługiwane przypadki),
- **warstwy zależne od siebie** (np. wszystkie oparte na jednym uwierzytelnieniu),
- potrzeba **równowagi z użytecznością**,
- konieczność **ciągłego utrzymania** (aktualizacje, przeglądy reguł, testy).

## Zasady skutecznego wdrożenia

dobór kontroli w oparciu o **analizę ryzyka**, **niezależność i różnorodność** warstw, integracja z SIEM, **testowanie** (audyty, pentesty, ćwiczenia red/blue team), dokumentacja i polityki, ciągłe doskonalenie (PDCA), edukacja użytkowników.

## Podsumowanie

- **Ochrona wielowarstwowa (defense in depth)** – wiele niezależnych warstw kontroli; awaria jednej nie oznacza kompromitacji całości.
- Warstwy: **polityki/ludzie → fizyczna → perymetr → sieć wewnętrzna (segmentacja) → dostęp (802.1X, port security) → host → aplikacje → dane**, plus **tożsamość** i **monitoring/reagowanie** przekrojowo.
- Zatrzymuje wieloetapowe ataki, ogranicza ruch boczny i skutki, kompensuje błędy pojedynczych zabezpieczeń; wymaga równowagi kosztu, wydajności i złożoności.

---
[⬅️ Poprzedni temat](6_Zagrożenia_komunikacji_bezprzewodowej_i_sposoby_jej_zabezpieczania.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Audyt_bezpieczeństwa_testy_penetracyjne_i_ocena_podatności.md)