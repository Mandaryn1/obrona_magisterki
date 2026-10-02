# Źródła, struktura i ocena alertów bezpieczeństwa w lokalnych sieciach komputerowych

> Opracowanie oparte na kursie Cisco *Cyber Threat Management* (Moduł 3: źródła informacji, threat intelligence, standardy STIX/TAXII, CVE, honeypoty; Moduł 4: CVSS, NVD; Moduł 2: SIEM) oraz wykładach o IDS/IPS; struktury alertów i metodykę triage uzupełniłem z własnej wiedzy ***(uzupełnienie)***.

## Czym jest alert bezpieczeństwa

**Alert** to **powiadomienie** wygenerowane przez system bezpieczeństwa o zdarzeniu **potencjalnie** wskazującym na naruszenie lub próbę naruszenia polityki bezpieczeństwa. Alert ≠ incydent: alert wymaga **oceny**, a dopiero potwierdzony i zakwalifikowany staje się incydentem.

## 1. Źródła alertów

### A. Źródła wewnętrzne (w sieci lokalnej)

| Źródło | Co generuje alerty |
| :--- | :--- |
| **IDS/IPS** (Snort, Suricata – NIDS/NIPS; OSSEC/Wazuh – HIDS) | dopasowanie sygnatury, anomalia ruchu, skanowanie, exploit |
| **Zapory i NGFW** | zablokowane połączenia, naruszenia polityki, wykrycia IPS, malware |
| **SIEM** | **reguły korelacyjne** łączące zdarzenia z wielu źródeł (kurs Cisco: korelacja, agregacja), UEBA |
| **EDR/antywirus** | wykrycie malware, podejrzane zachowanie procesu |
| **NAC / 802.1X / ISE** | nowe nieznane urządzenie, nieudane uwierzytelnienie, niezgodna postura |
| **Przełączniki/AP (WIDS/WIPS)** | naruszenie port security, DAI, DHCP snooping, rogue AP |
| **Monitoring przepływów (NetFlow), NBA** | odchylenia od baseline'u (kurs Cisco) |
| **Skanery podatności i narzędzia integralności** | nowe podatności, zmiany plików/konfiguracji (Tripwire) |
| **Logi systemów i usług** | wiele nieudanych logowań, zmiany uprawnień (4625, 4670), `auth.log`, auditd |
| **Honeypoty** (kurs Cisco) | **symulowane sieci/serwery**, które przyciągają atakujących; każda interakcja jest podejrzana; honeypot w chmurze izoluje od produkcji |
| **DLP, poczta (antyspam/antyphishing), proxy, DNS filtering** | wycieki, phishing, złośliwe domeny |
| **Użytkownicy i helpdesk** | zgłoszenia podejrzanych zdarzeń |

### B. Źródła zewnętrzne – threat intelligence (kurs Cisco, Moduł 3)

**Społeczności i organizacje:**

| Organizacja | Rola |
| :--- | :--- |
| **SANS** | Internet Storm Center (system wczesnego ostrzegania), NewsBites, **@RISK** (nowe wektory ataków, luki z aktywnymi exploitami), alerty Flash, czytelnia |
| **MITRE** | prowadzi listę **CVE** (identyfikatory podatności), baza **ATT&CK** (taktyki i techniki) |
| **FIRST** | zrzesza zespoły reagowania na incydenty (CSIRT) – współpraca, wymiana informacji; opiekun **CVSS** |
| **SecurityNewsWire** | portal wiadomości o alertach, exploitach i lukach |
| **(ISC)²** | edukacja i usługi kariery |
| **CIS / MS-ISAC** | całodobowe ostrzeżenia i porady, identyfikacja podatności, reagowanie na incydenty (administracja SLTT w USA) |

**Usługi analizy zagrożeń:** wymiana informacji o lukach, **IOC** i technikach łagodzenia – nie tylko z ludźmi, ale też **z systemami bezpieczeństwa**: usługi tworzą i dystrybuują **reguły zapory i IOC** do subskrybentów.

- **Cisco Talos** – jeden z największych komercyjnych zespołów threat intelligence; zbiera informacje o aktywnych i pojawiających się zagrożeniach; utrzymuje zestawy reguł dla **Snort.org, ClamAV i SpamCop**; podcasty, blogi.
- **FireEye (Mandiant)** – trójstronne podejście (inteligencja, ekspertyza, technologia); **Helix** (chmurowy SIEM/SOAR), wykrywanie zero-day bez sygnatur.
- **DHS AIS (Automated Indicator Sharing)** – bezpłatna wymiana wskaźników w czasie rzeczywistym (złośliwe IP, nadawcy phishingu) między rządem USA a sektorem prywatnym.
- *(uzupełnienie – Polska/UE)*: **CSIRT NASK, CSIRT GOV, CSIRT MON**, **CERT Polska**, ENISA, ostrzeżenia producentów.

