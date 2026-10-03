# Etapy i metody testowania bezpieczeństwa sieci teleinformatycznych

**Cel testowania:** znaleźć możliwe ścieżki włamania **zanim zrobią to atakujący** i zweryfikować, że zabezpieczenia (zapory, IPS, VPN, WAF) rzeczywiście działają. Test jest oceną **w punkcie czasowym**, więc powtarza się go regularnie, szczególnie po zmianach.

**Etyczny haker** działa jak atakujący, ale **za zgodą i w ramach ustalonego zakresu (scope)**. O różnicy decyduje intencja i uprawnienie.

**Metodyki i standardy:**

- **NIST SP 800-115** (wytyczne testów bezpieczeństwa),
- **PTES** (7 faz: interakcje przedwstępne, zbieranie informacji, modelowanie zagrożeń, analiza podatności, eksploatacja, post-eksploatacja, raportowanie),
- **OSSTMM**, **ISSAF**,
- **MITRE ATT&CK** (taktyki i techniki przeciwnika), **OWASP WSTG** (aplikacje WWW).

**Perspektywa testera:** **unknown-environment** (black box: minimum wiedzy, perspektywa zewnętrznego atakującego, tańszy), **partially known** (gray box) i **known-environment** (white box: pełna wiedza, najpełniejsze wykrycie luk, najdroższy). Środowiska: infrastruktura sieciowa, aplikacje, chmura, fizyczne, socjotechniczne.

**Etapy (4 fazy wg NIST/Cisco):**

1. **Planowanie i określenie zakresu:** pisemna zgoda, **reguły zaangażowania (RoE)**, lista celów (IP, domeny), okno czasowe, umowy (SOW, MSA, NDA), ścieżka eskalacji, wymogi prawne i zgodność (PCI DSS, RODO). Ważne jest kontrolowanie **scope creep**.
2. **Odkrywanie:** rozpoznanie **pasywne** (DNS, Whois, OSINT, Shodan, certyfikaty) i **aktywne** (skany portów Nmap, enumeracja usług, użytkowników i udziałów) oraz **skanowanie podatności** (Nessus, OpenVAS): wykrycie hostów i portów, identyfikacja wersji, dopasowanie do znanych podatności, raport. Skany mogą być uwierzytelnione lub nie, nieinwazyjne lub inwazyjne.
3. **Atak:** wykorzystanie podatności (weryfikacja wyników skanera), **eskalacja uprawnień**, ruch boczny, utrwalenie dostępu, dostęp do danych jako dowód wpływu. Tylko w zakresie, bez działań destrukcyjnych, z natychmiastowym zgłoszeniem aktywnej kompromitacji.
4. **Analiza i raportowanie:** weryfikacja i **eliminacja fałszywych alarmów**, priorytetyzacja (**CVSS**, **CVE**, **CWE**, krytyczność zasobu), rekomendacje i terminy, podsumowanie dla kierownictwa i część techniczna, **retest** po naprawach.

**Metody testowania:**

- **test penetracyjny** (symulacja ataku),
- **skanowanie sieci i podatności**,
- łamanie haseł (audyt polityki haseł),
- przegląd logów i kontrola integralności,
- testy zapór, IPS, VPN, segmentacji i sieci bezprzewodowych,
- testy socjotechniczne (phishing, tailgating) i fizyczne,
- audyt konfiguracji (CIS Benchmarks),
- **ST&E** (testy i ocena zabezpieczeń),
- ćwiczenia zespołów: **red team** (atakuje), **blue team** (broni), **white team** (sędzia), **purple team** (współpraca).

**Ocena podatności vs pentest:** pierwsza **identyfikuje** słabości (szeroko, automatycznie), drugi **wykorzystuje** je (głębiej, ręcznie) i pokazuje realny wpływ.

**Dobre praktyki:** 

- **Pisemna zgoda i jasny zakres**, kontrola scope creep,
- **lab testowy** i znajomość narzędzi przed użyciem u klienta (W3),
- **ostrożność w produkcji** (czas, tempo, kruche systemy),
- **walidacja wyników** i eliminacja fałszywych alarmów,
- **bezpieczne przechowywanie danych** z testu i usunięcie po zakończeniu,
- **raport rozumiany przez odbiorców** + plan naprawczy i retest,
- **cykliczność** (np. roczny pentest, kwartalne skany, test po zmianach) i integracja z zarządzaniem ryzykiem i ISMS; **spełnienie wymogów regulacji** (PCI DSS, NIS2 – audyt, RODO).

## Podsumowanie

- **Cel:** znaleźć luki przed napastnikami i zweryfikować działanie zabezpieczeń; test jest **pomiarem w punkcie czasowym**, powtarzanym regularnie.
- **Metodyki:** **NIST SP 800-115, PTES (7 faz), OSSTMM, ISSAF, MITRE ATT&CK, OWASP WSTG**; środowiska: infrastruktura, aplikacje, chmura, fizyczne, socjotechniczne; perspektywy: **unknown/partially known/known environment**.
- **Etapy:** **planowanie** (zakres, RoE, SOW/MSA/NDA, zgody, zgodność) → **odkrywanie** (rozpoznanie pasywne i aktywne, skanowanie podatności) → **atak** (eksploatacja, eskalacja, ruch boczny, persistence) → **raportowanie** (analiza, CVSS/CVE/CWE, priorytety, rekomendacje, retest).
- **Metody:** pentest, skanowanie sieci i podatności, łamanie haseł, przegląd logów, kontrola integralności, testy zapór/IPS/VPN/segmentacji/Wi-Fi, socjotechnika, ST&E, ćwiczenia red/blue/white/purple team.

---
[⬅️ Poprzedni temat](9_Zastosowanie_sieci_VPN_w_bezpiecznej_komunikacji_oraz_wybrane_technologie_VPN.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](11_Zastosowanie_sztucznej_inteligencji_i_uczenia_maszynowego_w_ochronie_sieci.md)