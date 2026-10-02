# Mechanizmy uwierzytelniania: hasła, klucze SSH, uwierzytelnianie dwuskładnikowe, IAM i SSO

> Wykład: MBK1 (slajdy 7–8, 14–22: uwierzytelnianie, hasła, tokeny, biometria, MFA; 29–39: IAM, standardy, SSO, Kerberos), W8 (slajdy 19–21, 32). Klucze SSH i szczegóły techniczne – ***(uzupełnienie)***.

## Uwierzytelnianie – czynniki (wykład, slajdy 7–8, 14–17)

**Uwierzytelnianie** potwierdza tożsamość – „kim jesteś?"; jest warunkiem autoryzacji.

| Czynnik | Przykłady |
| :--- | :--- |
| **coś, co wiesz** | hasło, PIN, fraza |
| **coś, co masz** | token sprzętowy (YubiKey, Google Titan, smart card), aplikacja z kodami (Authenticator), telefon |
| **coś, czym jesteś** | biometria: odcisk palca, twarz (Windows Hello 3D), tęczówka/siatkówka |
| *(uzup.)* **gdzie jesteś / kontekst** | lokalizacja, urządzenie, godzina |

## 1. Hasła (wykład, slajd 15)

Najpopularniejsza metoda (prosta), ale **słabe hasła są głównym zagrożeniem**.

- **silne hasło:** minimum **12–16 znaków**, mieszanka wielkich i małych liter, cyfr, znaków specjalnych, brak słów słownikowych i sekwencji, **unikalność** dla każdego systemu; wykład W8 (slajd 19): zasady bezpiecznego zarządzania hasłami; *(uzup.: NIST SP 800-63B – nacisk na długość i listy zabronionych haseł, brak wymuszonej cyklicznej zmiany bez powodu)*,
- **zagrożenia:** **ataki słownikowe** (ochrona: złożoność, lista zabronionych), **brute force** (blokada konta po N próbach, opóźnienia), **phishing** (edukacja, MFA), *(uzup.)* credential stuffing, password spraying, keylogger, przechwycenie (sniffing), **pass-the-hash**.

### Przechowywanie haseł *(uzupełnienie)*

Nigdy jawnie. **Skrót z solą i wolną funkcją KDF**: Linux `/etc/shadow` – `$6$` (SHA-512-crypt), `$y$` (yescrypt), bcrypt, scrypt, **Argon2id**; Windows – **NTLM hash** (MD4 bez soli – słaby), **Kerberos**, hasła domenowe w `NTDS.dit`; **Credential Guard** chroni sekrety w pamięci LSASS. Ochrona przed brute force (wykład W8, slajd 32): **Fail2Ban** (blokada IP, integracja z iptables/firewalld), **Account Lockout Policy** (GPO), **rate limiting**, **CAPTCHA**.

## 2. Klucze SSH *(uzupełnienie)*

**SSH** – bezpieczna zdalna powłoka (zastępuje Telnet, port 22). Uwierzytelnianie **kluczem publicznym** zamiast hasła:

```
 klient: klucz PRYWATNY (~/.ssh/id_ed25519)        serwer: klucz PUBLICZNY w ~/.ssh/authorized_keys
   └── podpisuje wyzwanie ───────────────────────▶ weryfikuje podpis → dostęp (hasło nie jest przesyłane)
```

```bash
ssh-keygen -t ed25519 -C "anna@stacja"      # generowanie pary kluczy (z passphrase!)
ssh-copy-id anna@serwer                     # instalacja klucza publicznego
```

Konfiguracja serwera (`/etc/ssh/sshd_config`): `PasswordAuthentication no`, `PermitRootLogin no`, `PubkeyAuthentication yes`, `AllowUsers`, `MaxAuthTries`, wersja 2 protokołu; dodatkowo **fail2ban**, **zmiana domyślnego portu** (słaba osłona), **certyfikaty SSH** i **agent** (`ssh-agent`), **klucze FIDO2** (`ed25519-sk`). Zalety: odporność na brute force i phishing haseł, automatyzacja; wady: konieczność ochrony klucza prywatnego (passphrase, HSM/TPM – wykład MBK1, slajd 45: *TPM jako provider kluczy SSH*), zarządzanie kluczami (rotacja, odbieranie). Wykład W2: **używać SSH i wyłączyć logowanie roota** (slajd 28), **przechwytywanie kluczy SSH w laboratorium Nmap**.

