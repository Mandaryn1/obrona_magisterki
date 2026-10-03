# Testy penetracyjne, ocena podatności oraz bazy CVE i MITRE ATT&CK

## 1. Testy penetracyjne a Ocena Podatności (Vulnerability Assessment)

* **Testy penetracyjne (Penetration Testing):** Kontrolowany i autoryzowany proces poszukiwania oraz aktywnego wykorzystywania (eksploatacji) luk w zabezpieczeniach systemów, aplikacji i sieci w celu oceny ich rzeczywistej odporności na ataki.
* **Ocena podatności (Vulnerability Assessment):** Automatyczny proces identyfikacji i katalogowania znanych słabych punktów w oprogramowaniu i konfiguracji bez podejmowania prób ich eksploatacji.
* **Perspektywy i rodzaje testów (ze względu na poziom wiedzy):**
  * **Testy z brakiem wiedzy (Black-box / Unknown-Environment):** Tester dysponuje jedynie podstawowymi informacjami (np. nazwa domeny, zakres IP) i działa z perspektywy zewnętrznego atakującego.
  * **Testy z pełną wiedzą (White-box / Known-Environment):** Tester posiada pełną dokumentację, schematy sieci, kod źródłowy oraz poświadczenia, co pozwala na dokładne wykrycie maksymalnej liczby luk.
  * **Testy z częściową wiedzą (Gray-box / Partially Known Environment):** Podejście hybrydowe, w którym tester otrzymuje np. konta użytkowników bez pełnej dokumentacji infrastruktury.
* **Zespoły Red Team vs Blue Team:** **Red Team** to grupa ekspertów symulująca realnego przeciwnika (ataki na technologię, ludzi i bezpieczeństwo fizyczne), podczas gdy **Blue Team** to wewnętrzny zespół obronny (SOC, CSIRT) odpowiadający za detekcję i odpieranie ataków.

---

## 2. Przebieg i rodzaje skanowania podatności

* **Cztery etapy automatycznego skanowania:**
  1. *Inwentaryzacja:* Wykrywanie aktywnych hostów i otwartych portów (np. za pomocą Nmapa).
  2. *Identyfikacja:* Określanie typu i wersji uruchomionego oprogramowania.
  3. *Korelacja:* Dopasowywanie wykrytych wersji do baz znanych luk w zabezpieczeniach.
  4. *Raportowanie:* Generowanie zestawienia potencjalnych luk z oceną ryzyka.
* **Rodzaje skanów podatności:**
  * **Skanowanie nieuwierzytelnione:** Skaner bada system z zewnątrz bez poświadczeń, pokazując widok otwarty dla zewnętrznego atakującego.
  * **Skanowanie uwierzytelnione:** Skaner loguje się do systemu (np. przez SSH z prawami root/admin) i uruchamia polecenia wewnętrzne, dostarczając pełny i wiarygodny obraz podatności (np. identyfikacja PID procesów).
  * **Skanowanie zgodności (Compliance):** Weryfikacja spełniania wymogów regulacyjnych i branżowych (np. PCI DSS, HIPAA, GLBA).
* **Wyzwania skanowania:** Eliminacja fałszywych alarmów (*False Positives*) poprzez ręczną weryfikację lub eksploatację, ograniczanie wpływu na przepustowość sieci oraz ostrożność przy skanowaniu wrażliwych urządzeń (np. drukarek i IoT).

---

## 3. Bazy danych i standardy opisu luk: CVE, CWE, CAPEC i CVSS