**Raporty i wiedza:** **Cisco Annual/Mid-Year Cybersecurity Report**, blogi i podcasty; ciągłe podnoszenie umiejętności.

### C. Standardy wymiany informacji (kurs Cisco)

| Standard | Opis |
| :--- | :--- |
| **STIX** (Structured Threat Information Expression) | zestaw specyfikacji do **wymiany informacji o cyberzagrożeniach** (format) |
| **TAXII** (Trusted Automated eXchange of Indicator Information) | protokół **warstwy aplikacji (HTTPS)** do przesyłania CTI; zaprojektowany do obsługi STIX |
| **CybOX** | schematy do opisu zdarzeń i właściwości operacji sieciowych (włączony do STIX) |
| **MISP** (Malware Information Sharing Platform) | otwarta platforma wymiany IOC; wspierana przez UE, używana przez ponad 6000 organizacji; automatyczna wymiana STIX |
| **TIP** (Threat Intelligence Platform) | **centralizuje** dane z wielu źródeł i formatów, agreguje i prezentuje je zrozumiale |

**Trzy główne typy danych wywiadowczych (TIP):** **IOC** (wskaźniki kompromitacji), **TTP** (narzędzia, techniki i procedury), **informacje o reputacji** (miejsc docelowych i domen).

## 2. Struktura alertu

### Typowe pola alertu *(uzupełnienie)*

| Pole | Opis |
| :--- | :--- |
| **znacznik czasu** | kiedy zdarzenie wystąpiło (synchronizacja NTP!) |
| **sensor / źródło alertu** | który system i gdzie (np. NIDS na SPAN, host X) |
| **identyfikator reguły/sygnatury** | np. Snort **SID**, Suricata `signature_id`, reguła korelacyjna SIEM |
| **opis/komunikat i kategoria** | np. „ET SCAN Nmap SYN", klasyfikacja ataku |
| **priorytet/dotkliwość (severity)** | np. 1 (wysoki) … 3 (niski) |
| **źródło i cel** | **adresy IP i porty**, protokół, kierunek |
| **użytkownik/host/zasób** | konto, nazwa hosta, system, krytyczność aktywa |
| **zawartość** | fragment pakietu/payload, polecenie, nazwa pliku i hash |
| **zastosowana akcja** | `alert`, `drop`, `block`, `reject` |
| **odniesienia** | CVE, link do reguły, technika MITRE ATT&CK |
| **liczba wystąpień** (po agregacji) | ile razy zdarzenie się powtórzyło |

### Przykład alertu Snort (fast.log) *(uzupełnienie)*

```text
10/01-14:32:05.123456  [**] [1:1000010:1] Possible SSH brute force [**]
[Classification: Attempted Administrator Privilege Gain] [Priority: 1]
{TCP} 203.0.113.45:51234 -> 10.0.10.15:22
```

### Przykład alertu w formacie JSON (Suricata EVE) *(uzupełnienie)*

```json
{"timestamp":"2026-10-01T14:32:05.123+0200","event_type":"alert",
 "src_ip":"203.0.113.45","src_port":51234,"dest_ip":"10.0.10.15","dest_port":22,
 "proto":"TCP","alert":{"action":"allowed","signature_id":2001219,
 "signature":"ET SCAN Potential SSH Scan","category":"Attempted Information Leak","severity":2}}
```

### Alert z systemu operacyjnego

Windows Event **4625** (nieudane logowanie: konto, źródłowy IP, typ logowania, kod błędu), Linux `auth.log` (`Failed password for root from …`).

### Alert po wzbogaceniu w SIEM (kurs Cisco, Moduł 2)

SIEM dodaje **kontekst źródła**:

- **informacje o użytkowniku:** nazwa, status uwierzytelnienia, lokalizacja, grupa autoryzacji, status kwarantanny,
- **informacje o urządzeniu:** producent, model, wersja systemu, adres MAC, metoda połączenia, lokalizacja,
- **informacje o postawie (posture):** zgodność z polityką, wersja AV, poprawki, zgodność z MDM.

