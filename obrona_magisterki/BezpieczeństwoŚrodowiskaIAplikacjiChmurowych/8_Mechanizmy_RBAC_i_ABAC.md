# Mechanizmy RBAC i ABAC

## Wprowadzenie

**RBAC** i **ABAC** to dwa modele **autoryzacji** (kontroli dostępu) stosowane w zarządzaniu uprawnieniami w chmurze (slajd 17), będące częścią IAM (temat 6). Odpowiadają na pytanie: *co wolno uwierzytelnionemu podmiotowi?*

## RBAC – Role-Based Access Control

**RBAC (kontrola dostępu oparta na rolach)** – uprawnienia przypisuje się **do ról**, a role **do użytkowników**. Użytkownik uzyskuje uprawnienia przez przynależność do roli.

```
 użytkownik ──▶ rola ──▶ uprawnienia ──▶ zasoby
   Anna         Admin       read/write/delete   baza produktów
   Jan          Viewer      read                baza produktów
```

- **Zalety (wykład):** **prostszy w implementacji i zarządzaniu**; szczególnie skuteczny tam, gdzie role i odpowiedzialności są **dobrze zdefiniowane**.
- **Wady:** **mniej elastyczny**; przy wielu wyjątkach i kontekstach prowadzi do **„eksplozji ról"** (setki wąsko wyspecjalizowanych ról); nie uwzględnia kontekstu (czas, lokalizacja, właściciel zasobu).
- Zgodny z zasadą **najmniejszych uprawnień** i **separacji obowiązków** (przy dobrze zaprojektowanych rolach).
- Przykłady: role w AWS IAM (role + polityki), **Azure RBAC** (Owner, Contributor, Reader), **Kubernetes RBAC** (`Role`, `ClusterRole`, `RoleBinding`, `ServiceAccount`), Spring Security (`hasRole('ADMIN')`, `@PreAuthorize`).

Przykład (Kubernetes) *(uzupełnienie)*:

```yaml
kind: Role
metadata: { name: pod-reader, namespace: catalog }
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
---
kind: RoleBinding
metadata: { name: read-pods, namespace: catalog }
subjects:
- kind: ServiceAccount
  name: monitoring-sa
roleRef: { kind: Role, name: pod-reader, apiGroup: rbac.authorization.k8s.io }
```

## ABAC – Attribute-Based Access Control

**ABAC (kontrola dostępu oparta na atrybutach)** – decyzję o dostępie podejmuje się na podstawie **atrybutów**:

- **użytkownika** (dział, stanowisko, poziom dostępu, kraj),
- **zasobu** (typ, właściciel, klasyfikacja danych, tag),
- **akcji** (odczyt, zapis),
- **środowiska/kontekstu** (czas, lokalizacja, adres IP, urządzenie, poziom ryzyka).

Reguła ma postać polityki, np.: *„Zezwól, jeśli `użytkownik.dział == zasób.dział` **i** `zasób.klasyfikacja != ściśle tajne` **i** godzina w 8:00–18:00 **i** urządzenie zarządzane."*

- **Zalety (wykład):** **bardziej elastyczny**; szczególnie użyteczny w środowiskach **cloud-native** (dynamiczne zasoby i użytkownicy), przy **dynamicznych wymaganiach dostępu i złożonych politykach** bezpieczeństwa; drobnoziarnisty (*fine-grained*), mniej ról.
- **Wady:** **bardziej złożony** (projektowanie polityk, zarządzanie atrybutami, trudniejszy audyt „kto ma dostęp do czego"); wymaga rzetelnych źródeł atrybutów; potencjalnie większy koszt obliczeniowy decyzji.
- Przykłady: **AWS – ABAC za pomocą tagów** (`aws:PrincipalTag`, `aws:ResourceTag`), Azure ABAC (warunki w przypisaniach ról), **Open Policy Agent (OPA)**, XACML.

Przykład (AWS ABAC) *(uzupełnienie)*:

```json
{
  "Effect": "Allow",
  "Action": ["ec2:StartInstances", "ec2:StopInstances"],
  "Resource": "*",
  "Condition": {
    "StringEquals": { "aws:ResourceTag/projekt": "${aws:PrincipalTag/projekt}" }
  }
}
```

(użytkownik może uruchamiać tylko maszyny z tym samym tagiem `projekt`, który ma on sam).

### Architektura ABAC (XACML) *(uzupełnienie)*

- **PEP** (Policy Enforcement Point) – egzekwuje decyzję,
- **PDP** (Policy Decision Point) – podejmuje decyzję (permit/deny),
- **PIP** (Policy Information Point) – dostarcza atrybuty,
- **PAP** (Policy Administration Point) – zarządza politykami.

## Porównanie

| Cecha | **RBAC** | **ABAC** |
| :--- | :--- | :--- |
| Podstawa decyzji | **rola** użytkownika | **atrybuty** (użytkownik, zasób, akcja, środowisko) |
| Złożoność | **prosty** | **złożony** |
| Elastyczność | niższa | **wysoka**, kontekstowa |
| Skalowalność przy zmianach | „eksplozja ról" | polityki oparte na atrybutach |
| Dostęp zależny od kontekstu (czas, lokalizacja) | nie (bez rozszerzeń) | **tak** |
| Audyt „kto ma dostęp" | łatwy (lista ról i członków) | trudniejszy (trzeba analizować polityki) |
| Najlepsze zastosowanie | stałe role, jasna struktura organizacji | dynamiczne środowiska cloud-native, złożone polityki, wiele tenantów |
| Zasada | uprawnienie ⟵ rola | uprawnienie ⟵ polityka(atrybuty) |

## Model hybrydowy

W praktyce często łączy się oba podejścia: **RBAC jako szkielet** (podstawowe role) uzupełniony **atrybutami (ABAC)** do doprecyzowania dostępu (np. rola „Menedżer" + atrybut „dział = sprzedaż" + „godziny pracy"). Inne modele: **DAC** (uznaniowa), **MAC** (obowiązkowa), **ReBAC** (oparta na relacjach), **PBAC** (oparta na politykach).

## Zasady stosowania w chmurze

- **najmniejsze uprawnienia** – role/polityki jak najwęższe,
- rozdzielenie ról administracyjnych (**separacja obowiązków**), brak kont współdzielonych,
- regularne **przeglądy** przypisań ról,
- uprawnienia do **tożsamości usług** (np. `ServiceAccount` + `Role/RoleBinding` w Kubernetes),
- **polityki jako kod** (policy-as-code: OPA/Rego, Terraform) i audyt zmian,
- logowanie decyzji dostępowych (SIEM).

## Podsumowanie

- **RBAC** – uprawnienia przypisane do **ról**, role do użytkowników; **prosty**, dobry przy stałych, dobrze określonych rolach; mniej elastyczny.
- **ABAC** – decyzje na podstawie **atrybutów** użytkownika, zasobu i środowiska; **elastyczny i złożony**, dobry dla dynamicznych środowisk **cloud-native**.
- Często stosowane razem; oba realizują zasadę najmniejszych uprawnień.

---
[⬅️ Poprzedni temat](7_MFA_w_kontekście_chmury.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](9_Szyfrowanie_danych_w_spoczynku_i_w_tranzycie.md)