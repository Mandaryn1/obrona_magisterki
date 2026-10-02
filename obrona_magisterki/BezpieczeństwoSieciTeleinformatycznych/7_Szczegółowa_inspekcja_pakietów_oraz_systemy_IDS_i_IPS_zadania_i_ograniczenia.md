# Szczegółowa inspekcja pakietów w wykrywaniu i blokowaniu zagrożeń. Zadania oraz ograniczenia systemów IDS i IPS

> Opracowanie oparte na wykładzie *Zapory sieciowe i systemy wykrywania włamań* (slajdy 8–19, 22, 26–31, 33); uzupełnienia – ***(uzupełnienie)***.

## Szczegółowa inspekcja pakietów (DPI – Deep Packet Inspection)

**DPI** – analiza ruchu **wykraczająca poza nagłówki L3–L4**: obejmuje **dane warstwy aplikacji** (ładunek, payload), rozpoznawanie protokołów i śledzenie sesji. Wykład: zapory aplikacyjne dokonują *głębokiej inspekcji ruchu na poziomie protokołów aplikacyjnych*; NGFW oferują **głęboką inspekcję pakietów, zapobieganie włamaniom i świadomość aplikacji**.

### Różnica względem klasycznego filtrowania

| | Filtrowanie nagłówków | **DPI** |
| :--- | :--- | :--- |
| Analiza | IP, porty, flagi | **+ treść i protokół aplikacyjny** |
| Rozpoznaje aplikację | po porcie | **niezależnie od portu** (identyfikacja aplikacji) |
| Wykrywa | adresy/porty | **exploity, malware, SQLi/XSS, C2, DLP** |
| Koszt | niski | wysoki (CPU/ASIC, opóźnienia) |

### Techniki DPI *(uzupełnienie + wykład)*

| Technika | Opis |
| :--- | :--- |
| **dekodowanie protokołów** | HTTP, FTP, SMTP, DNS, protokoły baz danych (wykład, slajd 8) |
| **składanie strumieni i fragmentów (reassembly) i normalizacja** | zapobiega ukrywaniu ataków przez fragmentację i obfuskację; Snort – **preprocesory normalizujące protokoły** (slajd 26) |
| **dopasowanie sygnatur/wzorców** | algorytmy wielowzorcowe (Aho-Corasick), wyrażenia regularne |
| **analiza heurystyczna i behawioralna** | |
| **identyfikacja aplikacji** | sygnatury, zachowanie, **metadane TLS (SNI, JA3/JA3S)** |
| **inspekcja SSL/TLS** | rozszyfrowanie (proxy MITM) i analiza; potem ponowne szyfrowanie |
| **sandboxing plików, reputacja URL i IP** | |
| **akceleracja sprzętowa** | **FPGA/ASIC** (wykład, slajd 29) |

### Co wykrywa i blokuje DPI (wykład, slajdy 17, 31)

**SQL Injection** (wzorce `UNION`, `SELECT`, `DROP`, `' OR '1'='1`), **XSS** (tagi `<script>`, handlery zdarzeń), **command injection** (znaki `; | &`), **path traversal** (`../`, kanonikalizacja ścieżek), **exploity znanych luk** (EternalBlue/MS17-010, Shellshock, Heartbleed – sygnatury payloadów), **komunikację C2 botnetów** (podejrzane domeny i wzorce), **malware** w pobieranych plikach.

### Wyzwania DPI

- **ruch szyfrowany** – ponad 90% ruchu (wykład, slajd 27): bez inspekcji TLS brak wglądu; **TLS 1.3**, certificate pinning; malware ukrywa C2 w TLS,
- **prywatność i prawo** – przechwytywanie SSL może naruszać prywatność i regulacje (RODO) → polityka prywatności, wyłączenia (bankowość, zdrowie), zgoda,
- **wydajność** – opóźnienia, potrzeba sprzętu,
- **omijanie** – fragmentacja, kodowanie, tunelowanie (DNS, ICMP, HTTPS),
- **neutralność i etyka** (DPI operatorów).

# Systemy wykrywania włamań (IDS)

**IDS (Intrusion Detection System)** – system **monitorujący aktywność sieciową i systemową** i **generujący alerty** o podejrzanych zdarzeniach. Wykład: **kluczowy element strategii defense-in-depth**, zapewniający ciągły monitoring.

## Cechy (wykład, slajd 10)

- **monitoring pasywny** – analizuje **kopię** ruchu (SPAN/TAP), **bez wpływu** na przepływ danych,
- **generowanie alertów** – powiadomienia dla administratorów lub **SIEM**,
- **rejestrowanie zdarzeń** – szczegółowe logi do **analizy powłamaniowej** i audytu.

## Rodzaje (slajd 11)