## 3. Ocena alertów (triage)

### Cel i etapy

Ocena odpowiada na pytania: **czy to prawdziwy atak? jak poważny? co robić i w jakiej kolejności?**

```
 alert → wstępna weryfikacja (prawda/fałsz) → wzbogacenie kontekstem → ocena wpływu i priorytetu
   → (eskalacja i reakcja) lub zamknięcie jako fałszywy alarm → dokumentacja i strojenie reguł
```

### Klasyfikacja wyników

| | **Atak rzeczywisty** | **Brak ataku** |
| :--- | :--- | :--- |
| **Alert wystąpił** | **True Positive (TP)** – poprawne wykrycie | **False Positive (FP)** – fałszywy alarm |
| **Alertu brak** | **False Negative (FN)** – przeoczony atak (najgroźniejszy) | **True Negative (TN)** – poprawny brak alertu |

Miary jakości detekcji: **precyzja** (TP/(TP+FP)), **czułość** (TP/(TP+FN)); w kursie: skanery uwierzytelnione dają mniej FP i FN.

### Pytania oceniające (kurs Cisco, Moduł 2 – SIEM)

- **Kto** jest powiązany ze zdarzeniem?
- Czy to **ważny użytkownik** z dostępem do własności intelektualnej lub poufnych informacji?
- Czy użytkownik jest **upoważniony** do dostępu do tego zasobu?
- Czy ma dostęp do **innych wrażliwych zasobów**?
- Jakiego **rodzaju urządzenie** jest używane?
- Czy zdarzenie stanowi potencjalną **kwestię zgodności**?

### Czynniki priorytetu *(uzupełnienie)*

**krytyczność aktywa**, dotkliwość i skuteczność ataku (czy zakończony sukcesem?), kierunek (z zewnątrz, **wewnętrzny/boczny**), wiarygodność źródła i sygnatury, **korelacja z innymi alertami** (np. skan → logowanie → pobieranie danych), powiązanie z **IOC/TTP** i threat intelligence, czas i wzorzec, wpływ biznesowy i prawny, kontekst (okno serwisowe, test pentestu).

### Ocena podatności CVSS (kurs Cisco, Moduł 4)

**CVSS (Common Vulnerability Scoring System)** – otwarty, **neutralny dla dostawców** standard oceny **dotkliwości podatności** (wersja 3.0; 3.1 – czerwiec 2019; opiekun: **FIRST**; opracowany m.in. przy udziale Cisco). Daje **wynik liczbowy 0–10** do określenia **pilności i priorytetu naprawy**; standaryzuje oceny i ułatwia ich porównywanie i komunikację.

**Trzy grupy metryk:**

| Grupa | Co mierzy |
| :--- | :--- |
| **Podstawowa (Base)** | cechy podatności **stałe w czasie i środowiskach**: **możliwość wykorzystania** (exploitability) i **wpływ** (CIA) |
| **Czasowa (Temporal)** | cechy zmienne w czasie, ale nie między środowiskami (dostępność exploita, poprawek, pewność raportu) – wynik maleje, gdy pojawiają się łatki i sygnatury |
| **Środowiskowa (Environmental)** | specyfika organizacji (znaczenie aktywa, istniejące zabezpieczenia) – dostrojenie wyniku do lokalnego kontekstu |

**Metryki grupy podstawowej:**

| Metryka | Skrót | Wartości | Znaczenie |
| :--- | :-: | :-: | :--- |
| **Wektor ataku** | AV | N, A, L, P | **Network, Adjacent, Local, Physical** – bliskość atakującego; im dalej (sieć), tym wyższa dotkliwość |
| **Złożoność ataku** | AC | L, H | Low/High – czynniki poza kontrolą atakującego |
| **Wymagane uprawnienia** | PR | N, L, H | poziom dostępu potrzebny do wykorzystania |
| **Interakcja z użytkownikiem** | UI | N, R | czy wymagane jest działanie użytkownika |
| **Zakres** | S | U, C | Unchanged/Changed – czy exploit wpływa na inny komponent (authority) |
| **Wpływ na poufność / integralność / dostępność** | C, I, A | H, L, N | High/Low/None |

**Ciąg wektorowy** (przykład z kursu): `CVSS:3.1/AV:N/AC:L/PR:H/UI:N/S:U/C:L/I:L/A:N` – podatność zdalna (sieć), prosta, wymaga wysokich uprawnień, bez interakcji, zakres niezmieniony, niski wpływ na C i I, brak wpływu na A. **Wynik bazowy: 3,8 (Niski)** (obliczone wg formuły CVSS 3.1).

