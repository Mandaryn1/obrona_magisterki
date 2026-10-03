# Czym są mechanizmy RBAC i ABAC?

**RBAC (Role-Based Access Control)** to kontrola dostępu oparta na **rolach**. Uprawnienia przypisuje się rolom (np. administrator, programista, księgowy), a role użytkownikom. Użytkownik dziedziczy uprawnienia swojej roli. W chmurze są to np. role w AWS IAM, Azure RBAC czy Kubernetes. Model jest **prosty, przejrzysty i łatwy do audytu**, dobrze realizuje zasadę najmniejszych uprawnień, ale może być mało elastyczny. Przy wielu niuansach pojawia się **„eksplozja ról"**.

**ABAC (Attribute-Based Access Control)** to kontrola dostępu oparta na **atrybutach**. Decyzję podejmuje polityka, która ocenia atrybuty:

- użytkownika (dział, stanowisko, poziom uprawnień),
- zasobu (typ, klasyfikacja, właściciel, tag),
- kontekstu (czas, lokalizacja, urządzenie, poziom ryzyka).

Przykład reguły: „dostęp do danych ma pracownik działu finansów, tylko w godzinach pracy i z firmowego urządzenia". W chmurze tak działają np. tagi w AWS (dostęp, gdy tag użytkownika = tag zasobu). ABAC jest **bardzo elastyczny i szczegółowy (dynamiczny)** oraz dobrze skaluje się w dużych środowiskach, ale jest **bardziej złożony** w projektowaniu, wdrażaniu i audycie.

**Różnica:** w RBAC pytamy „jaką ma rolę?", a w ABAC „jakie ma atrybuty i w jakich okolicznościach próbuje dostępu?".

W praktyce często łączy się oba podejścia: role jako baza, a atrybuty jako dodatkowe warunki.

## Podsumowanie

- **RBAC** – uprawnienia przypisane do **ról**, role do użytkowników; **prosty**, dobry przy stałych, dobrze określonych rolach; mniej elastyczny.
- **ABAC** – decyzje na podstawie **atrybutów** użytkownika, zasobu i środowiska; **elastyczny i złożony**, dobry dla dynamicznych środowisk **cloud-native**.
- Często stosowane razem; oba realizują zasadę najmniejszych uprawnień.

---
[⬅️ Poprzedni temat](7_MFA_w_kontekście_chmury.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](9_Szyfrowanie_danych_w_spoczynku_i_w_tranzycie.md)