| | **NIDS (sieciowy)** | **HIDS (hostowy)** |
| :--- | :--- | :--- |
| Zakres | ruch na kluczowych segmentach | pojedynczy host: **logi systemowe, integralność plików, zachowanie procesów** |
| Rozmieszczenie | **przełączniki SPAN/TAP** | agent na hostach |
| Analiza | czas rzeczywisty pakietów | monitoring integralności plików |
| Przykłady | **Snort, Suricata** | **OSSEC, Wazuh** |
| Zalety | pełny widok segmentu | widzi ruch po deszyfrowaniu, aktywność lokalną |
| Wady | ślepy na TLS, tylko widoczne segmenty | tylko dany host, zużycie zasobów |

*(uzupełnienie)* Dodatkowo: **NBA/NDR** (analiza zachowania sieci), **WIDS** (bezprzewodowe), **IDS aplikacji**.

## Metody wykrywania (slajd 12)

| Metoda | Zasada | Zalety | Wady |
| :--- | :--- | :--- | :--- |
| **Sygnaturowa (signature-based)** | porównanie ruchu z **bazą znanych wzorców ataków** | wysoka precyzja dla znanych ataków, **niska liczba fałszywych alarmów** | **bezradna wobec zero-day**, wymaga **regularnych aktualizacji sygnatur** |
| **Anomalii (anomaly-based)** | odchylenia od **profilu normalnej aktywności**; **ML i statystyka** | wykrywa **nowe, nieznane ataki** | **więcej fałszywych alarmów**, wymaga **okresu uczenia** |
| **Hybrydowa** | kombinacja obu | optymalne pokrycie przy akceptowalnej liczbie FP | złożoność |

*(uzupełnienie)* Również **wykrywanie oparte na regułach/politykach** i **reputacji**. Przykład reguły Snort: `alert tcp any any -> $HOME_NET 22 (msg:"SSH brute force"; flags:S; detection_filter:track by_src, count 5, seconds 60; sid:1000010;)`.

## Zadania IDS

- **szybkie wykrywanie znanych wzorców ataków** (skanowanie, brute force, exploity, DDoS, SQLi, C2 – wykład, slajd 17),
- **ciągły monitoring** (24/7) bez wpływu na wydajność,
- **alertowanie** i **integracja z SIEM/SOC**,
- **rejestracja dowodów** do analizy powłamaniowej,
- **wsparcie zgodności** (RODO, PCI DSS – wykład, slajd 13),
- **wczesne ostrzeganie** i threat hunting.

# Systemy zapobiegania włamaniom (IPS)

**IPS (Intrusion Prevention System)** – rozwinięcie IDS **z możliwością aktywnej reakcji w czasie rzeczywistym**; wykład: *„IDS wykrywa – IPS reaguje".*

## Cykl działania (slajd 14)

**Wykrywanie** (identyfikacja potencjalnych zagrożeń w ruchu) → **Analiza** (poziom zagrożenia, kontekst, źródło) → **Blokowanie** (automatyczne przerwanie złośliwej komunikacji, **blokada adresów źródłowych**) → **Rejestracja** (dokumentacja incydentu w logach).

## Rodzaje (slajd 15)

| | **NIPS** | **HIPS** |
| :--- | :--- | :--- |
| Wdrożenie | **inline** – cały ruch przepływa przez system; między routerami/przełącznikami | agent na hoście |
| Zakres | ochrona **segmentów sieci**, blokowanie pakietów „w locie" | **ochrona hosta**: kontrola procesów, **monitoring integralności pamięci**, blokowanie exploitów aplikacyjnych |
| Ryzyko | **potencjalny punkt awarii (SPOF)** | |
| Przykłady | **Cisco Firepower, Suricata w trybie IPS** | Trend Micro Deep Security, McAfee Host IPS |

## Integracja z infrastrukturą (slajd 16)

**SIEM** (centralizacja logów, korelacja), **NGFW** z modułem IPS (platforma zunifikowana), **SOAR** (automatyzacja reakcji), **EDR/XDR**, **threat intelligence** (aktualizacje sygnatur). Przykład: **Fortinet Security Fabric** (FortiGate + FortiAnalyzer + FortiSIEM).

## Wdrożenie i zarządzanie (slajdy 18–19)

1. **analiza wymagań** (infrastruktura, aktywa krytyczne, akceptowalne ryzyko),
2. **dobór rozwiązania** (open source vs komercyjne; NIDS/HIDS/NIPS; zintegrowane z NGFW),
3. **projekt architektury** (punkty monitorowania, topologia, redundancja, HA),
4. **konfiguracja reguł** (sygnatury, polityki, progi anomalii),
5. **udoskonalanie** (minimalizacja fałszywych alarmów, dostrojenie wydajności, aktualizacje sygnatur),
6. **monitorowanie i utrzymanie** (analiza alertów przez SOC, przeglądy, testy skuteczności).

Dobre praktyki: **ciągłe dostrajanie**, **whitelisting**, integracja z SIEM/SOAR/threat intelligence, szkolenia SOC (**MITRE ATT&CK**, ćwiczenia purple team), **testy penetracyjne i symulacje** (Metasploit, Caldera).