**Skala jakościowa** (kurs Cisco):

| Ocena | Zakres |
| :--- | :--- |
| Brak (None) | 0 |
| **Niski (Low)** | 0,1–3,9 |
| **Średni (Medium)** | 4,0–6,9 |
| **Wysoki (High)** | 7,0–8,9 |
| **Krytyczny (Critical)** | 9,0–10,0 |

Kurs: *ogólnie każda podatność powyżej 3,9 powinna zostać wyeliminowana*. Wynik bazowy często dostarcza dostawca; **organizacja uzupełnia metryki środowiskowe**, aby dopasować ocenę. Przykłady *(uzupełnienie, obliczone)*: zdalne wykonanie kodu bez uwierzytelnienia `AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H` = **9,8 (krytyczny)**; lokalna eskalacja uprawnień `AV:L/AC:L/PR:L/…/C:H/I:H/A:H` = **7,8 (wysoki)**.

*(uzupełnienie)* **CVSS 4.0** (listopad 2023) zmienia grupy metryk (Base, Threat, Environmental, Supplemental) i rozszerza ocenę; w praktyce nadal spotyka się CVSS 3.1.

**Inne źródła o podatnościach (kurs Cisco):**

- **CVE** (MITRE) – słownik **wspólnych identyfikatorów** podatności (np. `CVE-2017-0144` – EternalBlue); standardowy sposób odwoływania się do luk, dostępu do poprawek; używane w logach i przez threat intelligence; **CVE Details** łączy CVE z wynikami CVSS,
- **NVD (National Vulnerability Database, NIST)** – wykorzystuje identyfikatory CVE i dodaje oceny CVSS, szczegóły techniczne, dotknięte produkty, odnośniki do zasobów.

### Priorytetyzacja alertów i podatności – wspólne zasady

Alert/podatność **pilna**, gdy: wysoki CVSS **i** krytyczny zasób **i** aktywny exploit (threat intelligence) **i** ekspozycja zewnętrzna; **niskie priorytety** – zadania planowe. Wykład ryzyko: ryzyko wysokie – natychmiastowe działania; średnie – monitorować i redukować wg planu; niskie – akceptować z nadzorem; alokacja zasobów uwzględnia krytyczność aktywów, koszt zabezpieczeń i prawdopodobieństwo.

## 4. Zarządzanie alertami w SOC *(uzupełnienie + wykład IDS/IPS)*

- **Alert fatigue** – tysiące alertów dziennie, większość fałszywych → wypalenie analityków; środki: **strojenie reguł, whitelisting, korelacja, SOAR (automatyzacja niskopoziomowych zdarzeń – kurs Cisco), priorytetyzacja AI**,
- **playbooki** – ustandaryzowane reakcje; **eskalacja** do wyższego poziomu (L1 → L2 → L3, CISO),
- **dokumentacja** (rejestr: kto, kiedy, skąd, co; post-mortem),
- **pętla zwrotna** – wnioski do strojenia reguł i threat intelligence,
- metryki: **MTTD, MTTR**, odsetek FP, pokrycie.

## Podsumowanie

- **Źródła alertów:** IDS/IPS, zapory, SIEM (korelacja), EDR, NAC/ISE, urządzenia sieciowe i logi, skanery, honeypoty oraz **zewnętrzne threat intelligence** (SANS, MITRE, FIRST, CIS, Talos, FireEye, AIS) wymieniane przez **STIX/TAXII/MISP/TIP**.
- **Struktura alertu:** czas, sensor, sygnatura/reguła, kategoria i priorytet, źródło/cel (IP, porty), użytkownik/host, payload, akcja, odniesienia (CVE, ATT&CK); **SIEM wzbogaca** o kontekst użytkownika, urządzenia i postury.
- **Ocena:** weryfikacja TP/FP/FN, pytania kontekstowe (kto, jak ważny, uprawniony?), krytyczność aktywa, korelacja, **CVSS (0–10: Niski <4, Średni, Wysoki, Krytyczny ≥9; metryki Base/Temporal/Environmental)** + CVE/NVD; priorytetyzacja i walka z alert fatigue.

---
[⬅️ Poprzedni temat](8_Audyt_bezpieczeństwa_testy_penetracyjne_i_ocena_podatności.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Systemy_zarządzania_bezpieczeństwem_informacji_w_ochronie_sieci_lokalnych.md)