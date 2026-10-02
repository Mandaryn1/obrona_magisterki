# OWASP Top 10

> **Uwaga o źródłach:** wykład wspomina OWASP Top 10 tylko krótko (slajdy 66, 122, 216–217, 237). Poniższy opis pochodzi głównie z uzupełnień. Lista kategorii została sprawdzona na oficjalnej stronie projektu (owasp.org/Top10/2025).

## Czym jest OWASP Top 10

**OWASP (Open Worldwide Application Security Project)** – otwarta, niezależna organizacja non-profit rozwijająca wiedzę i narzędzia z zakresu bezpieczeństwa aplikacji.

**OWASP Top 10** to **ranking dziesięciu najpoważniejszych kategorii ryzyka bezpieczeństwa aplikacji webowych**, powstały na podstawie danych z testów bezpieczeństwa wielu organizacji i ankiet społeczności; służy jako **dokument uświadamiający** dla programistów i specjalistów bezpieczeństwa oraz **punkt odniesienia do zgodności** (w wykładzie: *lista najważniejszych zagrożeń bezpieczeństwa aplikacji web, którą należy uwzględnić w projektowaniu*, slajd 66; testowanie zgodności z OWASP Top 10, PCI DSS i RODO, slajd 122).

- Aktualizowana co kilka lat (wydania: 2003, 2004, 2007, 2010, 2013, 2017, **2021**, **2025**).
- Jest listą **kategorii** (grup podatności/słabości CWE), a nie kompletną listą wszystkich błędów.
- Nie jest standardem do „odhaczenia" – to minimum świadomości; do pełniejszej weryfikacji służy **OWASP ASVS** (Application Security Verification Standard; wymieniony w wykładzie przy mapowaniu testów, slajd 237).

## OWASP Top 10:2025

| Nr | Kategoria | Istota |
| :-: | :--- | :--- |
| **A01** | **Broken Access Control** (błędy kontroli dostępu) | użytkownik może wykonać akcje lub odczytać zasoby, do których nie ma uprawnień (np. IDOR – zmiana identyfikatora w URL, eskalacja uprawnień, brak autoryzacji w API); w 2025 obejmuje także **SSRF** |
| **A02** | **Security Misconfiguration** (błędna konfiguracja zabezpieczeń) | domyślne hasła, niepotrzebne usługi, szczegółowe komunikaty błędów, otwarte magazyny w chmurze, brak nagłówków bezpieczeństwa |
| **A03** | **Software Supply Chain Failures** (awarie łańcucha dostaw oprogramowania) | rozszerzenie dawnej kategorii „podatne i przestarzałe komponenty": zależności, system budowania, dystrybucja (skompromitowane biblioteki, potoki CI/CD, obrazy) |
| **A04** | **Cryptographic Failures** (błędy kryptograficzne) | brak lub słabe szyfrowanie danych w spoczynku/tranzycie, stare algorytmy, złe zarządzanie kluczami, hasła bez solenia/haszowania |
| **A05** | **Injection** (wstrzykiwanie) | SQL/NoSQL/OS command injection, **XSS** – niewalidowane dane interpretowane jako polecenia |
| **A06** | **Insecure Design** (niebezpieczny projekt) | wady w samej architekturze/projekcie (brak modelowania zagrożeń, brak limitów), nie do naprawienia samą implementacją |
| **A07** | **Authentication Failures** (błędy uwierzytelniania) | słabe hasła, brak MFA, ataki brute force/credential stuffing, nieprawidłowe zarządzanie sesją i tokenami |
| **A08** | **Software or Data Integrity Failures** (błędy integralności oprogramowania lub danych) | brak weryfikacji integralności kodu, aktualizacji, danych, niebezpieczna deserializacja, niepodpisane artefakty |
| **A09** | **Security Logging and Alerting Failures** (błędy logowania i alarmowania) | brak logów zdarzeń bezpieczeństwa i alertów → ataki wykrywane za późno |
| **A10** | **Mishandling of Exceptional Conditions** (niewłaściwa obsługa sytuacji wyjątkowych) | błędy w obsłudze wyjątków i błędów: ujawnianie informacji, „fail-open", niekontrolowane stany |

