# Systemy zarządzania bezpieczeństwem informacji w ochronie sieci lokalnych

> Opracowanie oparte na kursie Cisco *Cyber Threat Management* (Moduł 1: zarządzanie, polityki, role, ISO 27000, kontrole, zgodność; Moduł 4: zarządzanie ryzykiem) oraz wykładzie *Zarządzanie ryzykiem bezpieczeństwa informacji* (W11). Fragmenty prawa USA z Modułu 1 pomijam; uzupełnienia – ***(uzupełnienie)***.

## Czym jest ISMS (SZBI)

**System Zarządzania Bezpieczeństwem Informacji (SZBI, ISMS)** – uporządkowany zestaw **polityk, procesów, ról, kontroli i zasobów** do zarządzania bezpieczeństwem informacji w organizacji w sposób **ciągły i oparty na ryzyku** (kurs Cisco: ISMS składa się ze **wszystkich kontroli administracyjnych, technicznych i operacyjnych** dotyczących bezpieczeństwa informacji). Dla sieci lokalnej ISMS wyznacza **cele, zasady i odpowiedzialności**, a konkretne mechanizmy (802.1X, segmentacja, monitoring) są jego **kontrolami technicznymi**.

## Zarządzanie bezpieczeństwem IT (governance)

**Zarządzanie** określa, **kto jest upoważniony do podejmowania decyzji** dotyczących zagrożeń cyberbezpieczeństwa; zapewnia **odpowiedzialność i nadzór**, że zagrożenia są łagodzone, a strategie bezpieczeństwa **zgodne z celami biznesowymi i przepisami** (kurs Cisco).

### Role w programie zarządzania danymi (kurs Cisco)

| Rola | Zadanie |
| :--- | :--- |
| **Właściciel danych** | zapewnia zgodność z zasadami, przypisuje klasyfikację zasobów, określa kryteria dostępu |
| **Administrator danych osobowych** | określa cele i sposób przetwarzania danych osobowych (RODO: administrator) |
| **Podmiot przetwarzający** | przetwarza dane osobowe w imieniu administratora |
| **Opiekun/dozorca danych** (*custodian*) | wdraża klasyfikację i kontrole zgodnie z zasadami właściciela – odpowiada za **techniczną** kontrolę |
| **Zarządca danych** (*steward*) | dba, by dane wspierały potrzeby biznesowe i wymogi regulacyjne |
| **Inspektor ochrony danych (IOD)** | nadzoruje strategię ochrony danych |

### Kategorie pracy w cyberbezpieczeństwie (NIST NICE, kurs Cisco)

obsługa i konserwacja; ochrona i obrona; dochodzenie; zbieranie i obsługa; analiza; nadzorowanie i zarządzanie; bezpieczne udostępnianie.

## Polityki bezpieczeństwa

**Polityka cyberbezpieczeństwa** – dokument wysokiego szczebla: **wizja, cele, potrzeby, zakres i obowiązki**; demonstruje zaangażowanie kierownictwa; określa standardy zachowania i wymagania; zapewnia spójność nabywania i utrzymania systemów, sprzętu i oprogramowania; **określa konsekwencje naruszeń**; zapewnia wsparcie kierownictwa dla zespołu bezpieczeństwa (kurs Cisco).

**Rodzaje polityk:**

- **główna** (master) – strategiczny plan programu cyberbezpieczeństwa,
- **specyficzna dla systemu** – standardy dla określonych urządzeń/systemów (zatwierdzone aplikacje, konfiguracje OS, sprzęt, środki zaradcze),
- **dotycząca konkretnych zagadnień** – szczegółowe wymagania dla pewnych kwestii operacyjnych.

**Polityki szczegółowe istotne dla sieci lokalnej (kurs Cisco):**

| Polityka | Treść |
| :--- | :--- |
| **identyfikacji i uwierzytelniania** | kto ma dostęp do zasobów sieciowych i jakie procedury weryfikacji |
| **haseł** | minimalne wymagania |
| **dopuszczalnego użytkowania** | zasady dostępu do zasobów sieciowych i korzystania z nich |
| **zdalnego dostępu** | jak łączyć się z siecią wewnętrzną, jakie informacje są dostępne |
| **konserwacji sieci** | aktualizacje systemów i aplikacji |
| **obsługi incydentów** | zgłaszanie i reagowanie |
| **danych** | miejsce przechowywania, klasyfikacja, postępowanie, usuwanie |
| **poświadczeń** | zasady tworzenia poświadczeń |
| **organizacyjna** | jak wykonywać pracę w organizacji |

