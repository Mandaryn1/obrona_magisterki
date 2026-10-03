# Czym jest OWASP Top 10 ?

**OWASP Top 10** to **ranking dziesięciu najważniejszych kategorii ryzyka bezpieczeństwa aplikacji webowych**, opracowywany przez **OWASP (Open Worldwide Application Security Project)**, czyli międzynarodową, niezależną fundację non-profit. Powstaje na podstawie danych zebranych z wielu organizacji i opinii ekspertów, a aktualizuje się go co kilka lat. Jest powszechnie uznawanym standardem świadomości bezpieczeństwa dla programistów, testerów i firm. Do tych kategorii odwołują się standardy, wymagania i testy bezpieczeństwa.

**Cel:** pokazać najczęstsze i najgroźniejsze podatności oraz sposoby zapobiegania im. Nie jest to pełna lista błędów, tylko punkt wyjścia.

**Lista z wersji 2025 (nowe kategorie wyróżnione):**

1. Broken Access Control: błędy kontroli dostępu,
2. Security Misconfiguration: błędy konfiguracji,
3. **Software Supply Chain Failures**: ataki na łańcuch dostaw oprogramowania (nowe),
4. Cryptographic Failures: błędy kryptograficzne,
5. Injection: wstrzykiwanie kodu (SQL, XSS, polecenia),
6. Insecure Design: niebezpieczne projektowanie,
7. Authentication Failures: błędy uwierzytelniania,
8. Software or Data Integrity Failures: naruszenia integralności,
9. Security Logging and Alerting Failures: brak logowania i alertów,
10. **Mishandling of Exceptional Conditions**: błędna obsługa wyjątków i błędów (nowe).

**Uwaga:** poprzednia wersja (2021) miała m.in. osobne kategorie „Vulnerable and Outdated Components" oraz SSRF. W 2025 pierwsza została włączona do łańcucha dostaw, a SSRF do Broken Access Control. Podaj na obronie wersję, którą omawiał prowadzący.

**Jak stosować:** do szkoleń programistów, projektowania i przeglądu kodu, testów bezpieczeństwa (SAST/DAST, pentesty) i oceny ryzyka aplikacji.

## Podsumowanie

- **OWASP Top 10** = lista **dziesięciu najpoważniejszych kategorii ryzyka bezpieczeństwa aplikacji webowych**, uaktualniana co kilka lat (2021, **2025**), podstawa świadomości i testów zgodności.
- Na czele: **Broken Access Control**; dalej m.in. **błędna konfiguracja, łańcuch dostaw, błędy kryptograficzne, wstrzykiwanie, niebezpieczny projekt, błędy uwierzytelniania**, integralność, logowanie i obsługa wyjątków.
- Środki: walidacja wejścia, parametryzowane zapytania, uwierzytelnianie/autoryzacja, szyfrowanie, bezpieczna konfiguracja, skanowanie (SAST/DAST/zależności/obrazy), SBOM, logowanie i monitoring, WAF, DevSecOps.

---
[⬅️ Poprzedni temat](14_Data_minimization_minimalizacja_danych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../AlgorytmyKryptograficzne/AlgorytmyKryptograficzne_tytul.md)