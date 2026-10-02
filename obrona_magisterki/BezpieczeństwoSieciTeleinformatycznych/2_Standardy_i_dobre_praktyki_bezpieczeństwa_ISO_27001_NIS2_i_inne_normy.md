# Standardy i dobre praktyki bezpieczeństwa: ISO 27001, NIS2 oraz inne wybrane normy

> Opracowanie oparte na slajdach o ISO 27001/27002/SZBI/NIST CSF (wykład *Mechanizmy bezpieczeństwa*, W1:45–51), wykładzie o ryzyku (ISO 27005, 22301, KSC), W4 (PCI DSS, RODO, HIPAA, FedRAMP, GLBA) i kursie Cisco (ISO 27000, CIS Controls). NIS2 i pozostałe – ***(uzupełnienie)***; terminy polskiej ustawy **sprawdziłem w sieci**.

## Po co standardy

Standardy i dobre praktyki dają **wspólny język**, listę sprawdzonych zabezpieczeń, podstawę **audytu i certyfikacji**, a często są **wymogiem prawnym lub umownym**. Wykład: normy to *fundament nowoczesnego bezpieczeństwa informacji*; wdrożenie SZBI zapewnia **zaufanie rynkowe, zgodność prawną, redukcję ryzyka, kulturę bezpieczeństwa i szybką reakcję**.

Podział:

| Rodzaj | Przykłady |
| :--- | :--- |
| **normy zarządcze** (systemy zarządzania) | ISO/IEC 27001 (ISMS), ISO 22301 (ciągłość) |
| **wytyczne i katalogi zabezpieczeń** | ISO/IEC 27002, NIST SP 800-53, **CIS Controls** |
| **zarządzanie ryzykiem** | ISO/IEC 27005, ISO 31000, NIST SP 800-30/37/39 |
| **ramy (frameworks)** | **NIST Cybersecurity Framework**, COBIT |
| **regulacje prawne** | **NIS2/KSC**, **RODO**, DORA, ustawa o ochronie danych osobowych |
| **standardy branżowe** | **PCI DSS** (karty płatnicze), HIPAA (zdrowie, USA), IEC 62443 (przemysł), SOC 2 |
| **testowanie** | NIST SP 800-115, OWASP WSTG, PTES, OSSTMM (temat 10) |

# 1. ISO/IEC 27001 – System Zarządzania Bezpieczeństwem Informacji (SZBI/ISMS)

**ISO/IEC 27001:2022** (polska: **PN-EN ISO/IEC 27001:2023**) – międzynarodowa norma określająca **wymagania** dla ISMS; **certyfikowalna** (wykład: *systemowe podejście do zarządzania bezpieczeństwem, cykl PDCA, podejście oparte na ryzyku, możliwość certyfikacji*).

## Cykl PDCA (wykład, slajd 45)

| Faza | Treść |
| :--- | :--- |
| **Plan** | planowanie ISMS (kontekst, ryzyko, cele, SoA) |
| **Do** | implementacja kontroli |
| **Check** | monitorowanie skuteczności (audyty, przeglądy) |
| **Act** | doskonalenie systemu |

## Fundament SZBI – klauzule normy (wykład, slajd 50)

1. **Kontekst organizacji** – zakres SZBI, potrzeby stron zainteresowanych, wymagania prawne i biznesowe,
2. **Przywództwo** – zaangażowanie kierownictwa, **polityka bezpieczeństwa informacji**, role i odpowiedzialności,
3. **Planowanie** – **ocena ryzyka** bezpieczeństwa informacji, cele, plan działań,
4. **Wsparcie** – zasoby, kompetencje, świadomość, komunikacja, udokumentowana informacja,
5. **Operacje** – wdrożenie procesów i kontroli,
6. **Ocena i doskonalenie** – monitorowanie, pomiar, **audyty wewnętrzne**, **przeglądy zarządcze**, ciągłe doskonalenie.

## Proces wdrażania (wykład, slajd 51)

analiza kontekstu → **ocena ryzyka** (zagrożenia, podatności, wpływ, prawdopodobieństwo, akceptowalny poziom) → dokumentacja (polityki, procedury, instrukcje) → **szkolenia i świadomość** → monitorowanie i doskonalenie (audyty wewnętrzne, przeglądy, pomiar skuteczności).

