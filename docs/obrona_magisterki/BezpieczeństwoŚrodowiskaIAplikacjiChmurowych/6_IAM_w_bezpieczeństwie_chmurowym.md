# Czym jest IAM w bezpieczeństwie chmurowym?

**IAM (Identity and Access Management)** to zestaw zasad, procesów i narzędzi, które zarządzają **tożsamościami** (użytkownikami, usługami, aplikacjami) oraz ich **dostępem do zasobów chmury**. Odpowiada na pytania: kim jesteś (uwierzytelnianie), co możesz robić (autoryzacja) i co zrobiłeś (audyt).

W chmurze IAM jest **fundamentem bezpieczeństwa**, bo nie ma tradycyjnego perymetru sieci, a dostęp odbywa się przez Internet i API. To tożsamość staje się nową granicą ochrony, zgodnie z podejściem **Zero Trust**.

**Główne elementy:**

- **Uwierzytelnianie:** hasła, **MFA**, klucze, certyfikaty.
- **Autoryzacja:** uprawnienia przypisywane przez **role i polityki** (RBAC, ABAC).
- **Federacja i SSO:** logowanie jedną tożsamością do wielu usług (SAML, OAuth 2.0, OpenID Connect).
- **Zarządzanie cyklem życia:** tworzenie, zmiana i **natychmiastowe usuwanie** kont.
- **Audyt i logowanie:** kto, kiedy i do czego miał dostęp.

**Dobre praktyki:**

- **zasada najmniejszych uprawnień**,
- **MFA** dla wszystkich, szczególnie administratorów,
- rozdzielenie obowiązków,
- regularne przeglądy uprawnień,
- ograniczanie i rotacja kluczy dostępowych (nie używać konta głównego),
- monitoring.

Dobrze skonfigurowany IAM chroni przed przejęciem kont i nieautoryzowanym dostępem, czyli jednymi z najczęstszych zagrożeń chmury.

## Podsumowanie

- **IAM** = identyfikacja, uwierzytelnianie, autoryzacja, zarządzanie użytkownikami i audyt dostępu; **fundament bezpieczeństwa chmury**.
- W chmurze obsługuje **federację, SSO** (SAML, OAuth 2.0, OIDC) i **różne typy tożsamości** (ludzie, aplikacje, usługi).
- Zasady: **najmniejsze uprawnienia, separacja obowiązków, regularne przeglądy dostępu**; uzupełnione **MFA, RBAC/ABAC, zarządzaniem sesjami, kluczami i audytem**.

---
[⬅️ Poprzedni temat](5_Model_chmury_w_którym_dostawca_odpowiada_za_infrastrukturę_i_platformę.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️️](7_MFA_w_kontekście_chmury.md)