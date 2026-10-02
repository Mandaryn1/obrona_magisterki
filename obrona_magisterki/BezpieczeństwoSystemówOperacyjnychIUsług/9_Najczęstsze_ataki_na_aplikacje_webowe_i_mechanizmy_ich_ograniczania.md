# Najczęstsze ataki na aplikacje webowe oraz podstawowe mechanizmy ich ograniczania

> Wykład: W5 web (slajdy 2–19: pojęcia, OWASP Top 10, SQL Injection, XSS, CSRF, sesje, WAF, TLS, kontenery, automatyzacja testów, monitoring); W3 (OWASP WSTG). Dodatkowe ataki i fragmenty kodu – ***(uzupełnienie)***. Listę OWASP Top 10 w wersji 2025 sprawdziłem na oficjalnej stronie OWASP.

## Kontekst (wykład, slajd 2)

Aplikacje działają w **warstwie 7**, gdzie zachodzi większość ataków na **logikę biznesową i dane**; atakujący celują tam, bo daje bezpośredni dostęp do danych i funkcji. Podstawy: **uwierzytelnianie** (kim jesteś), **autoryzacja** i kontrola dostępu (co możesz; najmniejsze uprawnienia), **szyfrowanie i integralność** (TLS, skróty, podpisy).

## OWASP Top 10

**OWASP (Open Worldwide Application Security Project)** – niezależna organizacja non-profit; **Top 10** to ranking najważniejszych kategorii ryzyka aplikacji webowych (aktualizowany co kilka lat).

| Nr | Wykład (OWASP Top 10:2021) | OWASP Top 10:2025 *(uzupełnienie)* |
| :-: | :--- | :--- |
| A01 | Broken Access Control | Broken Access Control (obejmuje SSRF) |
| A02 | Cryptographic Failures | **Security Misconfiguration** |
| A03 | Injection | **Software Supply Chain Failures** |
| A04 | Insecure Design | Cryptographic Failures |
| A05 | Security Misconfiguration | Injection |
| A06 | Vulnerable and Outdated Components | Insecure Design |
| A07 | Identification and Authentication Failures | Authentication Failures |
| A08 | Software and Data Integrity Failures | Software or Data Integrity Failures |
| A09 | Security Logging and Monitoring Failures | Security Logging and **Alerting** Failures |
| A10 | Server-Side Request Forgery (SSRF) | Mishandling of Exceptional Conditions |

*(Na obronie podawaj wersję, którą omawiał prowadzący; wykład używa 2021.)*

## 1. SQL Injection (SQLi) – wykład, slajdy 4–5

**Mechanizm:** wstrzyknięcie kodu SQL do zapytania budowanego przez **konkatenację** danych użytkownika. Klasyczny przykład: `' OR '1'='1` w polu logowania:

```sql
SELECT * FROM users WHERE username = '' OR '1'='1' AND password = '' OR '1'='1';
-- warunek zawsze prawdziwy → zwraca wszystkie rekordy / omija weryfikację hasła
```

**Skutki:** **kradzież danych** (dane osobowe, hasła, finansowe), **modyfikacja danych** (podwyższanie uprawnień, manipulacja transakcjami), **usunięcie danych** (`DROP TABLE`), w niektórych konfiguracjach **wykonanie komend systemu operacyjnego**. Przykład rzeczywisty: Heartland Payment Systems (2008). *(uzup.)* Odmiany: in-band (UNION, error-based), blind (boolean/time-based), out-of-band; NoSQL injection.

**Obrona:**

