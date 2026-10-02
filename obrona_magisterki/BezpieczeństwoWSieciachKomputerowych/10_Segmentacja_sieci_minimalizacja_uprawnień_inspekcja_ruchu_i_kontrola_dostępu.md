# Segmentacja sieci, minimalizacja uprawnień, inspekcja ruchu i kontrola dostępu w ograniczaniu skutków ataków

> Opracowanie oparte m.in. na wykładach: W7 (segmentacja, mikrosegmentacja, Zero Trust, slajdy 18–19), *Zapory/IDS*, W8 (najmniejsze uprawnienia, slajdy 6–7), W1 (obrona w głąb, APT); uzupełnienia oznaczone ***(uzupełnienie)***.

## Idea wspólna: ograniczanie „promienia rażenia" (blast radius)

Nie da się zagwarantować, że atak nigdy się nie powiedzie – dlatego należy zakładać naruszenie (*assume breach* – Zero Trust, wykład W7, slajd 19) i **ograniczać jego skutki**. Cztery mechanizmy wzajemnie się uzupełniają:

| Mechanizm | Pytanie, na które odpowiada | Efekt |
| :--- | :--- | :--- |
| **Segmentacja** | *gdzie* atakujący może dotrzeć? | ogranicza **ruch boczny** |
| **Minimalizacja uprawnień** | *co* może zrobić po przejęciu konta/procesu? | ogranicza **skalę szkód** |
| **Inspekcja ruchu** | *co* płynie między segmentami? | **wykrywa i blokuje** złośliwy ruch |
| **Kontrola dostępu** | *kto* i *na jakich zasadach* ma dostęp? | **odmawia** nieuprawnionym |

## 1. Segmentacja sieci

**Segmentacja** – podział sieci na mniejsze, logicznie lub fizycznie odseparowane strefy, między którymi ruch kontroluje się zaporą (wykład W7, slajd 18).

### Poziomy segmentacji

| Poziom | Opis |
| :--- | :--- |
| **Makrosegmentacja** (wykład) | podział na duże strefy o różnym poziomie zaufania: **DMZ/publiczna** (serwery WWW, load balancery), **warstwa aplikacyjna**, **warstwa danych** (bazy), **strefa zarządzania** (jump hosty, bastiony) |
| **Mikrosegmentacja** (wykład) | granularna kontrola na poziomie **pojedynczych obciążeń/aplikacji**; każdy zasób z własną polityką (**biała lista**); w kontenerach **Network Policies**, w chmurze **Security Groups/NSG** |
| **Techniki** *(uzupełnienie)* | **VLAN** + routing/ACL/zapora międzysegmentowa, osobne podsieci IP, **Private VLAN**, strefy zapory (inside/outside/DMZ), VRF, fizyczna separacja (air gap) dla sieci krytycznych, ZTNA |

### Przykładowa architektura stref

```
 Internet ─▶ [zapora zewnętrzna/NGFW] ─▶ DMZ (WWW, proxy, poczta)
                       │
                [zapora wewnętrzna]
        ┌──────────────┼───────────────┬──────────────┐
   sieć użytkowników  serwery aplikacyjne  strefa danych  zarządzanie
       (VLAN 10)         (VLAN 20)         (VLAN 30)       (VLAN 99)
   + goście (VLAN 50, tylko Internet)   + IoT/OT (VLAN 60, odizolowane)
```

### Korzyści

- **ograniczenie ruchu bocznego** (APT: etap *lateral movement* – wykład, W1, slajd 37) – przejęcie serwera WWW nie daje bezpośredniego dostępu do bazy,
- **zmniejszenie powierzchni ataku** i widoczności sieci,
- **izolacja awarii i malware** – lekcja **WannaCry**: *segmentacja sieci, aktualizacje, kopie offline* (wykład, slajd 35),
- łatwiejszy **monitoring** (kontrolowane punkty przejścia = miejsca inspekcji),
- spełnienie wymagań zgodności (PCI DSS: wydzielenie strefy danych kartowych).

### Zasada w polityce zapory

Ruch między strefami: **domyślna odmowa (default deny)**, jawnie zezwolone tylko niezbędne przepływy (np. WWW→aplikacja:8080, aplikacja→baza:5432). Wykład (W7, slajd 18): segmentacja oparta na zasadzie **białej listy**.

## 2. Minimalizacja uprawnień (zasada najmniejszych uprawnień)

**Least privilege** (wykład W8, slajd 6): każdy użytkownik, proces i aplikacja ma dostęp **wyłącznie do zasobów niezbędnych** do zadań. Wdrożenie: precyzyjna identyfikacja potrzeb, **regularne przeglądy uprawnień**, **audyt aktywności** i wykrywanie anomalii.

### Zastosowania