## 3. Uwierzytelnianie wieloskładnikowe (MFA/2FA) – wykład, slajdy 18–22

**MFA** wymaga **co najmniej dwóch różnych czynników**; **nawet skompromitowane hasło nie wystarczy** (ochrona przed phishingiem i **credential stuffing**; compliance: RODO, PCI DSS, HIPAA; próby użycia MFA sygnalizują kompromitację hasła).

| Metoda | Opis | Bezpieczeństwo |
| :--- | :--- | :--- |
| **TOTP/HOTP** (aplikacje: Google/Microsoft Authenticator, Authy) | 6-cyfrowy kod czasowy (30 s) | wysokie |
| **klucze sprzętowe FIDO2/WebAuthn, YubiKey, smart card** | kryptografia klucza publicznego; wiązanie z domeną | **najwyższe, odporne na phishing** |
| **powiadomienia push** (z dopasowaniem numeru) | zatwierdzenie na telefonie | wysokie/średnie (MFA fatigue) |
| **SMS/e-mail** | kod | **najsłabsze** (SIM swapping) |
| **biometria** (+ TPM) | lokalna weryfikacja | zależnie od implementacji |

**Windows (slajd 20):** **Windows Hello for Business** (biometria + PIN zabezpieczony **TPM**; klucze nie opuszczają chipu), **Smart Card**, **Azure AD MFA**; wymuszanie przez GPO; przykład: hasło AD → kod SMS/Authenticator → zatwierdzenie → dostęp.

**Linux (slajd 21):** **PAM** (Pluggable Authentication Modules) – elastyczna architektura; moduły **`pam_google_authenticator`** (TOTP: `apt-get install libpam-google-authenticator`, `google-authenticator`) i **`pam_yubico`** (YubiKey OTP); łączenie modułów w stos (`/etc/pam.d/sshd`, `/etc/pam.d/login`).

**Zalety i wyzwania (slajd 22):** wzrost bezpieczeństwa, ochrona przed phishingiem, zgodność, detekcja włamań; **wyzwania**: niedogodność dla użytkowników, **utrata dostępu** (zgubiony telefon/token – potrzebne **procedury awaryjne**: kody zapasowe), koszty (tokeny, licencje, helpdesk), problemy techniczne, opór użytkowników. Wykład W8: MFA – przykłady wdrożeń w Windows i Linux (slajdy 20–21).

## 4. IAM – Identity and Access Management (wykład, slajdy 29–33)

**IAM** – zarządzanie tożsamością i dostępem: tworzenie, utrzymanie i usuwanie tożsamości, uwierzytelnianie, autoryzacja, audyt (cykl *joiner–mover–leaver*).

| Windows (slajd 31) | Linux (slajd 32) |
| :--- | :--- |
| **Active Directory** – jednolite zarządzanie kontami, **polityki haseł** (długość, złożoność, wygasanie), **delegacja uprawnień administratorskich**, **replikacja** między kontrolerami domeny | **LDAP** (OpenLDAP, 389 Directory Server) – katalog użytkowników, **Kerberos** (MIT/Heimdal) – bilety i SSO, **SSSD** – demon pośredniczący między Linuksem a AD/LDAP/Kerberos |
| **Azure AD** (obecnie **Microsoft Entra ID**) – chmurowy IAM: tożsamość **hybrydowa** (Azure AD Connect), **Conditional Access** (polityki z kontekstem: lokalizacja, urządzenie, ryzyko; wymuszanie MFA), **Identity Protection** (wykrywanie anomalii przez ML) | typowa konfiguracja enterprise: **SSSD + LDAP + Kerberos**; `realm join domena.com` dołącza Linuksa do AD |

**Standardy IAM (slajd 33):** **SAML 2.0** (XML, uwierzytelnienie i autoryzacja między domenami, SSO aplikacji enterprise), **OAuth 2.0** (autoryzacja delegowana – dostęp aplikacji do zasobów bez ujawniania hasła; API), **OpenID Connect** (warstwa uwierzytelniania nad OAuth 2.0; SSO aplikacji webowych i mobilnych). **Federacja tożsamości** – logowanie kontem z własnej organizacji w serwisach zewnętrznych.