1. **zapytania parametryzowane (Prepared Statements)** – rozdzielają kod SQL od danych; baza traktuje parametry wyłącznie jako dane (najskuteczniejsza),
2. **walidacja i sanityzacja wejścia** – typ, długość, format, **biała lista** zamiast czarnej; odrzucać nieprawidłowe dane,
3. **ORM** i mechanizmy frameworków (ale nie chronią w 100% – błędy w „raw SQL"),
4. **najmniejsze uprawnienia konta bazy** – aplikacja łączy się kontem o minimalnych prawach (nie administrator),
5. *(uzup.)* ukrywanie szczegółów błędów, WAF, procedury składowane, szyfrowanie wrażliwych kolumn.

```javascript
// NIEBEZPIECZNE
const query = `SELECT * FROM users WHERE id = ${userId}`;
// BEZPIECZNE – zapytanie parametryzowane
connection.query('SELECT * FROM users WHERE id = ?', [userId]);
```

## 2. Cross-Site Scripting (XSS) – wykład, slajdy 6–7

**Mechanizm:** wstrzyknięcie złośliwego **JavaScriptu** do stron wyświetlanych innym użytkownikom; przeglądarka wykonuje go **w kontekście domeny aplikacji**. Atakuje **użytkowników**, nie serwer.

| Typ | Opis |
| :--- | :--- |
| **Reflected (odbity)** | skrypt jest częścią żądania i odbijany w odpowiedzi (link) |
| **Stored (trwały)** | skrypt zapisany w bazie (komentarz, profil) i pokazywany wielu użytkownikom |
| **DOM-based** | manipulacja DOM po stronie klienta |

**Skutki:** kradzież **ciasteczek sesyjnych** i tokenów, przejęcie konta, przekierowanie na phishing, keyloggery, defacement, wykonywanie akcji w imieniu użytkownika.

**Obrona:**

- **kodowanie danych wyjściowych** zależnie od kontekstu (HTML – encje `< > & " '`; JavaScript; URL; CSS); PHP: `htmlspecialchars($in, ENT_QUOTES, 'UTF-8')`,
- **Content Security Policy (CSP)** – nagłówek ograniczający źródła treści: `Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.cdn.com; style-src 'self' 'unsafe-inline';` (blokuje skrypty inline i z nieautoryzowanych źródeł),
- **walidacja wejścia** (biała lista; nie jedyna obrona – obrona w głąb),
- **bezpieczne frameworki** (React, Angular, Vue kodują domyślnie; uważać na `dangerouslySetInnerHTML`/`innerHTML`),
- sanityzacja HTML (np. DOMPurify) dla treści bogatych, ciasteczka **HttpOnly**.

## 3. Cross-Site Request Forgery (CSRF) – wykład, slajd 8

**Mechanizm:** wykorzystanie faktu, że przeglądarka **automatycznie dołącza ciasteczka** do żądań do danej domeny. Zalogowany w banku użytkownik odwiedza złośliwą stronę z ukrytym formularzem/JS wysyłającym np. przelew do banku – żądanie jest uwierzytelnione, więc wykonane.

**Obrona:** **tokeny CSRF** (synchronizujące; unikalne dla sesji, nieprzewidywalne – dla POST/PUT/DELETE), **atrybut `SameSite`** ciasteczek (`Strict`/`Lax`), *(uzup.)* sprawdzanie nagłówków `Origin`/`Referer`, ponowne uwierzytelnienie przy operacjach krytycznych, brak zmian stanu w żądaniach GET.

## 4. Sesje – wykład, slajdy 9–10

HTTP jest **bezstanowy**; **sesja** utrzymuje stan (identyfikator sesji w ciasteczku → dane po stronie serwera).

**Zagrożenia:** **przejęcie sesji** (session hijacking – XSS, podsłuch, malware), **utrwalenie sesji** (session fixation – znany ID przed logowaniem), **session sidejacking** (sieci Wi-Fi bez szyfrowania), **brute force ID sesji**.

**Bezpieczne zarządzanie:** flagi **`HttpOnly`** (brak dostępu z JS) i **`Secure`** (tylko HTTPS), `SameSite`; np. `Set-Cookie: sessionId=abc123; HttpOnly; Secure; SameSite=Strict`; **limity czasu** – bezczynności (15–30 min) i całkowity (8–12 h); **regeneracja ID po zmianie poziomu uprawnień** (logowanie); długie, losowe identyfikatory; unieważnianie przy wylogowaniu; opcjonalnie powiązanie z IP/User-Agent.

## 5. Inne częste ataki *(uzupełnienie)*

| Atak | Opis | Obrona |
| :--- | :--- | :--- |
| **Broken Access Control / IDOR** | brak sprawdzania uprawnień (zmiana `?id=` w URL daje cudze dane), eskalacja funkcji | autoryzacja po stronie serwera dla każdego żądania, domyślna odmowa, RBAC |
| **Command / OS injection** | dane trafiają do polecenia systemowego | unikać powłoki, API bez powłoki, biała lista, najmniejsze uprawnienia |
| **Path traversal** | `../../etc/passwd` | kanonikalizacja ścieżki, biała lista, chroot/piaskownica |
| **File upload** | wgranie skryptu/malware | kontrola typu i treści, przechowywanie poza webroot, losowe nazwy, skanowanie |
| **SSRF** | serwer wykonuje żądanie do wewnętrznych zasobów (np. metadane chmury) | biała lista adresów, segmentacja, blokada adresów wewnętrznych |
| **XXE / niebezpieczna deserializacja** | złośliwy XML/obiekt | wyłączenie DTD, bezpieczne parsery, podpisane dane |
| **Clickjacking** | ukryta ramka | `X-Frame-Options`/`frame-ancestors` |
| **Brute force, credential stuffing** | zgadywanie haseł | MFA, rate limiting, blokady, CAPTCHA |
| **Security misconfiguration** | domyślne hasła, listing katalogów, ujawnianie błędów | utwardzanie, nagłówki bezpieczeństwa |
| **Podatne komponenty (Log4Shell, Struts)** | luki w bibliotekach | SCA, SBOM, aktualizacje |
| **DoS na warstwie 7** | HTTP flood, Slowloris | WAF, rate limiting, CDN |

## WAF – wykład, slajd 11

**Zapora aplikacji webowych** monitoruje, filtruje i blokuje ruch HTTP; analizuje żądania i odpowiedzi według reguł. Typy: **sprzętowy** (wydajny, kosztowny), **programowy** (ModSecurity, Imperva), **chmurowy** (Cloudflare, AWS WAF, Azure WAF – SaaS). Modele: **pozytywny (biała lista)** i **negatywny (czarna lista)**; nowoczesne łączą oba z ML. WAF to **dodatkowa warstwa**, nie zastępuje bezpiecznego kodu.

## TLS i certyfikaty – wykład, slajdy 12–14

TLS (handshake, szyfrowanie, certyfikaty DV/OV/EV, wildcard, SAN) zapobiega **MITM**, buduje zaufanie, jest wymagany przez **RODO (art. 32)** i **PCI DSS (TLS ≥ 1.2)**; **HSTS**, automatyczne odnawianie certyfikatów (temat 8).

## Aplikacje kontenerowe – wykład, slajdy 15–16

**Zagrożenia:** nieaktualne obrazy z lukami CVE (Log4Shell, Heartbleed), konfiguracja (proces **root** w kontenerze, tryb uprzywilejowany, niepotrzebne porty), **ataki na rejestry i łańcuch dostaw** (złośliwe obrazy, **typosquatting** – „alphine"). **Praktyki:** **minimalne, oficjalne obrazy bazowe** (Alpine, distroless), **skanowanie obrazów** (Trivy, Clair, Snyk, Docker Scout) w CI/CD, **użytkownik niebędący rootem** (`USER appuser`), ograniczanie uprawnień (read-only FS, `--cap-drop`), NetworkPolicies, podpisy obrazów, SBOM.

## Automatyzacja testów – wykład, slajd 17

Testy penetracyjne (black/white/gray box; co najmniej raz w roku i po zmianach), **fuzzing** (AFL, libFuzzer, Atheris – przepełnienia, błędy iniekcji, parsowania), narzędzia **SAST/DAST/SCA** w CI/CD (*uzup.:* SonarQube, OWASP ZAP, Burp Suite, Dependabot).

## Monitorowanie i reagowanie – wykład, slajd 18

**Centralizacja logów (SIEM)** z aplikacji, WAF, serwerów, baz danych, Kubernetes i chmury; reguły alertów: wiele nieudanych logowań z IP (brute force), **wzorce SQLi w parametrach**, dostęp do wrażliwych ścieżek (path traversal), nagły wzrost ruchu (DDoS), **wykonanie poleceń systemowych przez aplikację**, zmiany plików konfiguracyjnych; automatyczne blokady.

## Zasady bezpiecznego tworzenia aplikacji (podsumowanie)

1. nigdy nie ufać danym wejściowym (walidacja po stronie serwera, biała lista),
2. kodowanie wyjścia; parametryzowane zapytania,
3. autoryzacja na każdym żądaniu (najmniejsze uprawnienia),
4. bezpieczne sesje i ciasteczka, CSRF tokeny, MFA,
5. TLS wszędzie, bezpieczne nagłówki (CSP, HSTS, X-Content-Type-Options),
6. aktualizacje zależności i obrazów, SBOM,
7. logowanie i monitoring, WAF jako dodatkowa warstwa,
8. testy (SAST/DAST/pentest), **security by design**.

## Podsumowanie

- Główne ataki: **SQL Injection** (obrona: zapytania parametryzowane, walidacja, ORM, najmniejsze uprawnienia bazy), **XSS** (kodowanie wyjścia, CSP, frameworki), **CSRF** (tokeny, SameSite), **ataki na sesje** (HttpOnly/Secure, timeouty, regeneracja ID), a także Broken Access Control, SSRF, command injection, path traversal.
- Warstwy: bezpieczny kod, **WAF**, **TLS**, bezpieczne kontenery, testy (pentest, fuzzing), monitoring (SIEM) – zgodnie z **OWASP Top 10** (2021/2025).

---
[⬅️ Poprzedni temat](8_Zastosowanie_kryptografii_w_ochronie_danych_i_komunikacji.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Bezpieczeństwo_podstawowych_usług_sieci_lokalnej_DHCP_DNS_NAT_HTTP_FTP.md)