## Załącznik A i ISO/IEC 27002

- **ISO/IEC 27002:2022** (PN-EN ISO/IEC 27002:2023-01) – wytyczne wdrażania zabezpieczeń (wykład: *praktyczny przewodnik wdrażania zabezpieczeń w ramach SZBI; integracja cyberbezpieczeństwa z ochroną prywatności*).
- ***(uzupełnienie)*** Wersja 2022 ma **93 zabezpieczenia w 4 grupach**: **organizacyjne (37)**, **dotyczące osób (8)**, **fizyczne (14)**, **technologiczne (34)** (wersja 2013: 114 zabezpieczeń w 14 obszarach). Kurs Cisco opisuje starszy układ „12 domen" (m.in. ocena ryzyka, polityka bezpieczeństwa, organizacja, zarządzanie aktywami, zasoby ludzkie, bezpieczeństwo fizyczne, zarządzanie komunikacją i operacjami, kontrola dostępu, incydenty, ciągłość działania, zgodność).
- **Oświadczenie o stosowalności (SoA)** – które zabezpieczenia stosuje organizacja i dlaczego.
- Zabezpieczenia istotne dla sieci teleinformatycznych: **bezpieczeństwo sieci, segregacja sieci, filtrowanie WWW, bezpieczeństwo usług sieciowych, kontrola dostępu, kryptografia, logowanie i monitorowanie, zarządzanie podatnościami, zarządzanie konfiguracją, kopie zapasowe, redundancja**.

## Certyfikacja *(wykład ryzyko + uzupełnienie)*

analiza luk → wdrożenie SZBI → audyty wewnętrzne i przegląd zarządzania → **audyt certyfikacyjny etap 1 (dokumentacja) i etap 2 (wdrożenie)** → certyfikat **ważny 3 lata** → audyty nadzoru co 12 miesięcy → recertyfikacja. Korzyści: dowód dla klientów, wymóg w przetargach, ułatwienie zgodności z RODO i innymi, doskonalenie procesów.

## Pokrewne normy z rodziny 27000

| Norma | Treść |
| :--- | :--- |
| **ISO/IEC 27005** | zarządzanie ryzykiem bezpieczeństwa informacji (identyfikacja, analiza, ocena, postępowanie) |
| **ISO 22301** | zarządzanie ciągłością działania (BCMS) |
| **ISO/IEC 27017 / 27018** *(uzup.)* | bezpieczeństwo usług chmurowych / ochrona danych osobowych w chmurze publicznej |
| **ISO/IEC 27035** *(uzup.)* | zarządzanie incydentami |
| **ISO/IEC 27701** *(uzup.)* | rozszerzenie dla zarządzania prywatnością |
| **ISO 31000** | zarządzanie ryzykiem (ogólne) |

# 2. NIS2 – dyrektywa UE o cyberbezpieczeństwie *(uzupełnienie)*

**NIS2 – Dyrektywa (UE) 2022/2555** w sprawie środków na rzecz wysokiego wspólnego poziomu cyberbezpieczeństwa w Unii (następczyni dyrektywy NIS z 2016 r.). W 2026 r. kontynuacja wdrażania w prawie krajowym. W slajdach wykładu NIS2 wymieniono jako element wymogów regulacyjnych (*GDPR, AI Act, NIS2*).

## Zakres

- **Podmioty kluczowe** i **ważne** w sektorach o wysokiej krytyczności (energetyka, transport, bankowość, infrastruktura rynków finansowych, ochrona zdrowia, woda pitna i ścieki, **infrastruktura cyfrowa**, zarządzanie usługami ICT, administracja publiczna, przestrzeń kosmiczna) i innych krytycznych (poczta, gospodarka odpadami, chemikalia, żywność, produkcja, **dostawcy usług cyfrowych**, badania).
- Zasada **wielkości** (duże i średnie podmioty) + wyjątki; **samoidentyfikacja** – podmiot sam ustala, czy podlega przepisom.

## Główne obowiązki

