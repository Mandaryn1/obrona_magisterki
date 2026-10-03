# Najczęstsze ataki na aplikacje webowe oraz podstawowe mechanizmy ich ograniczania

## 1. Specyfika bezpieczeństwa aplikacji webowych

* **Model architektoniczny:** Aplikacje webowe opierają się zazwyczaj na interfejsie przeglądarkowym, serwerze WWW oraz bazodanowym zapleczu (*back-end database*), co stwarza unikalną powierzchnię ataku opartą na przetwarzaniu danych wejściowych od użytkowników.
* **Standardy i wytyczne:** Podstawowym punktem odniesienia do analizy i testowania zabezpieczeń aplikacji webowych są zestawienia **OWASP Top 10** oraz przewodnik **OWASP Web Security Testing Guide (WSTG)**, określający metodyki wykrywania i zapobiegania podatnościom.

---

## 2. Najczęstsze ataki na aplikacje webowe

1. **Iniekcja SQL (SQL Injection — SQLi):**
   * **Mechanizm:** Wstrzyknięcie złośliwego kodu SQL do pól formularzy lub parametrów URL nieprawidłowo walidowanych przez aplikację.
   * **Skutek:** Modyfikacja zapytania bazy danych, co pozwala atakującemu na odczyt poufnych danych, ich usunięcie, a nawet przejęcie kontroli nad serwerem bazodanowym.

2. **Skrypty Międzywitrynowe (Cross-Site Scripting — XSS):**
   * **Mechanizm:** Wstrzyknięcie złośliwego kodu JavaScript do treści strony docelowej.
   * **Odmiany:**
     * **Reflected XSS:** Złośliwy skrypt wykonuje się natychmiast w przeglądarce po kliknięciu wygenerowanego linku.
     * **Stored XSS:** Skrypt zostaje trwale zapisany w bazie danych aplikacji (np. w komentarzu) i wykonuje się u każdego odwiedzającego.
     * **DOM-based XSS:** Manipulacja strukturą DOM strony po stronie klienta.
   * **Skutek:** Kradzież ciasteczek sesyjnych (*session hijacking*), przejmowanie kont oraz podstawianie fałszywych formularzy.

3. **Fałszerstwo Żądań Międzywitrynowych (Cross-Site Request Forgery — CSRF):**
   * **Mechanizm:** Zmuszenie przeglądarki zalogowanej ofiary do wysłania nieautoryzowanego żądania HTTP do podatnej aplikacji (np. zmiana adresu e-mail lub przelew bankowy) bez jej wiedzy.

4. **Złamanie Kontroli Dostępu (Broken Access Control):**
   * **Mechanizm:** Błędy w autoryzacji pozwalające użytkownikowi na dostęp do zasobów lub funkcji poza jego uprawnieniami (np. modyfikacja ID w adresie URL — *IDOR / Insecure Direct Object References*).

5. **Zewnętrzne Encje XML (XXE) oraz Fałszowanie Żądań po Stronie Serwera (SSRF):**
   * **XXE:** Atak na parsery XML pozwalający na odczyt plików lokalnych serwera lub wykonanie ataków sieciowych.
   * **SSRF:** Zmuszenie serwera aplikacji do wysłania nieautoryzowanych żądań HTTP do wewnętrznej sieci organizacji.

---

## 3. Podstawowe mechanizmy ograniczania zagrożeń (Mitigation)

* **Zapytania Parametryzowane (Prepared Statements):**
  * Stosowanie instrukcji przygotowanych i mapowania obiektowo-relacyjnego (ORM). Całkowicie oddziela kod SQL od danych wprowadzanych przez użytkownika, neutralizując SQLi.
* **Walidacja i Czyszczenie Danych Wejściowych (Input Validation & Sanitization):**
  * Stosowanie białych list (*allow lists*) dopuszczalnych znaków i formatów danych.
* **Kodowanie Danych Wyjściowych (Output Encoding / Escaping):**
  * Zamiana znaków specjalnych (np. `<`, `>`, `"`, `'`) na ich bezpieczne encje HTML przed wyrenderowaniem strony, co blokuje wykonanie skryptów XSS.
* **Tokeny CSRF (Anti-CSRF Tokens):**
  * Generowanie unikalnych, losowych i jednorazowych tokenów przypisanych do sesji użytkownika i weryfikowanych przy każdym żądaniu zmieniającym stan aplikacji.
* **Zapora Aplikacji Webowych (WAF — Web Application Firewall):**
  * Dedykowane rozwiązanie chroniące aplikację webową poprzez inspekcję i filtrowanie ruchu HTTP/HTTPS oraz blokowanie znanych wzorców ataków (np. SQLi, XSS) przed dotarciem do serwera.
* **Flagi bezpiecznych ciasteczek (Secure Cookie Flags):**
  * Ustawianie atrybutów `HttpOnly` (blokuje dostęp do ciasteczka z poziomu JavaScript) oraz `SameSite=Strict/Lax` (chroni przed CSRF).

---

## 4. Narzędzia do testowania aplikacji webowych

* **Interception Proxies:** Burp Suite oraz OWASP ZAP — umożliwiają przechwytywanie, analizę i modyfikację ruchu HTTP/HTTPS między przeglądarką a aplikacją.
* **Automatyczne skanery:** Skanery podatności sieciowych i aplikacji webowych (np. Nikto) służące do szybkiego wykrywania znanych luk i błędnej konfiguracji serwerów.

---

## 5. Podsumowanie do wypowiedzi na obronie

> *"Najczęstszymi atakami na aplikacje webowe są ataki typu Injection (np. SQLi), XSS, CSRF oraz błędna kontrola dostępu. Przed atakami typu SQLi chroni stosowanie zapytań parametryzowanych (Prepared Statements), przed XSS — kodowanie danych wyjściowych i czyszczenie wejścia, a przed CSRF — unikalne tokeny anty-CSRF oraz atrybuty SameSite w ciasteczkach. Dodatkowo bezpieczeństwo warstwy aplikacji wzmacnia wdrożenie zapory WAF oraz regularne testy z użyciem narzędzi takich jak Burp Suite czy OWASP ZAP w oparciu o wytyczne OWASP WSTG."*

## Podsumowanie

- Główne ataki: **SQL Injection** (obrona: zapytania parametryzowane, walidacja, ORM, najmniejsze uprawnienia bazy), **XSS** (kodowanie wyjścia, CSP, frameworki), **CSRF** (tokeny, SameSite), **ataki na sesje** (HttpOnly/Secure, timeouty, regeneracja ID), a także Broken Access Control, SSRF, command injection, path traversal.
- Warstwy: bezpieczny kod, **WAF**, **TLS**, bezpieczne kontenery, testy (pentest, fuzzing), monitoring (SIEM) – zgodnie z **OWASP Top 10** (2021/2025).

---
[⬅️ Poprzedni temat](8_Zastosowanie_kryptografii_w_ochronie_danych_i_komunikacji.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Bezpieczeństwo_podstawowych_usług_sieci_lokalnej_DHCP_DNS_NAT_HTTP_FTP.md)