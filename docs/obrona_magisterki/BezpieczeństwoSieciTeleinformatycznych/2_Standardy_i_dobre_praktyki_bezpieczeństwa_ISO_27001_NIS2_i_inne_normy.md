# Standardy i dobre praktyki bezpieczeństwa: ISO 27001, NIS2 oraz inne wybrane normy

**Po co standardy:** dają wspólny język, listę sprawdzonych zabezpieczeń, podstawę audytu i certyfikacji, a często są **wymogiem prawnym lub umownym**. Wszystkie opierają się na **zarządzaniu ryzykiem**.

**ISO/IEC 27001 – system zarządzania bezpieczeństwem informacji (SZBI/ISMS):**

- Norma określa **wymagania** dla ISMS (wersja 2022; w Polsce PN-EN ISO/IEC 27001:2023). Jest **dobrowolna i certyfikowalna**.
- Opiera się na cyklu **PDCA** (planuj, wdrażaj, sprawdzaj, doskonal) i na podejściu opartym na ryzyku.
- Klauzule: kontekst organizacji, przywództwo (polityka bezpieczeństwa), planowanie (ocena ryzyka), wsparcie, operacje, ocena (audyty wewnętrzne, przeglądy zarządcze) i doskonalenie.
- **Oświadczenie o stosowalności (SoA)** wskazuje, które zabezpieczenia stosujemy i dlaczego.
- **ISO/IEC 27002:2022** to wytyczne wdrażania: **93 zabezpieczenia w 4 grupach** (organizacyjne, dotyczące osób, fizyczne, technologiczne).
- **Certyfikacja:** audyt etapu 1 (dokumentacja) i 2 (wdrożenie), certyfikat ważny **3 lata**, audyty nadzoru co 12 miesięcy.
- Pokrewne: ISO 27005 (ryzyko), ISO 22301 (ciągłość działania), 27017/27018 (chmura), 27701 (prywatność).

**NIS2 – dyrektywa UE 2022/2555** o cyberbezpieczeństwie:

- Dotyczy **podmiotów kluczowych i ważnych** w wielu sektorach (energetyka, transport, bankowość, zdrowie, infrastruktura cyfrowa, administracja publiczna i inne).
- **Środki zarządzania ryzykiem:** analiza ryzyka, obsługa incydentów, ciągłość działania, **bezpieczeństwo łańcucha dostaw**, zarządzanie podatnościami, kryptografia, kontrola dostępu i MFA, szkolenia.
- **Zgłaszanie incydentów:** wczesne ostrzeżenie w **24 h**, zgłoszenie w **72 h**, raport końcowy po miesiącu.
- **Odpowiedzialność kierownictwa** i wysokie kary (do 10 mln € lub 2% obrotu dla podmiotów kluczowych).
- **W Polsce:** wdraża ją nowelizacja ustawy o **KSC**, która weszła w życie **3.04.2026**. Wpis do wykazu podmiotów trzeba było złożyć do **3.10.2026**, dostosowanie do **3.04.2027**, a pierwszy audyt podmiotów kluczowych do **3.04.2028**.

**Inne standardy i ramy:**

- **NIST Cybersecurity Framework:** funkcje Identify, Protect, Detect, Respond, Recover (wersja 2.0 dodaje Govern); **NIST SP 800-53, 800-115, 800-61**.
- **CIS Controls** i **CIS Benchmarks** (priorytetyzowane zabezpieczenia i wzorce utwardzania).
- **PCI DSS** (dane kart płatniczych, wymaga segmentacji, szyfrowania i regularnych testów), **RODO** (ochrona danych osobowych, zgłoszenie naruszenia w 72 h, kary do 20 mln € lub 4% obrotu).
- **DORA** (sektor finansowy), **IEC 62443** (systemy przemysłowe), **SOC 2** (atestacja usługodawców), **HIPAA** (zdrowie, USA), **Krajowe Ramy Interoperacyjności** (administracja publiczna w Polsce).

**Różnice (ISO 27001 vs NIS2):** ISO 27001 jest dobrowolne i certyfikowane przez audyt, a NIS2 jest **obowiązkiem prawnym** z nadzorem organu i karami. Wdrożony ISMS ułatwia spełnienie NIS2 i RODO.

**Wniosek:** normy uzupełniają się. ISO 27001 daje system zarządzania, NIST i CIS dobre praktyki techniczne, a NIS2 i RODO wyznaczają wymagania prawne.

## Podsumowanie

- **ISO/IEC 27001** – wymagania dla SZBI (PDCA, klauzule 4–10, SoA) i **27002** (93 zabezpieczenia w 4 grupach); certyfikacja 3-letnia z nadzorem rocznym.
- **NIS2** – dyrektywa UE: środki zarządzania ryzykiem (art. 21), **zgłaszanie incydentów 24 h/72 h/1 mies.**, odpowiedzialność zarządu, kary; w Polsce wdrożona nowelizacją KSC (**w życie 3.04.2026**; wpis do wykazu do 3.10.2026, dostosowanie do 3.04.2027).
- Inne: **NIST CSF** (Identify–Protect–Detect–Respond–Recover; 2.0 + Govern), **CIS Controls**, **PCI DSS**, **RODO**, ISO 27005/22301/27017, DORA, IEC 62443, SOC 2.
- Standardy uzupełniają się i wspólnie budują podejście oparte na ryzyku.

---
[⬅️ Poprzedni temat](1_Ataki_polegające_na_rozpoznaniu_uzyskaniu_dostępu_oraz_inżynierii_społecznej.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](3_Zabezpieczanie_styku_sieci_teleinformatycznej_z_sieciami_zewnętrznymi.md)