| Obszar | Treść |
| :--- | :--- |
| **Zarządzanie ryzykiem (art. 21)** | **środki techniczne, operacyjne i organizacyjne** proporcjonalne do ryzyka, m.in.: polityki analizy ryzyka i bezpieczeństwa systemów; **obsługa incydentów**; **ciągłość działania** (kopie zapasowe, odtwarzanie, zarządzanie kryzysowe); **bezpieczeństwo łańcucha dostaw**; bezpieczeństwo nabywania, rozwoju i utrzymania systemów, **zarządzanie podatnościami**; ocena skuteczności środków; **higiena cyberbezpieczeństwa i szkolenia**; **kryptografia i szyfrowanie**; bezpieczeństwo kadr, **kontrola dostępu**, zarządzanie aktywami; **MFA/uwierzytelnianie ciągłe** i bezpieczna komunikacja |
| **Zgłaszanie incydentów (art. 23)** | **wczesne ostrzeżenie w ciągu 24 h**, **powiadomienie o incydencie w ciągu 72 h**, **raport końcowy do miesiąca** (do CSIRT/organu) |
| **Odpowiedzialność kierownictwa** | **organy zarządzające zatwierdzają środki i odpowiadają** za ich nadzór; szkolenia kadry zarządzającej |
| **Nadzór i sankcje** | audyty, kontrole; kary dla podmiotów kluczowych do **10 mln € lub 2% rocznego obrotu**, dla ważnych do **7 mln € lub 1,4%** (wyższa z kwot) |
| **Współpraca** | CSIRT, organy właściwe, wymiana informacji |

## NIS2 w Polsce – nowelizacja ustawy o KSC *(sprawdzone w sieci)*

NIS2 wdrożono w Polsce **nowelizacją ustawy o krajowym systemie cyberbezpieczeństwa (KSC)**, która **weszła w życie 3 kwietnia 2026 r.** Harmonogram:

| Termin | Obowiązek |
| :--- | :--- |
| **3 października 2026 r.** | złożenie wniosku o **wpis do wykazu** podmiotów kluczowych i ważnych (6 miesięcy od wejścia w życie) |
| **3 kwietnia 2027 r.** | **dostosowanie systemów, procedur i struktur** do wymagań (12 miesięcy) |
| **3 kwietnia 2028 r.** | pierwszy **audyt cyberbezpieczeństwa** podmiotów kluczowych (potem co najmniej co 3 lata); kary głównie po tym okresie przejściowym |

Zakres podmiotów znacznie szerszy niż w ustawie z 2018 r. (z kilkuset do kilkudziesięciu tysięcy). Przed obroną **zweryfikuj aktualny stan** przepisów i interpretacji.

# 3. Pozostałe wybrane normy i ramy

## NIST Cybersecurity Framework (CSF)

Wykład (W1:46): **5 funkcji** – **Identify** (zasoby, zagrożenia, podatności), **Protect** (zabezpieczenia), **Detect** (wykrywanie), **Respond** (reagowanie), **Recover** (odtwarzanie) + **profil organizacyjny** dopasowany do priorytetów i tolerancji ryzyka. ***(uzupełnienie)*** **CSF 2.0 (2024)** dodaje szóstą funkcję **Govern** (zarządzanie). Inne publikacje NIST: SP 800-30/37/39 (ryzyko), **SP 800-53** (katalog zabezpieczeń), **SP 800-115** (testowanie – temat 10), SP 800-61 (reagowanie na incydenty).

## CIS Critical Security Controls (kurs Cisco)

Priorytetyzowane zabezpieczenia: **podstawowe** (inwentaryzacja, zarządzanie podatnościami, uprawnienia administracyjne, bezpieczne konfiguracje, logi), **fundamentalne** (poczta/przeglądarki, malware, **kontrola portów i usług**, bezpieczne konfiguracje urządzeń sieciowych, **ochrona granic**, kontrola dostępu bezprzewodowego), **organizacyjne** (szkolenia, reagowanie na incydenty, **testy penetracyjne i red team**). Plus **CIS Benchmarks** do utwardzania.

## PCI DSS (W4)

Standard dla **organizacji przetwarzających, przechowujących lub przesyłających dane kart płatniczych**; obejmuje wszystkie komponenty, jeśli występuje **PAN**. Wymaga m.in. **izolacji sieci (segmentacji)**, silnego zarządzania hasłami, **szyfrowania PAN**, nieprzechowywania danych uwierzytelniających po autoryzacji, regularnych **testów penetracyjnych i skanów podatności** (zewnętrzne skany przez ASV). Kary i utrata możliwości przyjmowania kart. Aktualna wersja: **PCI DSS 4.0.1** *(uzup.)*.