| Obszar | Przykłady |
| :--- | :--- |
| **konta użytkowników** | zwykłe konta zamiast administratora; **PAM** dla kont uprzywilejowanych; Linux: `sudo` zamiast root, wyłączone logowanie root (wykład W8, slajd 13) |
| **procesy i usługi** | konta usługowe o minimalnych uprawnieniach, SELinux/AppArmor, kontenery nie jako root |
| **sieć (zapory)** | tylko niezbędne porty i adresy (default deny) |
| **dane i bazy** | RBAC, ACL, osobne konta dla usług, `GRANT` minimalne |
| **urządzenia sieciowe** | uprawnienia administracyjne wg ról (TACACS+/RADIUS), poziomy uprawnień |
| **Zero Trust** (wykład, W7, slajd 19) | **just-in-time / just-enough access**, ciągła weryfikacja, kontekstowa autoryzacja |

**Efekt:** przejęte konto lub usługa daje ograniczone szkody (nie ma „kluczy do królestwa"), utrudniona **eskalacja uprawnień**.

## 3. Inspekcja ruchu

**Inspekcja** – analiza ruchu przechodzącego między strefami i na brzegu w celu **wykrycia lub zablokowania** zagrożeń.

| Mechanizm | Zakres (wykład, Zapory/IDS) |
| :--- | :--- |
| **Filtracja pakietów** | adresy IP, porty, protokół, flagi TCP (L3–4) – szybka, bez analizy treści |
| **Zapory stanowe** | tabela stanów, dynamiczne reguły dla odpowiedzi (iptables/conntrack, Cisco ASA) |
| **Zapory aplikacyjne/WAF** | inspekcja HTTP/HTTPS, FTP, SMTP, DNS; SQLi, XSS, command injection, directory traversal |
| **NGFW** | zapora + **IPS**, kontrola aplikacji niezależnie od portu, antymalware, **inspekcja SSL/TLS** |
| **IDS/IPS** | wykrywanie/blokowanie (sygnatury, anomalie); inline IPS, NIDS na SPAN/TAP |
| **Inspekcja ruchu wschód–zachód** (Zero Trust, wykład W7) | kontrola ruchu **wewnątrz** sieci, nie tylko na perymetrze |
| **UTM** | wiele funkcji w jednym urządzeniu (zapora, IPS, AV, VPN, filtrowanie WWW, DLP) |
| **Proxy, filtrowanie DNS/URL, sandboxing** | blokada złośliwych domen i plików |

**Ograniczenia:** szyfrowanie (potrzebna inspekcja TLS z zachowaniem prawa/prywatności), wydajność (DPI → opóźnienia), SPOF (wykład, slajdy 27, 29).

## 4. Kontrola dostępu

Kto i do czego ma dostęp – na kilku poziomach:

- **sieć:** 802.1X/NAC, VPN z MFA, ACL, reguły zapory,
- **system:** ACL, RBAC, MAC (SELinux), polityki GPO,
- **aplikacja:** uwierzytelnianie, autoryzacja, sesje, tokeny,
- **dane:** szyfrowanie, klasyfikacja, DLP.

## Jak razem ograniczają skutki ataku – scenariusz

**Atak:** phishing → przejęcie stacji pracownika → próba ruchu bocznego do serwera plików i bazy.

| Mechanizm | Zadziała jako… |
| :--- | :--- |
| **MFA / kontrola dostępu** | utrudnia użycie skradzionego hasła |
| **Minimalne uprawnienia** | konto nie jest administratorem, nie czyta danych innych działów |
| **Segmentacja** | stacja użytkownika **nie ma trasy** do strefy danych; ruch SMB/RDP między stacjami zablokowany |
| **Inspekcja (IPS, NGFW, NIDS)** | wykrywa skanowanie, próby exploitów (np. EternalBlue), komunikację C2 |
| **Monitoring (SIEM/SOC)** | alert o anomalii → izolacja hosta |
| **Kopie zapasowe offline** | odtworzenie po ransomware |

Efekt: incydent ograniczony do **jednej stacji**, a nie całej organizacji (inaczej niż w WannaCry/NotPetya).

## Podsumowanie

- Cztery uzupełniające się mechanizmy: **segmentacja** (gdzie można dotrzeć), **minimalne uprawnienia** (co można zrobić), **inspekcja** (co płynie), **kontrola dostępu** (kto wchodzi).
- Wspólna zasada: **domyślna odmowa i biała lista** + założenie naruszenia (**Zero Trust**) → ograniczenie ruchu bocznego i skali szkód.
- Wdrożenie: strefy (DMZ, aplikacje, dane, zarządzanie), mikrosegmentacja, least privilege/PAM, zapory stanowe i NGFW/IPS, 802.1X i MFA, monitoring.
