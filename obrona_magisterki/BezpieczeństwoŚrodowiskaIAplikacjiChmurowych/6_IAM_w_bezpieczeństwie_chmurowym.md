# IAM (Identity and Access Management) w bezpieczeństwie chmurowym

## Definicja

**IAM (Identity and Access Management – zarządzanie tożsamością i dostępem)** to zestaw procesów, polityk i technologii, które zapewniają, że **właściwe podmioty (osoby, aplikacje, usługi) mają dostęp do właściwych zasobów, w odpowiednim czasie i w niezbędnym zakresie**.

Wg wykładu (slajd 14): **IAM jest fundamentem bezpieczeństwa chmury**. W chmurze jest często **głównym punktem wejścia** do zasobów – kto kontroluje tożsamość, kontroluje zasoby. To ona zastępuje tradycyjny perymetr sieciowy.

## Co obejmuje IAM

| Element | Znaczenie |
| :--- | :--- |
| **Identyfikacja** | kim jest podmiot (login, identyfikator) |
| **Uwierzytelnianie** (authentication) | potwierdzenie tożsamości (hasło, **MFA**, certyfikat, token) |
| **Autoryzacja** (authorization) | co wolno uwierzytelnionemu podmiotowi (role, polityki: **RBAC/ABAC**) |
| **Zarządzanie użytkownikami** | tworzenie, zmiana, usuwanie kont; cykl życia (*joiner–mover–leaver*) |
| **Audyt dostępu** (accounting) | rejestracja prób dostępu i działań |

## Typy tożsamości w chmurze

- **użytkownicy** (pracownicy, administratorzy, klienci),
- **aplikacje i usługi** (konta usługowe, *service accounts*, *managed identities*, role instancji, tokeny) – **w mikroserwisach każda usługa może wymagać własnej tożsamości**,
- *(uzupełnienie)* **urządzenia** i **potoki CI/CD** (tożsamość workloadu, OIDC).

Tożsamości niebędące ludźmi bywają bardziej liczne niż ludzkie i często **przesadnie uprzywilejowane**.

## Kluczowe zasady IAM (wykład)

1. **Najmniejsze uprawnienia** (*least privilege*) – tylko niezbędne uprawnienia, na niezbędny czas.
2. **Separacja obowiązków** (*separation of duties*) – kluczowe operacje wymagają więcej niż jednej osoby/roli (np. ten, kto tworzy, nie zatwierdza).
3. **Regularne przeglądy dostępu** (*access reviews/recertification*) – usuwanie zbędnych uprawnień.

## Funkcje IAM w chmurze (wykład)

### Federacja tożsamości i SSO (slajd 16)

- **Federacja tożsamości** – współdzielenie informacji o użytkownikach między domenami (te same dane uwierzytelniające do różnych systemów, integracja z lokalnymi systemami tożsamości, np. Active Directory).
- **SSO (Single Sign-On)** – dostęp do wielu aplikacji po jednym logowaniu; poprawia **UX i bezpieczeństwo** (mniej haseł).
- **Standardy:**
  - **SAML** – wymiana danych uwierzytelniających między domenami (XML, często dla aplikacji korporacyjnych),
  - **OAuth 2.0** – umożliwia aplikacji dostęp do zasobów **w imieniu użytkownika bez udostępniania poświadczeń** (autoryzacja delegowana),
  - **OpenID Connect (OIDC)** – rozszerza OAuth 2.0 o **uwierzytelnianie** (standardowa weryfikacja tożsamości; token ID/JWT).

### MFA (slajd 15) – zob. temat 7.

### Uprawnienia: RBAC i ABAC (slajd 17) – zob. temat 8.

### Klucze i certyfikaty (slajd 18)

Zarządzanie obejmuje **generowanie, przechowywanie, rotację i unieważnianie**; klucze **oddzielnie od zaszyfrowanych danych**; certyfikaty SSL/TLS zarządzane centralnie i odnawiane; bezpieczne przechowywanie w **HSM**.

### Audyt dostępu i monitorowanie (slajd 19)

- logi wszystkich prób dostępu: **kto, kiedy, do czego, jaka akcja, jaki wynik**,
- logi przechowywane w bezpiecznym miejscu i **chronione przed modyfikacją**,
- monitorowanie w czasie rzeczywistym i **alerty**; **SIEM** (korelacja zdarzeń); automatyzacja analizy logów.

### Zarządzanie sesjami (slajd 20)

Tworzenie, monitorowanie i kończenie sesji; **timeout sesji**; zarządzanie sesjami API i tokenami (krótki czas życia – TTL); procedury awaryjne natychmiastowego zakończenia sesji.

## IAM u dostawców i w ekosystemie

- **AWS IAM**, **Microsoft Entra ID (Azure AD)** + RBAC, **Google Cloud IAM**; dla własnych aplikacji: **Keycloak** (OIDC/SAML), **Spring Security** (Resource Server, JWT), Vault (sekrety).
- Przykład uprawnień *(uzupełnienie)*: polityka AWS dająca tylko odczyt z jednego bucketu:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["s3:GetObject"],
    "Resource": "arn:aws:s3:::catalog-data/*"
  }]
}
```

## Dobre praktyki IAM *(uzupełnienie)*

- **MFA** dla wszystkich, zwłaszcza kont uprzywilejowanych; **zakaz używania konta root** na co dzień,
- **brak stałych kluczy** dostępu – preferować **role i krótkotrwałe poświadczenia**,
- **PAM** (Privileged Access Management), **just-in-time access**,
- automatyczne **usuwanie kont** przy odejściu pracownika, okresowa recertyfikacja,
- **polityki warunkowe** (lokalizacja, urządzenie, ryzyko),
- centralizacja tożsamości (jeden IdP) i SSO,
- alerty na zmiany uprawnień i użycie kont uprzywilejowanych.

## Typowe zagrożenia dla IAM

słabe hasła, brak MFA, wycieki kluczy (np. w repozytoriach), nadmierne uprawnienia, osierocone konta, eskalacja uprawnień, przejęcie sesji/tokenu, błędna konfiguracja federacji.

## Podsumowanie

- **IAM** = identyfikacja, uwierzytelnianie, autoryzacja, zarządzanie użytkownikami i audyt dostępu; **fundament bezpieczeństwa chmury**.
- W chmurze obsługuje **federację, SSO** (SAML, OAuth 2.0, OIDC) i **różne typy tożsamości** (ludzie, aplikacje, usługi).
- Zasady: **najmniejsze uprawnienia, separacja obowiązków, regularne przeglądy dostępu**; uzupełnione **MFA, RBAC/ABAC, zarządzaniem sesjami, kluczami i audytem**.
