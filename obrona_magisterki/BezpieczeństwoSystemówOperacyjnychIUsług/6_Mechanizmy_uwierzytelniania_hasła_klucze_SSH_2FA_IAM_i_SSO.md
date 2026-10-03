# Mechanizmy uwierzytelniania: hasła, klucze SSH, uwierzytelnianie dwuskładnikowe, IAM i SSO

## 1. Hasła (Passwords)

* **Definicja:** Podstawowy i najpopularniejszy mechanizm uwierzytelniania oparty na wiedzy użytkownika („coś, co wiesz”).
* **Zagrożenia:** Wykorzystywanie słabych lub domyślnych haseł, ataki słownikowe i *Brute Force* oraz wycieki i zrzuty haseł z poprzednich naruszeń bezpieczeństwa (analizowane np. narzędziem `h8mail`).
* **Polityki haseł (Password Policies):** Wymuszanie przez zasady zabezpieczeń odpowiedniej długości, złożoności haseł, historii haseł, wygasania sesji oraz blokowania kont po wyznaczonej liczbie nieudanych prób logowania.

---

## 2. Klucze SSH (SSH Key-Based Authentication)

* **Definicja:** Bezpieczny mechanizm uwierzytelniania w protokole SSH (obsługującym domyślnie port 22) wykorzystujący kryptografię asymetryczną zamiast tradycyjnego hasła.
* **Zasada działania:**
  * Tworzona jest para kluczy: **klucz prywatny** (przechowywany bezpiecznie na urządzeniu klienta) oraz **klucz publiczny** (umieszczany na serwerze w pliku `~/.ssh/authorized_keys`).
  * Podpis cyfrowy wygenerowany kluczem prywatnym jest weryfikowany przez serwer kluczem publicznym.
* **Zalety:** Całkowita odporność na ataki *Brute Force* nakierowane na słabe hasła oraz brak przesyłania poświadczeń w sieci.

---

## 3. Uwierzytelnianie dwuskładnikowe i wieloskładnikowe (2FA / MFA)

* **Zasada działania:** Wymaganie od użytkownika przedstawienia co najmniej dwóch niezależnych dowodów tożsamości z różnych kategorii:
  1. **Wiedza** („coś, co wiesz” — np. PIN, hasło).
  2. **Posiadanie** („coś, co masz” — np. klucz sprzętowy YubiKey, aplikacja TOTP, token SMS/Push).
  3. **Cecha osobista** („coś, czym jesteś” — biometria: odcisk palca, skan twarzy).
* **Znaczenie:** Wymóg formalny wielu standardów bezpieczeństwa (np. PCI DSS, HIPAA); drastycznie ogranicza ryzyko przejęcia konta w przypadku wykradzenia samego hasła.

---

## 4. IAM (Identity and Access Management)

* **Definicja:** Kompleksowy system i ramy organizacyjne służące do centralnego zarządzania tożsamościami cyfrowymi, kontami użytkowników oraz ich uprawnieniami w całej infrastrukturze (lokalnej i chmurowej).
* **Główne funkcje:**
  * **Zarządzanie cyklem życia konta:** Tworzenie, modyfikacja i usuwanie dostępu (np. przy zatrudnieniu/zwolnieniu pracownika).
  * **Uwierzytelnianie (AuthN) i Autoryzacja (AuthZ):** Weryfikacja tożsamości oraz przyznawanie precyzyjnych praw dostępu na podstawie ról (RBAC) i zasady minimalnych uprawnień.
  * **Audytowalność:** Śledzenie i rejestrowanie wszystkich aktywności użytkowników w systemach.

---

## 5. SSO (Single Sign-On — Jednokrotne logowanie)

* **Definicja:** Mechanizm federacyjny umożliwiający użytkownikowi jednorazowe uwierzytelnienie się u centralnego dostawcy tożsamości (IdP) i uzyskanie dostępu do wielu niezależnych aplikacji i usług bez ponownego wpisywania poświadczeń.
* **Standardowe protokoły:** SAML 2.0, OAuth 2.0, OpenID Connect (OIDC), Kerberos (w środowiskach domeny Active Directory).
* **Zalety:** Wysoka wygoda użytkowników, redukcja liczby haseł do zapamiętania, centralne wymuszanie 2FA/MFA na poziomie dostawcy tożsamości.
* **Wada:** Ryzyko pojedynczego punktu awarii (*Single Point of Failure*) — przejęcie konta centralnego daje dostęp do wszystkich zintegrowanych systemów.

---

## 6. Podsumowanie do wypowiedzi na obronie

> *"Mechanizmy uwierzytelniania ewoluowały od podatnych na ataki haseł tekstowych do rozwiązań krypto-graficznych i wieloskładnikowych. Klucze SSH eliminują ryzyko złamania hasła w dostępie zdalnym, a uwierzytelnianie 2FA/MFA stanowi standard ochrony wymagany przez regulacje. W środowiskach korporacyjnych kluczowe jest wdrożenie systemów IAM do centralnego zarządzania tożsamościami oraz SSO, które umożliwia bezpieczne i wygodne jednokrotne logowanie do wielu usług za pomocą bezpiecznych protokołów federacyjnych."*

## Podsumowanie

- Uwierzytelnianie: **wiem / mam / jestem**; hasła (silne, unikalne, KDF, ochrona przed brute force: Fail2Ban, lockout), **klucze SSH** (bez haseł, bez roota), **MFA** (TOTP, FIDO2, PAM, Windows Hello+TPM) – najważniejsza obrona przed przejęciem haseł.
- **IAM** (AD/Entra ID, LDAP/Kerberos/SSSD; SAML, OAuth 2.0, OIDC) centralizuje tożsamości; **SSO** (Kerberos TGT 10 h, tokeny) – wygoda i audyt, ale koncentracja ryzyka → MFA, HA, monitoring.

---
[⬅️ Poprzedni temat](5_Modele_kontroli_dostępu_DAC_MAC_i_RBAC.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Zapobieganie_i_wykrywanie_zagrożeń_w_systemach_operacyjnych.md)