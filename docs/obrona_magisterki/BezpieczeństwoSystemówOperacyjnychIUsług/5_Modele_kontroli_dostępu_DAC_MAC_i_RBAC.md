# Modele kontroli dostępu: DAC, MAC i RBAC

## 1. Wstęp i definicja kontroli dostępu (Access Control)

Kontrola dostępu to fundamentalny mechanizm bezpieczeństwa w systemach operacyjnych i aplikacjach, który decyduje, czy dany podmiot (**Subject** — np. użytkownik, proces) może wykonać określoną operację (**Action** — np. odczyt, zapis, wykonanie) na konkretnym zasobie/obiekcie (**Object** — np. plik, katalog, urządzenie, usługa).

---

## 2. DAC (Discretionary Access Control — Uznaniowa kontrola dostępu)

* **Zasada działania:** Właściciel obiektu (twórca pliku lub zasobu) ma pełne uznanie (*discretion*) w zakresie decydowania, komu i w jakim zakresie nadaje lub odbiera uprawnienia.
* **Mechanizmy i przykłady:**
  * Uprawnienia plików w Linux (model **UGO** — User, Group, Others oraz polecenia `chmod`/`chown`).
  * Listy kontroli dostępu **ACL** (*Access Control Lists*) w systemie plików NTFS w Windows.
* **Zalety:** Bardzo wysoka elastyczność i prostota zarządzania w mniejszych środowiskach.
* **Wady / Zagrożenia:**
  * **Słaba ochrona przed złośliwym oprogramowaniem:** Jeśli złośliwy program (np. trojan) zostanie uruchomiony na koncie właściciela pliku, przejmuje jego uprawnienia i może zmienić ACL lub usunąć dane.
  * Brak kontroli nad dalszym rozpowszechnianiem informacji (użytkownik z prawem odczytu może skopiować plik i przyznać do niego dostęp wszystkim).

---

## 3. MAC (Mandatory Access Control — Obligatoryjna kontrola dostępu)

* **Zasada działania:** O dostępie decyduje odgórnie centralna polityka bezpieczeństwa systemu, egzekwowana przez jądro. Właściciel pliku **nie może** samoczynnie zmienić uprawnień ani przekazać dostępu innemu użytkownikowi.
* **Mechanizm etykiet:** Każdemu podmiotowi przypisuje się poziom czyszczenia/zaufania (*Clearance Level*), a każdemu obiektowi etykietę bezpieczeństwa (*Security Label* — np. *Jawne*, *Poufne*, *Tajne*).
* **Modele formalne:**
  * **Bell-LaPadula (Poufność):** Zasady *"No Read Up"* (brak odczytu z wyższego poziomu) oraz *"No Write Down"* (brak zapisu na niższy poziom, zapobiegający wyciekowi danych).
* **Przykłady implementacji:** **SELinux** (*Security-Enhanced Linux*), **AppArmor**, systemy wojskowe i rządowe o wysokich rygorach bezpieczeństwa.
* **Zalety:** Najwyższy poziom ochrony; odporność na nadużycia ze strony właścicieli zasobów oraz złośliwe oprogramowanie (proces o niskim poziomie nie uzyska dostępu do tajnych plików).
* **Wady:** Wysoki stopień skomplikowania konfiguracji i duży narzut administracyjny.

---

## 4. RBAC (Role-Based Access Control — Kontrola dostępu oparta na rolach)

* **Zasada działania:** Uprawnienia nie są przypisywane bezpośrednio użytkownikom ani tworzone dowolnie przez właścicieli, lecz są powiązane z **rolami organizacyjnymi** (np. *Administrator*, *Księgowy*, *Analityk SOC*). Użytkownicy są przypisywani do odpowiednich ról.
* **Zastosowanie i przykłady:**
  * **Active Directory (Windows Server):** Grupy zabezpieczeń i grupy ról.
  * Zarządzanie tożsamością i dostępowi (**IAM**) w chmurze oraz w środowiskach orkiestracji (np. Kubernetes RBAC).
* **Zalety:**
  * **Łatwość zarządzania dużą organizacją:** Zmiana stanowiska pracownika wymaga jedynie zmiany jego przypisania do roli, bez konieczności edycji uprawnień w tysiącach plików.
  * Wspiera **zasadę minimalnych uprawnień** (*Least Privilege*) oraz **rozdzielenie obowiązków** (*Separation of Duties*).

---

## 5. Zestawienie porównawcze modeli

| Cecha | DAC (Uznaniowa) | MAC (Obligatoryjna) | RBAC (Oparta na rolach) |
| :--- | :--- | :--- | :--- |
| **Kto decyduje o dostępie?** | Właściciel obiektu | Centralna polityka jądra SO | Rola w organizacji / Administrator |
| **Kryterium dostępu** | Tożsamość użytkownika / ACL | Etykiety i poziomy poufności | Rola i funkcje użytkownika |
| **Elastyczność** | Bardzo wysoka | Niska | Wysoka w zarządzaniu kadrami |
| **Poziom bezpieczeństwa** | Podstawowy / Średni | Bardzo wysoki | Wysoki |
| **Typowe zastosowanie** | Ogólne SO (Windows, Linux) | Wojskowość, SELinux, krytyczne SO | Korporacje, Active Directory, IAM |

---

## 6. Podsumowanie do wypowiedzi na obronie

> *"Modele kontroli dostępu określają zasady przyznawania uprawnień w systemach. DAC opiera się na uznaniu właściciela zasobu (np. chmod w Linux lub ACL w NTFS), co daje elastyczność, ale stwarza ryzyko. MAC narzuca odgórne etykiety bezpieczeństwa egzekwowane przez jądro (np. SELinux), uniemożliwiając właścicielowi zmianę praw i chroniąc przed wyciekiem danych. RBAC uzależnia uprawnienia od ról pełnionych w organizacji (np. w Active Directory lub IAM), co znacząco ułatwia zarządzanie uprawnieniami w dużych strukturach i wspiera zasadę minimalnych uprawnień."*

## Podsumowanie

- **DAC** – właściciel decyduje (ACL, prawa rwx), elastyczny, ale podatny na błędy i trojany.
- **MAC** – polityka centralna i etykiety (Bell-LaPadula – poufność, Biba – integralność); **SELinux, AppArmor, Windows MIC**; silny, lecz złożony.
- **RBAC** – uprawnienia przypisane do ról/grup (grupy AD, `/etc/group`, `sudo`); skalowalny i łatwy do audytu; zagrożenie – eksplozja ról.
- Zwykle stosowane **razem** (DAC + MAC + RBAC).

---
[⬅️ Poprzedni temat](4_Zarządzanie_użytkownikami_uprawnieniami_i_kontrolą_dostępu_w_systemach_operacyjnych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_Mechanizmy_uwierzytelniania_hasła_klucze_SSH_2FA_IAM_i_SSO.md)