**Proces tworzenia polityki (wykład ryzyko):** analiza potrzeb i wymagań prawnych → konsultacje z interesariuszami → opracowanie i zatwierdzenie przez najwyższe kierownictwo → komunikacja i szkolenia → regularna aktualizacja. **Kluczowe elementy:** zakres i cele, role i odpowiedzialności, zasady dostępu i kontroli (least privilege, need-to-know), procedury zarządzania incydentami, mechanizmy egzekwowania i audytu.

*Przykład (wykład ryzyko) – polityka dostępu wg ISO 27001:* formalna autoryzacja właściciela zasobu, least privilege, kwartalna weryfikacja kont, natychmiastowe odebranie dostępu odchodzącym, **MFA dla systemów krytycznych**, logi dostępu ≥ 12 miesięcy.

## Normy i ramy: rodzina ISO/IEC 27000

**ISO/IEC 27000** – seria standardów dla ISMS (kurs Cisco): **uniwersalna struktura** stosowana w każdej organizacji.

| Norma | Treść |
| :--- | :--- |
| **ISO/IEC 27001** | **wymagania** dla ISMS (certyfikowalna): ustanowienie, wdrożenie, utrzymanie i ciągłe doskonalenie; **cele kontroli** (audyt) |
| **ISO/IEC 27002** | **zbiór kontroli (zabezpieczeń)** i wytyczne wdrożenia; w Polsce PN-ISO/IEC 27002 |
| **ISO/IEC 27005** | **zarządzanie ryzykiem** bezpieczeństwa informacji |
| **ISO 22301** | zarządzanie ciągłością działania (BCMS) |
| **ISO 31000** | zarządzanie ryzykiem (ogólne) |
| **NIST SP 800-30/37/39/53, CSF** | ocena ryzyka, ramy zarządzania ryzykiem, katalog kontroli, Cybersecurity Framework |
| **CIS Controls** | priorytetyzowane kontrole techniczne (zob. temat 7) |

### Dwanaście dziedzin (kurs Cisco)