* **CVE (Common Vulnerabilities and Exposures):** Ogólnoświatowy słownik znanych luk i zagrożeń w oprogramowaniu. Każda luka posiada unikalny identyfikator w formacie `CVE-YYYY-NNNN`.
* **CWE (Common Weakness Enumeration):** Słownik i klasyfikacja podstawowych typów słabości oprogramowania i błędów w kodzie (np. brak czyszczenia danych wejściowych), będących przyczyną powstania luk CVE.
* **CAPEC (Common Attack Pattern Enumeration and Classification):** Słownik i wykaz wzorców ataków wykorzystywanych przez przeciwników w środowisku naturalnym.
* **CVSS (Common Vulnerability Scoring System):** Standard punktowej oceny krytyczności luk w skali od 0 do 10. Wynik obliczany jest na podstawie trzech grup wskaźników:
  * **Podstawowej (Base Group):** Mierzy stałe cechy luki — wektor ataku, złożoność, wymagane uprawnienia oraz wpływ na poufność, integralność i dostępność (triada CIA).
  * **Czasowej (Temporal Group):** Uwzględnia zmiany w czasie (np. dostępność gotowego exploita czy łatki).
  * **Środowiskowej (Environmental Group):** Dostosowuje ocenę do konkretnej infrastruktury i konfiguracji klienta.

---

## 4. Framework MITRE ATT&CK i Metodyki Testów

* **MITRE ATT&CK:** Publiczna baza wiedzy oparta na rzeczywistych obserwacjach działań przeciwników. Porządkuje wiedzę w postaci macierzy (np. Enterprise, Cloud, Mobile) zawierających taktyki, techniki i procedury (TTP) wykorzystywane na poszczególnych etapach ataku.
* **Główne metodyki i standardy testów penetracyjnych:**
  * **PTES (Penetration Testing Execution Standard):** Obejmuje 7 faz: interakcje przed-wdrożeniowe, zbieranie wywiadu, modelowanie zagrożeń, analizę podatności, eksploatację, działania po-eksploatacyjne oraz raportowanie.
  * **NIST SP 800-115:** Standard branżowy wydany przez NIST, zawierający wytyczne dotyczące planowania i przeprowadzania testów bezpieczeństwa.
  * **OWASP WSTG (Web Security Testing Guide):** Kompleksowy przewodnik testowania aplikacji webowych (obejmuje m.in. ataki SQLi, XSS, CSRF, XXE).
  * **OSSTMM:** Przewodnik spójnego i powtarzalnego testowania bezpieczeństwa operacyjnego.

---

## 5. Podsumowanie do wypowiedzi na obronie

> *"Testy penetracyjne to autoryzowana eksploatacja luk w celu oceny odporności systemu, w przeciwieństwie do automatycznej oceny podatności, która jedynie kataloguje słabości. Identyfikacja luk opiera się na słownikach CVE, klasyfikacji błędów CWE oraz ocenie krytyczności w skali CVSS (0-10). Do ustrukturyzowania testów oraz analizy taktyk i technik atakujących stosuje się framework MITRE ATT&CK oraz metodyki branżowe takie jak PTES, NIST SP 800-115 i OWASP WSTG."*

## Podsumowanie

- **Test penetracyjny** = autoryzowana symulacja ataku (etyczny hacking); metodyki **PTES, NIST SP 800-115, OSSTMM, ISSAF, ATT&CK, OWASP WSTG**; fazy planowanie–odkrywanie–atak–raportowanie; ścisły **zakres i zgoda**.
- **Ocena podatności**: rozpoznanie pasywne/aktywne, skanery (uwierzytelnione/nieuwierzytelnione…), **weryfikacja fałszywych alarmów**, priorytetyzacja wg krytyczności i prawdopodobieństwa wykorzystania.
- **Bazy:** **CVE** (identyfikatory), **NVD** (oceny), **CVSS** (0–10), **CWE** (słabości), **CAPEC** (wzorce ataków), KEV/EPSS; **MITRE ATT&CK** – taktyki i techniki przeciwnika (14 taktyk Enterprise) – używane w testach, detekcji, threat huntingu i analizie incydentów.

---
[⬅️ Poprzedni temat](11_Proces_reagowania_na_incydenty_i_podstawy_analizy_powłamaniowej.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../PrzygotowanieIPublikowanieArtykułówNaukowych/PrzygotowanieIPublikowanieArtykułówNaukowych_tytul.md)