## RODO (GDPR) – W4, wykład ryzyko

Ochrona danych osobowych; **środki techniczne i organizacyjne adekwatne do ryzyka**, ochrona od projektu, **zgłaszanie naruszeń w ciągu 72 h**, DPIA; kary do **20 mln € lub 4% obrotu**. W sieciach: szyfrowanie, kontrola dostępu, minimalizacja, logowanie.

## Inne *(uzupełnienie)*

| Regulacja/standard | Zakres |
| :--- | :--- |
| **DORA** (rozp. UE 2022/2554) | odporność operacyjna sektora finansowego (stosowana od 17.01.2025) |
| **eIDAS** | usługi zaufania, podpis elektroniczny |
| **Akt o odporności cybernetycznej (CRA)** | cyberbezpieczeństwo produktów z elementami cyfrowymi |
| **IEC 62443** | bezpieczeństwo systemów automatyki przemysłowej (OT/ICS) |
| **SOC 2 (SSAE 18)** | niezależna atestacja kontroli usługodawców (typ I – w momencie, typ II – ≥ 6 mies.) |
| **HIPAA, FedRAMP, GLBA, SOX, FISMA** (USA, W4) | ochrona zdrowia, chmura rządowa, finanse; **GLBA i NY DFS wymagają okresowych testów penetracyjnych** |
| **Krajowe Ramy Interoperacyjności (KRI)** *(Polska)* | wymagania dla systemów administracji publicznej, w tym zarządzanie bezpieczeństwem informacji |
| **COBIT, ITIL** | zarządzanie IT |
| **Cloud Controls Matrix (CSA)** | 197 celów kontrolnych w 17 domenach dla chmury (kurs Cisco) |

## Wspólne cechy i zależności

- Wszystkie opierają się na **zarządzaniu ryzykiem**, **politykach i rolach**, **kontroli dostępu**, **monitoringu i reagowaniu**, **ciągłości działania**, **szkoleniach**, **audycie**.
- **ISO 27001 jako „parasol"**: wdrożony ISMS ułatwia spełnienie NIS2, RODO, wymogów branżowych (**mapowanie standardów na wymagania ustawowe** – resorty publikują mapowania).
- Różnica: **ISO 27001 – dobrowolna, certyfikowalna**; **NIS2/RODO – obowiązek prawny z karami**; **PCI DSS – wymóg umowny**; **NIST CSF/CIS – dobre praktyki**.

## Porównanie ISO 27001 i NIS2

| | **ISO 27001** | **NIS2 / KSC** |
| :--- | :--- | :--- |
| Charakter | norma dobrowolna | **prawo (dyrektywa → ustawa)** |
| Zakres | dowolna organizacja | wskazane sektory i wielkości |
| Podejście | system zarządzania, ryzyko, PDCA | minimalne środki (art. 21), zgłaszanie incydentów, odpowiedzialność zarządu |
| Weryfikacja | audyt certyfikujący | nadzór organu, audyty, kary |
| Zgłaszanie incydentów | nie wymusza zewnętrznego | **24 h / 72 h / miesiąc** |

## Podsumowanie

- **ISO/IEC 27001** – wymagania dla SZBI (PDCA, klauzule 4–10, SoA) i **27002** (93 zabezpieczenia w 4 grupach); certyfikacja 3-letnia z nadzorem rocznym.
- **NIS2** – dyrektywa UE: środki zarządzania ryzykiem (art. 21), **zgłaszanie incydentów 24 h/72 h/1 mies.**, odpowiedzialność zarządu, kary; w Polsce wdrożona nowelizacją KSC (**w życie 3.04.2026**; wpis do wykazu do 3.10.2026, dostosowanie do 3.04.2027).
- Inne: **NIST CSF** (Identify–Protect–Detect–Respond–Recover; 2.0 + Govern), **CIS Controls**, **PCI DSS**, **RODO**, ISO 27005/22301/27017, DORA, IEC 62443, SOC 2.
- Standardy uzupełniają się i wspólnie budują podejście oparte na ryzyku.