*(uzupełnienie)* Praktyki IAM: **PAM** (zarządzanie dostępem uprzywilejowanym), **just-in-time**, przeglądy dostępu, RBAC/ABAC, zasada najmniejszych uprawnień, centralne logi, automatyczne **offboarding**.

## 5. SSO – Single Sign-On (wykład, slajdy 34–39)

**SSO** – jedno logowanie, dostęp do wielu aplikacji. Cele: **mniej haseł**, wyższa produktywność (oszczędność nawet ok. 30 minut dziennie na pracownika wg wykładu), scentralizowane zarządzanie, lepsza kontrola i audyt, mniej zgłoszeń helpdesku, **natychmiastowa dezaktywacja** dostępu.

### Windows – Kerberos (slajd 36)

Kerberos (MIT) używa **biletów (tickets)** zamiast przesyłania haseł: 1) **logowanie** do domeny z hasłem, 2) **KDC** wydaje **TGT** (Ticket Granting Ticket – ważny **10 h**), 3) przy dostępie do usługi TGT służy do uzyskania **biletu serwisowego**, 4) usługa weryfikuje bilet i przyznaje dostęp. Bilety szyfrowane, o ograniczonym czasie życia. ***(uzup.)*** Wymaga **synchronizacji czasu**; ataki: **pass-the-ticket, Kerberoasting, Golden/Silver Ticket**.

### Linux (slajd 37)

Kerberos + LDAP, często z AD: `krb5-user`, `/etc/krb5.conf` (adres KDC), `realm join`, `sssd`; `kinit user@DOMENA.COM`, `klist`.

### Mechanizm (slajd 38)

1) **logowanie początkowe** w centralnym **Identity Provider**, 2) **wydanie tokenu/biletu**, 3) **przekazanie do aplikacji** (cookie, nagłówek HTTP), 4) **weryfikacja** (z IdP lub podpis kryptograficzny), 5) dostęp bez ponownego logowania; token ważny typowo kilka godzin.

### Zalety i ryzyka (slajd 39)

| Zalety | Ryzyka |
| :--- | :--- |
| wygoda, produktywność, jedno silne hasło, mniej helpdesku, lepszy audyt, łatwe zarządzanie dostępem | **Single Point of Failure**, **koncentracja ryzyka** (przejęcie jednego konta = dostęp do wszystkiego), złożona integracja starszych aplikacji, zależność od sieci i IdP, koszt licencji |

**Mitygacja:** **obowiązkowe MFA** dla kont SSO, **wysoka dostępność** infrastruktury uwierzytelniania, mechanizmy awaryjnego dostępu, **monitoring anomalii**.

## Zbiorcze porównanie

| Mechanizm | Siła | Główne słabości | Zalecenie |
| :--- | :--- | :--- | :--- |
| hasło | niska–średnia | słabe, phishing, brute force | długie, unikalne, KDF, blokady + MFA |
| klucz SSH | wysoka | wyciek klucza prywatnego | passphrase, FIDO2/TPM, rotacja |
| TOTP | wysoka | phishing w czasie rzeczywistym | + ochrona przed AiTM |
| FIDO2/passkeys | najwyższa | zarządzanie urządzeniami | dla administratorów i usług krytycznych |
| biometria | zależna | nieodwracalność wzorca | tylko lokalnie (TPM) |
| SSO + MFA | wysoka + wygoda | koncentracja ryzyka | HA i monitoring |

## Podsumowanie

- Uwierzytelnianie: **wiem / mam / jestem**; hasła (silne, unikalne, KDF, ochrona przed brute force: Fail2Ban, lockout), **klucze SSH** (bez haseł, bez roota), **MFA** (TOTP, FIDO2, PAM, Windows Hello+TPM) – najważniejsza obrona przed przejęciem haseł.
- **IAM** (AD/Entra ID, LDAP/Kerberos/SSSD; SAML, OAuth 2.0, OIDC) centralizuje tożsamości; **SSO** (Kerberos TGT 10 h, tokeny) – wygoda i audyt, ale koncentracja ryzyka → MFA, HA, monitoring.

---
[⬅️ Poprzedni temat](5_Modele_kontroli_dostępu_DAC_MAC_i_RBAC.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Zapobieganie_i_wykrywanie_zagrożeń_w_systemach_operacyjnych.md)