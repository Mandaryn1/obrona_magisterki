# Czym jest MFA w kontekście chmury?

**MFA (Multi-Factor Authentication)**, czyli uwierzytelnianie wieloskładnikowe, wymaga potwierdzenia tożsamości **co najmniej dwoma niezależnymi czynnikami** z różnych kategorii:

- **coś, co wiesz:** hasło, PIN,
- **coś, co masz:** aplikacja z kodami (TOTP), klucz sprzętowy (FIDO2/YubiKey), telefon,
- **coś, czym jesteś:** biometria (odcisk palca, twarz).

**Dlaczego jest ważne w chmurze:** dostęp do zasobów odbywa się przez Internet z dowolnego miejsca, więc samo hasło jest zbyt słabą ochroną. Przejęcie konta jest jednym z głównych zagrożeń chmury. Przy MFA **wyciek lub kradzież hasła nie wystarcza** do zalogowania. Chroni to przed phishingiem, atakami słownikowymi i brute force oraz credential stuffing (użycie haseł z wycieków).

**Zastosowanie:** MFA włącza się dla wszystkich użytkowników, a **obowiązkowo dla administratorów i konta głównego (root)**, przy logowaniu do konsoli zarządzania, przy operacjach krytycznych oraz dostępie zdalnym (VPN). Często stosuje się **dostęp warunkowy**: dodatkowy czynnik wymagany, gdy logowanie jest z nowego urządzenia lub lokalizacji.

**Siła metod:** najsilniejsze są **klucze sprzętowe FIDO2** (odporne na phishing), potem aplikacje TOTP. **SMS jest najsłabszy** (ryzyko przejęcia numeru, SIM swapping).

**Wyzwania:** utrata urządzenia, wygoda użytkowników i koszty, dlatego potrzebne są kody zapasowe i procedury odzyskiwania.

MFA to jedno z najskuteczniejszych i najtańszych zabezpieczeń kont w chmurze.

## Podsumowanie

- MFA = co najmniej **dwa różne czynniki**: wiedza (hasło), posiadanie (token), cecha (biometria).
- W chmurze chroni przed skutkami wycieku hasła i przejęciem kont; **obowiązkowe dla administratorów i kont uprzywilejowanych**.
- **SMS/e-mail słabsze** niż aplikacje i tokeny sprzętowe; trzeba zapewnić **procedury awaryjne**.
- Można je zapewnić przez dostawcę chmury lub zewnętrzne rozwiązania.

---
[⬅️ Poprzedni temat](6_IAM_w_bezpieczeństwie_chmurowym.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Mechanizmy_RBAC_i_ABAC.md)