## Platformy (slajd 26)

| Platforma | Opis |
| :--- | :--- |
| **Snort** | najpopularniejszy open-source NIDS/NIPS; sygnatury, język reguł, preprocesory, tryb inline; Community i VRT rules |
| **Suricata** | nowoczesny, **wielowątkowy**, identyfikacja protokołów, inspekcja TLS, skrypty Lua, zgodny z regułami Snorta |
| **Zeek (Bro)** | monitor bezpieczeństwa: logi metadanych (`conn.log`, `http.log`, `dns.log`), threat hunting, forensics |
| **OSSEC/Wazuh** | HIDS/HIPS: integralność plików, analiza logów, rootkity |
| **Cisco Firepower**, Palo Alto, Fortinet | komercyjne NGIPS |

# Ograniczenia IDS i IPS

## Ograniczenia IDS (wykład, slajd 13)

| Ograniczenie | Opis |
| :--- | :--- |
| **brak automatycznej reakcji** | wymaga interwencji człowieka, by zablokować zagrożenie |
| **fałszywe alarmy** | wysoki poziom, zwłaszcza przy wykrywaniu anomalii |
| **ataki zero-day** | trudność wykrycia nowych zagrożeń metodą sygnaturową |
| **wydajność** | wysokie wymagania sprzętowe przy dużej przepustowości |

## Ograniczenia IPS i wspólne (wykład, slajdy 27, 29)

| Ograniczenie | Problem | Rozwiązanie |
| :--- | :--- | :--- |
| **szyfrowanie ruchu** (ponad 90% HTTPS, TLS 1.3) | **brak wglądu w treść**; malware ukrywa C2 w TLS, eksfiltracja przez kanały szyfrowane | **inspekcja SSL/TLS** (MITM proxy, zaufane certyfikaty CA na urządzeniach), analiza **metadanych TLS (JA3)**, obejście certificate pinning w środowiskach testowych; uwaga etyczno-prawna (RODO) |
| **skalowalność** | analiza >10 Gb/s wymaga potężnego sprzętu lub klastrowania | load balancing przed sensorami, architektura rozproszona, **FPGA/ASIC** |
| **wydajność vs bezpieczeństwo** | DPI powoduje opóźnienia; **IPS inline to SPOF** | **tryb bypass**, selektywna inspekcja ruchu krytycznego, cache wzorców |
| **zarządzanie politykami** | setki reguł NGFW i tysiące sygnatur → fragmentacja polityk | **zarządzanie cyklem życia polityk**, automatyczna kontrola zgodności, scentralizowane platformy (Panorama, FortiManager) |
| **alert fatigue** | tysiące alertów dziennie, większość fałszywie pozytywna → wypalenie analityków | **SOAR**, priorytetyzacja oparta na AI, dostrajanie, threat intelligence |
| **fałszywe blokady (FP w IPS)** | zablokowanie legalnego ruchu = przestój | tryb detekcji przed blokowaniem, whitelisting, testy |

*(uzupełnienie)* Inne ograniczenia: **omijanie** (fragmentacja, obfuskacja, polimorfizm, tunelowanie), **wykrywanie tylko tego, co widoczne** (rozmieszczenie czujników, segmenty niemonitorowane), **zależność od jakości sygnatur** i aktualizacji, **koszty** i wymagane kompetencje, nie zastąpi innych warstw (zapory, aktualizacje, EDR).

## IDS a IPS – porównanie

| Cecha | **IDS** | **IPS** |
| :--- | :--- | :--- |
| Tryb | pasywny (kopia ruchu, SPAN/TAP) | **inline** (w ścieżce ruchu) |
| Reakcja | **alert** | **blokada** + alert |
| Wpływ na ruch | brak | **opóźnienie, SPOF**, ryzyko fałszywej blokady |
| Fałszywy alarm | tylko zbędny alert | **zablokowanie legalnego ruchu** |
| Zastosowanie | monitoring, forensics, zgodność | aktywna ochrona |
| Wdrożenie | łatwiejsze | wymaga HA/bypass, testów |

## Podsumowanie

- **DPI** analizuje ładunek i protokół aplikacyjny (L7): wykrywa SQLi, XSS, exploity, C2, malware; wyzwania: ruch szyfrowany (inspekcja TLS), wydajność, prywatność.
- **IDS** (pasywny, NIDS/HIDS, sygnatury/anomalie/hybryda) – alerty i logi; **IPS** (inline, NIPS/HIPS) – blokada w czasie rzeczywistym; „IDS wykrywa – IPS reaguje".
- **Ograniczenia:** fałszywe alarmy i alert fatigue, zero-day, szyfrowanie, wydajność i skalowalność, SPOF IPS, fragmentacja polityk; środki: dostrajanie, SOAR, inspekcja TLS, bypass, HA, integracja z SIEM i threat intelligence.
