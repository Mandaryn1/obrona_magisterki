# MFA (Multi-Factor Authentication) w kontekście chmury

## Definicja

**MFA – uwierzytelnianie wieloskładnikowe** wymaga **co najmniej dwóch różnych czynników uwierzytelniania** pochodzących z **różnych kategorii** (slajd 15):

| Czynnik | Kategoria | Przykłady |
| :--- | :--- | :--- |
| **Coś, co wiesz** | wiedza | hasło, PIN, odpowiedź na pytanie |
| **Coś, co masz** | posiadanie | **token** sprzętowy (np. YubiKey), aplikacja uwierzytelniająca na telefonie, karta, kod SMS |
| **Coś, czym jesteś** | cecha biometryczna | odcisk palca, twarz, tęczówka |

*(uzupełnienie)* Czasem wymienia się czwarty czynnik: **gdzie jesteś** (lokalizacja/kontekst). **2FA** to szczególny przypadek MFA (dokładnie dwa składniki). Dwa hasła **nie** są MFA (ta sama kategoria).

## Dlaczego MFA jest ważne w chmurze

- W chmurze **tożsamość jest głównym punktem wejścia** do zasobów (temat 6) – a usługi są dostępne **z Internetu** (szeroki dostęp sieciowy, zob. cechy NIST), więc samo hasło jest zbyt słabą barierą.
- **MFA znacząco redukuje ryzyko przejęcia konta, nawet jeśli hasło zostanie skompromitowane** (phishing, wyciek, credential stuffing).
- Konta uprzywilejowane (administratorzy, root) mają ogromny potencjał szkód – ich przejęcie to najgorszy scenariusz.

## Zasady wdrożenia (wykład)

- MFA powinno obejmować **wszystkie krytyczne konta**, **szczególnie administratorów** i konta o wysokich uprawnieniach (w praktyce – **wszystkich użytkowników**, zwłaszcza dostęp zdalny i konsole zarządzania).
- W chmurze MFA może być zapewnione **przez dostawcę chmury** (AWS IAM MFA, Entra ID MFA, Google 2-Step Verification) lub **przez rozwiązania zewnętrzne** (Okta, Duo, Keycloak).
- **Różne typy MFA mają różny poziom bezpieczeństwa:** **SMS i e-mail są mniej bezpieczne** niż **dedykowane aplikacje lub sprzętowe tokeny**.
- Należy zapewnić **procedury awaryjne** na przypadek, gdy MFA jest niedostępne (zgubiony telefon, awaria) – np. kody zapasowe, zweryfikowany proces odzyskiwania (inaczej to „tylne drzwi").

## Metody MFA – ranking siły *(uzupełnienie)*

| Metoda | Bezpieczeństwo | Uwagi |
| :--- | :--- | :--- |
| **Klucze sprzętowe FIDO2/WebAuthn, passkeys** | **najwyższe, odporne na phishing** | uwierzytelnienie powiązane z domeną; polecane dla administratorów |
| **Aplikacje TOTP** (Google/Microsoft Authenticator) | wysokie | kody jednorazowe czasowe (30 s); podatne na phishing w czasie rzeczywistym |
| **Powiadomienia push z dopasowaniem numeru** | wysokie/średnie | ryzyko „zmęczenia MFA" bez dopasowania numeru |
| **SMS / połączenie głosowe** | niskie | podatne na *SIM swapping*, przechwycenie |
| **E-mail (kod)** | niskie | jak bezpieczne jest samo konto e-mail |

## Zagrożenia i ataki na MFA *(uzupełnienie)*

- **MFA fatigue / prompt bombing** – zalewanie użytkownika żądaniami zatwierdzenia,
- **AiTM phishing** (Adversary-in-the-Middle: Evilginx) – przechwycenie sesji po MFA,
- **SIM swapping**, przechwycenie SMS,
- **Kradzież tokenu sesji** (cookie) – obejście MFA,
- słabe procesy odzyskiwania konta, socjotechnika na helpdesk.

Przeciwdziałanie: **metody odporne na phishing (FIDO2)**, dopasowanie numeru, **polityki dostępu warunkowego**, krótkie sesje, monitoring anomalii logowania, szkolenia.

## MFA a pozostałe elementy IAM

- **Adaptacyjne (kontekstowe) MFA** – wymagane przy ryzykownych zdarzeniach (nowa lokalizacja, urządzenie, operacje wrażliwe).
- **SSO + MFA** – jedno silne uwierzytelnienie dostępne dla wielu aplikacji (federacja – SAML/OIDC).
- Dla **kont usługowych** MFA nie ma zastosowania – stosuje się certyfikaty, **krótkotrwałe tokeny i role** (workload identity).

## Podsumowanie

- MFA = co najmniej **dwa różne czynniki**: wiedza (hasło), posiadanie (token), cecha (biometria).
- W chmurze chroni przed skutkami wycieku hasła i przejęciem kont; **obowiązkowe dla administratorów i kont uprzywilejowanych**.
- **SMS/e-mail słabsze** niż aplikacje i tokeny sprzętowe; trzeba zapewnić **procedury awaryjne**.
- Można je zapewnić przez dostawcę chmury lub zewnętrzne rozwiązania.

---
[⬅️ Poprzedni temat](6_IAM_w_bezpieczeństwie_chmurowym.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Mechanizmy_RBAC_i_ABAC.md)