Kurs opisuje ISO 27000 jako **dwanaście niezależnych domen** (model „równorzędny", nie warstwowy jak OSI; każda domena powiązana z innymi): **ocena ryzyka; polityka bezpieczeństwa; organizacja bezpieczeństwa informacji; zarządzanie aktywami; bezpieczeństwo zasobów ludzkich; bezpieczeństwo fizyczne i środowiskowe; zarządzanie komunikacją i operacjami; pozyskiwanie, rozwój i utrzymanie systemów; kontrola dostępu; zarządzanie incydentami; zarządzanie ciągłością działania; zgodność.**

> *(uwaga/uzupełnienie)* Układ „12 domen" odpowiada starszej wersji normy. Obecna **ISO/IEC 27001:2022 / 27002:2022** ma **93 zabezpieczenia w 4 grupach**: organizacyjne (37), osoby (8), fizyczne (14), technologiczne (34) (wcześniej, w wersji 2013 – 114 zabezpieczeń w 14 obszarach). W wykładzie ryzyko wskazano wersję PN-EN ISO/IEC 27001:2023.

### Cele kontroli i kontrole (kurs Cisco)

- **Cele kontroli (ISO 27001)** – wysokie wymagania wdrożenia kompleksowego ISMS; zawierają listę kontrolną audytu ISMS; **pozytywny audyt** = zgodność z ISO 27001 i pewność dla partnerów.
- **Kontrole (ISO 27002)** – **jak** osiągnąć cele; wytyczne wdrażania, utrzymania i doskonalenia.
- Przykład: cel – kontrola dostępu do sieci przez uwierzytelnianie użytkowników i sprzętu; kontrola – silne hasła ≥ 8 znaków (litery duże/małe, cyfry, symbole) *(dziś zaleca się dłuższe)*.
- **Oświadczenie o stosowalności (SoA/SOA)** – organizacja dobiera domeny, cele i kontrole odpowiednie do swojego środowiska i priorytetów **poufności, integralności i dostępności**.
- Kontrole dotyczą danych w **trzech stanach: w procesie, w spoczynku, w tranzycie**; odpowiedzialność różnych zespołów (sieciowy – dane w tranzycie; programiści – dane przetwarzane; wsparcie sprzętowe – dane w spoczynku).
- Role: **kierownictwo ustala politykę**, a **specjaliści IT odpowiadają za wdrożenie i konfigurację** sieci, systemów i sprzętu.

## Struktura ISO 27001 i cykl PDCA *(uzupełnienie)*

Klauzule 4–10: **kontekst organizacji, przywództwo, planowanie** (ryzyko, cele), **wsparcie**, **działanie**, **ocena wyników**, **doskonalenie**. Model **PDCA** (Plan – Do – Check – Act): planowanie (analiza ryzyka, polityka, cele, SoA) → wdrożenie (kontrole) → monitoring i audyty → działania korygujące i doskonalenie.

## Zarządzanie ryzykiem w ISMS

**Ryzyko** – związek między zagrożeniem, podatnością i charakterem organizacji; wg ISO 27005 – wpływ niepewności na realizację celów bezpieczeństwa informacji.

### Cykl zarządzania ryzykiem (NIST SP 800-39 – wykład ryzyko)

**ustanowienie kontekstu** → **identyfikacja ryzyka** (zagrożenia i podatności; **pary T-V** w kursie Cisco) → **analiza i ocena** (prawdopodobieństwo × wpływ; macierz ryzyka) → **postępowanie z ryzykiem** → **monitorowanie** (ciągle).

Pytania oceny ryzyka (kurs Cisco): kim są aktorzy zagrożeń? jakie podatności mogą wykorzystać? jak ataki wpłyną na nas? jakie prawdopodobieństwo?

**Metody analizy:** **jakościowe** (macierz ryzyka, ocena ekspertów, scenariusze) i **ilościowe** (SLE – Single Loss Expectancy, **ALE – Annual Loss Expectancy**, modele statystyczne, ROI zabezpieczeń). *Wzór (uzupełnienie):* $ALE=SLE\times ARO$ (ARO – roczna częstość zdarzeń).

### Strategie postępowania z ryzykiem (kurs Cisco, wykład ryzyko)

| Strategia | Opis |
| :--- | :--- |
| **Unikanie** | zaprzestanie działań generujących ryzyko |
| **Redukcja (mitygacja)** | zabezpieczenia techniczne i organizacyjne zmniejszające prawdopodobieństwo lub skutek (najczęstsza) |
| **Podział/transfer** | przeniesienie części ryzyka (ubezpieczenie, outsourcing, SECaaS, umowy) |
| **Zatrzymanie (akceptacja)** | świadome przyjęcie ryzyka niskiego lub zbyt kosztownego do redukcji |

**BIA** (analiza skutków biznesowych): krytyczne funkcje, **MTD, RTO, RPO**, zależności; **testy odporności i ćwiczenia** (red team, scenariusze kryzysowe). Studium przypadku: **Colonial Pipeline (2021)** – ransomware, 6 dni wyłączenia rurociągu, okup 4,4 mln USD.

## Zarządzanie aktywami, podatnościami, konfiguracją i poprawkami (kurs Cisco, Moduł 4)

Elementy ISMS realizujące kontrole techniczne w sieci: **zarządzanie aktywami** (inwentarz), **zarządzanie podatnościami** (6 etapów), **zarządzanie konfiguracją** (NIST SP 800-128), **zarządzanie poprawkami**, **zarządzanie urządzeniami mobilnymi** (zob. tematy 5, 8, 11).

## Zgodność i audyt (kurs Cisco, wykład ryzyko)

- **Dostawcy usług** mogą dostarczać organizacji **raporty atestacyjne**, np. **SOC 2** (SSAE 18 – niezależny audyt kontroli dot. bezpieczeństwa, dostępności, integralności przetwarzania, poufności i prywatności; **typ I** – w określonym momencie, **typ II** – przez ≥ 6 miesięcy); **Cloud Controls Matrix (CSA)** – 197 celów kontrolnych w 17 domenach; **CMMC** (certyfikacja DoD USA).
- **Certyfikacja ISO 27001 (wykład ryzyko):** analiza luk → wdrożenie SZBI → audyty wewnętrzne i przegląd zarządzania → **audyt certyfikacyjny etap 1** (dokumentacja) → **etap 2** (wdrożenie) → certyfikat **ważny 3 lata** → **audyty nadzoru co 12 miesięcy** → **recertyfikacja po 3 latach**. Korzyści: dowód dla klientów, przewaga w przetargach, ułatwienie zgodności z RODO i innymi, doskonalenie procesów.
- **Rodzaje audytów:** wewnętrzne, zewnętrzne, zgodności, testy penetracyjne; cykl PDCA (temat 8).

## Aspekty prawne w Polsce i UE

| Regulacja | Treść w skrócie |
| :--- | :--- |
| **RODO (GDPR)** | ochrona danych osobowych; **środki techniczne i organizacyjne** adekwatne do ryzyka, ochrona danych od projektu, **zgłaszanie naruszeń w ciągu 72 h**, DPIA; kary do 20 mln € lub 4% obrotu (wykład ryzyko) |
| **Ustawa o krajowym systemie cyberbezpieczeństwa (KSC) / NIS2** | obowiązki podmiotów kluczowych i ważnych: **zarządzanie ryzykiem**, środki techniczne i organizacyjne, zgłaszanie incydentów do CSIRT, audyty. *(uzupełnienie, sprawdzone w sieci)*: **nowelizacja KSC wdrażająca NIS2 weszła w życie 3 kwietnia 2026 r.**; podmioty mają złożyć wniosek o wpis do wykazu **do 3 października 2026 r.**, dostosować systemy i procedury **do 3 kwietnia 2027 r.**; pierwszy audyt podmiotów kluczowych do 3 kwietnia 2028 r.; kary głównie po tej dacie (sprawdź aktualny stan przepisów przed obroną) |
| **Polskie Normy** | PN-EN ISO/IEC 27001, PN-ISO/IEC 27002 |

## Etyka (kurs Cisco, Moduł 1) – skrót

Specjalista musi rozumieć prawo i interesy organizacji; podejścia etyczne: **utylitarystyczne**, **oparte na prawach** (prawo do prawdy, prywatności, bezpieczeństwa), **na wspólnym dobru**; **dziesięć przykazań etyki komputerowej** (Computer Ethics Institute); cyberprzestępczość: komputer jako **cel**, **narzędzie** lub **incydentalny** element; Konwencja o cyberprzestępczości (Rady Europy) – pierwszy międzynarodowy traktat.

## ISMS a ochrona sieci lokalnej – powiązanie praktyczne

| Element ISMS | Realizacja w LAN |
| :--- | :--- |
| polityka kontroli dostępu | 802.1X/NAC, RBAC, least privilege, MFA |
| zarządzanie aktywami | inwentarz urządzeń, wykrywanie rogue devices |
| bezpieczeństwo komunikacji i operacji | segmentacja, zapory, szyfrowanie, hardening, patching |
| bezpieczeństwo fizyczne | szafy, gniazda, kontrola dostępu |
| monitorowanie i zarządzanie incydentami | SIEM, NetFlow, IDS, SOC, plan reakcji |
| ciągłość działania | redundancja, backup, RTO/RPO |
| zgodność | audyty, testy, raporty |
| ludzie | szkolenia, NDA, onboarding/offboarding |

## Korzyści z wdrożenia ISMS

systematyczne zarządzanie ryzykiem, spójne polityki, jasne role, zgodność z prawem, **dowód dla klientów** (certyfikat), mniejsze skutki incydentów, ciągłe doskonalenie, **pięć filarów** (wykład ryzyko): identyfikacja i ocena ryzyka, środki minimalizujące, polityki i procedury, monitoring i audyty, edukacja i zaangażowanie pracowników.

## Podsumowanie

- **ISMS (SZBI)** to system **polityk, ról, procesów i kontroli** zarządzania bezpieczeństwem informacji oparty na **ryzyku** i **PDCA**; podstawa: **ISO/IEC 27001** (wymagania, cele kontroli), **27002** (kontrole), **27005** (ryzyko).
- Kluczowe elementy: **governance i role** (właściciel, opiekun, IOD), **polityki** (uwierzytelnianie, hasła, dopuszczalne użycie, zdalny dostęp, incydenty, dane), **SoA**, **zarządzanie ryzykiem** (unikanie, redukcja, transfer, akceptacja), **zarządzanie aktywami/podatnościami/konfiguracją/poprawkami**, **audyty i certyfikacja**.
- W sieci lokalnej ISMS przekłada się na **kontrole techniczne** (802.1X, segmentacja, monitoring) i zgodność (RODO, KSC/NIS2).
