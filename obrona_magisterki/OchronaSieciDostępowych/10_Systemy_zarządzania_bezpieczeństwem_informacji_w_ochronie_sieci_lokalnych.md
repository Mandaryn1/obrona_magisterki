# Systemy zarządzania bezpieczeństwem informacji w ochronie sieci lokalnych

**System Zarządzania Bezpieczeństwem Informacji** z ang. *Information Security Management System* **(SZBI / ISMS)** to **uporządkowany zestaw polityk, procesów, ról i zabezpieczeń**, dzięki któremu organizacja systematycznie zarządza ryzykiem dla informacji. Odpowiada na pytanie, kto podejmuje decyzje i jak się je wdraża, a nie tylko jakie urządzenia zastosować. W ochronie sieci lokalnych zapewnia, że zabezpieczenia techniczne (zapory, 802.1X, segmentacja) są oparte na analizie ryzyka, udokumentowane, utrzymywane i weryfikowane.

**Główne elementy:**

- **Role:** właściciel zasobu, administrator, podmiot przetwarzający, zarządca, **IOD** (inspektor ochrony danych), kierownictwo.
- **Polityki:** główna polityka bezpieczeństwa, polityki szczegółowe (identyfikacji i haseł, dopuszczalnego użycia, dostępu zdalnego, obsługi incydentów, danych), procedury i instrukcje.
- **Zarządzanie ryzykiem:** identyfikacja aktywów i par zagrożenie–podatność, **analiza wpływu na biznes (BIA)**, macierz ryzyka, wybór strategii: **unikanie, redukcja, transfer lub akceptacja ryzyka**.
- **Zabezpieczenia i ich wybór:** zgodnie z normami, uwzględniając stany danych (w przetwarzaniu, spoczynku, tranzycie).
- **Cykl PDCA:** planuj, wdrażaj, sprawdzaj (audyty, przeglądy), doskonal.

**Normy:**

- **ISO/IEC 27001** (wymagania SZBI, certyfikowalna) oraz **27002** (wytyczne zabezpieczeń; wersja 2022 ma 93 zabezpieczenia w 4 grupach). Wcześniejsza wersja grupowała zabezpieczenia w 12–14 domenach (m.in. kontrola dostępu, bezpieczeństwo fizyczne, komunikacja i operacje, incydenty, ciągłość działania, zgodność). **Oświadczenie o stosowalności (SoA)** wskazuje, które zabezpieczenia stosuje organizacja.
- **ISO 27005** (ryzyko), **ISO 22301** (ciągłość działania).
- **SOC 2** (atestacja usługodawcy), **CSA CCM** (chmura), **CIS Controls**, **NIST**.
- **Certyfikacja ISO 27001:** audyt etapu 1 i 2, certyfikat ważny 3 lata, audyty nadzoru co 12 miesięcy.

**Regulacje:** **RODO** (zgłoszenie naruszenia w ciągu 72 godzin, kary do 20 mln € lub 4% obrotu) i **NIS2/KSC**, które wymagają zarządzania ryzykiem i zgłaszania incydentów. W Polsce nowelizacja ustawy o KSC weszła w życie 3.04.2026.

**Znaczenie dla sieci lokalnych:**

- zapewnia **spójne i udokumentowane** zasady (np. segmentacja, kontrola dostępu, zarządzanie poprawkami, monitoring i logowanie),
- określa **odpowiedzialność** i procedury reagowania na incydenty,
- wspiera **zgodność prawną** i zaufanie klientów,
- umożliwia **ciągłe doskonalenie** przez audyty i przeglądy.

**Przykład:** zła praktyka to **brak właściciela** konfiguracji przełączników i brak procedury zmian. SZBI przypisuje odpowiedzialność, wymaga dokumentacji i okresowych przeglądów.

### Aspekty prawne w Polsce i UE

| Regulacja | Treść w skrócie |
| :--- | :--- |
| **RODO (GDPR)** | ochrona danych osobowych; **środki techniczne i organizacyjne** adekwatne do ryzyka, ochrona danych od projektu, **zgłaszanie naruszeń w ciągu 72 h**, DPIA; kary do 20 mln € lub 4% obrotu (wykład ryzyko) |
| **Ustawa o krajowym systemie cyberbezpieczeństwa (KSC) / NIS2** | obowiązki podmiotów kluczowych i ważnych: **zarządzanie ryzykiem**, środki techniczne i organizacyjne, zgłaszanie incydentów do CSIRT, audyty. *(uzupełnienie, sprawdzone w sieci)*: **nowelizacja KSC wdrażająca NIS2 weszła w życie 3 kwietnia 2026 r.**; podmioty mają złożyć wniosek o wpis do wykazu **do 3 października 2026 r.**, dostosować systemy i procedury **do 3 kwietnia 2027 r.**; pierwszy audyt podmiotów kluczowych do 3 kwietnia 2028 r.; kary głównie po tej dacie (sprawdź aktualny stan przepisów przed obroną) |
| **Polskie Normy** | PN-EN ISO/IEC 27001, PN-ISO/IEC 27002 |

### Etyka

Specjalista musi rozumieć prawo i interesy organizacji; podejścia etyczne: **utylitarystyczne**, **oparte na prawach** (prawo do prawdy, prywatności, bezpieczeństwa), **na wspólnym dobru**; **dziesięć przykazań etyki komputerowej** (Computer Ethics Institute); cyberprzestępczość: komputer jako **cel**, **narzędzie** lub **incydentalny** element; Konwencja o cyberprzestępczości (Rady Europy) – pierwszy międzynarodowy traktat.

### Korzyści z wdrożenia ISMS

systematyczne zarządzanie ryzykiem, spójne polityki, jasne role, zgodność z prawem, **dowód dla klientów** (certyfikat), mniejsze skutki incydentów, ciągłe doskonalenie, **pięć filarów** (wykład ryzyko): identyfikacja i ocena ryzyka, środki minimalizujące, polityki i procedury, monitoring i audyty, edukacja i zaangażowanie pracowników.

## Podsumowanie

- **ISMS (SZBI)** to system **polityk, ról, procesów i kontroli** zarządzania bezpieczeństwem informacji oparty na **ryzyku** i **PDCA**; podstawa: **ISO/IEC 27001** (wymagania, cele kontroli), **27002** (kontrole), **27005** (ryzyko).
- Kluczowe elementy: **governance i role** (właściciel, opiekun, IOD), **polityki** (uwierzytelnianie, hasła, dopuszczalne użycie, zdalny dostęp, incydenty, dane), **SoA**, **zarządzanie ryzykiem** (unikanie, redukcja, transfer, akceptacja), **zarządzanie aktywami/podatnościami/konfiguracją/poprawkami**, **audyty i certyfikacja**.
- W sieci lokalnej ISMS przekłada się na **kontrole techniczne** (802.1X, segmentacja, monitoring) i zgodność (RODO, KSC/NIS2).

---
[⬅️ Poprzedni temat](9_Źródła_struktura_i_ocena_alertów_bezpieczeństwa_w_lokalnych_sieciach_komputerowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](11_Zagrożenia_i_metody_ochrony_systemów_mobilnych_oraz_urządzeń_IoT.md)