**Co się zmieniło w 2025:** dwie nowe kategorie (**A03 Software Supply Chain Failures** i **A10 Mishandling of Exceptional Conditions**); **SSRF** wchłonięty przez A01; **Security Misconfiguration** awansował (z #5 na #2); **Insecure Design** spadł (#4 → #6).

### Poprzednia lista (OWASP Top 10:2021) – często spotykana

| Nr | Kategoria 2021 |
| :-: | :--- |
| A01 | Broken Access Control |
| A02 | Cryptographic Failures |
| A03 | Injection (łącznie z XSS) |
| A04 | Insecure Design |
| A05 | Security Misconfiguration |
| A06 | Vulnerable and Outdated Components |
| A07 | Identification and Authentication Failures |
| A08 | Software and Data Integrity Failures |
| A09 | Security Logging and Monitoring Failures |
| A10 | Server-Side Request Forgery (SSRF) |

> Na egzaminie warto znać **ideę i przykłady kategorii**, a kolejność i dokładne nazwy podawać zgodnie z wersją, o której mówił prowadzący (2021 lub 2025).

## OWASP Top 10 w kontekście chmury i aplikacji z wykładu

| Kategoria | Odpowiedź w wykładzie (Spring/Cloud) |
| :--- | :--- |
| **Broken Access Control** | Spring Security: `@PreAuthorize`, role (RBAC), **Resource Server + JWT**, uprawnienia najmniejszego zakresu, Row Level Security w bazie, minimalne `GRANT`-y; testy DAST (IDOR, SSRF) |
| **Security Misconfiguration** | skanowanie **IaC** (Checkov), twarde ustawienia (Pod `runAsNonRoot`), nagłówki bezpieczeństwa (**HSTS, CSP, X-Frame-Options, X-Content-Type-Options**), poprawny **CORS** (nie `*` z `allowCredentials`), NetworkPolicies, brak domyślnych haseł |
| **Software Supply Chain Failures** | **SBOM** (CycloneDX/Syft), skanowanie zależności (**OWASP Dependency-Check, Snyk, Dependabot**), skan obrazów (**Trivy/Grype**), **podpisy obrazów (Cosign)**, minimalne obrazy bazowe, powtarzalne buildy (SLSA) – slajdy 216–217, 262–265 |
| **Cryptographic Failures** | **TLS/HTTPS + HSTS**, mTLS w mesh, szyfrowanie w spoczynku (TDE, pgcrypto, KMS), zarządzanie kluczami i sekretami w **Vault**; rotacja kluczy (slajdy 220–222) |
| **Injection / XSS** | **zapytania parametryzowane**, JPA/Hibernate (ORM), walidacja wejścia (**Bean Validation**: `@NotNull`, `@Size`, `@Pattern`), sanityzacja, kodowanie wyjścia, **CSP** (slajd 82) |
| **Insecure Design** | architektura bezpieczeństwa **od początku projektu**, modelowanie zagrożeń, Zero Trust, ograniczanie ruchu (rate limiting), wzorce odporności (circuit breaker) |
| **Authentication Failures** | **OAuth2/OIDC**, Keycloak, **MFA**, krótkie TTL tokenów JWT, zarządzanie sesją, rate limiting na logowaniu |
| **Software or Data Integrity Failures** | podpisywanie obrazów, polityka admission controller wymagająca podpisu, bezpieczny CI/CD (ephemeral runners, krótkie poświadczenia), weryfikacja zależności |
| **Security Logging and Alerting Failures** | **logowanie audytowe** (kto, co, kiedy, skąd), korelacja z IdP, **SIEM**, alerty (szczyt 401/403), Falco, metryki Prometheus/Grafana (slajdy 85, 189, 245–251) |
| **Mishandling of Exceptional Conditions** | `GlobalExceptionHandler`, brak ujawniania stack trace w odpowiedziach, **wzorce odporności** (Resilience4j: circuit breaker, retry, timeout), „fail-secure" |

## Obrona praktyczna w cyklu życia (DevSecOps – wykład)

- **SAST** (SonarQube, Semgrep, Snyk) – analiza kodu źródłowego,
- **DAST** (**OWASP ZAP**, Burp Suite) – testy działającej aplikacji (SQLi, XSS, SSRF, IDOR),
- **skanowanie zależności** (OWASP Dependency-Check), kontenerów (Trivy), IaC (Checkov),
- **SBOM + podpisy**, testy penetracyjne,
- integracja z **CI/CD** (GitHub Actions), raporty do SIEM; mapowanie testów do kontroli (ISO 27001 Annex A, SOC 2, **OWASP ASVS**),
- **WAF** (wykrywa SQL injection, XSS – slajd 28), **API Gateway**, rate limiting, nagłówki bezpieczeństwa.

*(uzupełnienie)* Powiązane listy OWASP: **OWASP API Security Top 10** (m.in. Broken Object Level Authorization, Broken Authentication, Unrestricted Resource Consumption) – szczególnie ważna w chmurze, gdzie wg wykładu **ataki na API są najbardziej niebezpieczne**; **OWASP Cloud-Native Application Security Top 10**, **OWASP Docker/Kubernetes Top 10**.

## Podsumowanie

- **OWASP Top 10** = lista **dziesięciu najpoważniejszych kategorii ryzyka bezpieczeństwa aplikacji webowych**, uaktualniana co kilka lat (2021, **2025**), podstawa świadomości i testów zgodności.
- Na czele: **Broken Access Control**; dalej m.in. **błędna konfiguracja, łańcuch dostaw, błędy kryptograficzne, wstrzykiwanie, niebezpieczny projekt, błędy uwierzytelniania**, integralność, logowanie i obsługa wyjątków.
- Środki: walidacja wejścia, parametryzowane zapytania, uwierzytelnianie/autoryzacja, szyfrowanie, bezpieczna konfiguracja, skanowanie (SAST/DAST/zależności/obrazy), SBOM, logowanie i monitoring, WAF, DevSecOps.
