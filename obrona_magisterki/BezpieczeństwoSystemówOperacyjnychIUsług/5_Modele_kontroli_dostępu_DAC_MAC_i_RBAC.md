# Modele kontroli dostępu: DAC, MAC i RBAC

> Wykład: MBK1 (slajdy 9–13, 23–28: ACL i RBAC; 42–44 – Bell-LaPadula, Biba, Clark-Wilson z poprzedniego przedmiotu), W8 (slajd 13: SELinux/AppArmor). Pozostałe szczegóły – ***(uzupełnienie)***.

## Po co modele kontroli dostępu

**Model kontroli dostępu** określa **zasady, według których podmioty (użytkownicy, procesy) uzyskują dostęp do obiektów (pliki, urządzenia, zasoby)**, oraz *kto decyduje* o przyznawaniu uprawnień. Składniki: **podmiot (subject)**, **obiekt**, **operacja/prawo**, **polityka**, **mechanizm egzekwujący** (monitor odwołań).

## 1. DAC – Discretionary Access Control (kontrola uznaniowa)

**Zasada:** **właściciel zasobu** decyduje, komu i jakie prawa nadaje (według własnego uznania). Podstawą są **listy kontroli dostępu (ACL)** lub macierz dostępu.

- **Windows:** **DACL** w NTFS – właściciel może modyfikować DACL (wykład MBK1, slajd 11),
- **Linux:** prawa **rwx** właściciel–grupa–inni (`chmod`, `chown`), **POSIX ACL** (`setfacl`).

| Zalety | Wady |
| :--- | :--- |
| elastyczny, prosty, naturalny dla użytkowników | polityka zależy od **woli i wiedzy użytkownika** (błędy, nadmierne uprawnienia) |
| precyzyjny dla pojedynczych zasobów | **brak kontroli przepływu informacji** – użytkownik z dostępem może skopiować dane i przekazać dalej; trojan uruchomiony z prawami użytkownika dziedziczy jego uprawnienia (**problem „zdezorientowanego pełnomocnika"**) |
| szeroko stosowany | trudny do audytu przy wielu zasobach |

## 2. MAC – Mandatory Access Control (kontrola obowiązkowa)

**Zasada:** **polityka centralna (administrator/system)** narzuca dostęp na podstawie **etykiet bezpieczeństwa** (poziom jawności podmiotu i klasyfikacja obiektu, **kategorie**, typy); **użytkownik nie może jej zmienić**, nawet jako właściciel zasobu. Odmówić może także **proces** działający z uprawnieniami użytkownika – ogranicza skutki włamania.

### Klasyczne modele formalne (wykład, poprzedni przedmiot)

| Model | Cel | Zasady |
| :--- | :--- | :--- |
| **Bell-LaPadula** | **poufność** | **no read up** (nie czytaj wyżej), **no write down** (nie pisz niżej) – systemy rządowe/wojskowe; nie adresuje integralności ani dostępności |
| **Biba** | **integralność** | **no read down**, **no write up** – zapobieganie „skażeniu" danych wysokiej jakości; finanse, medycyna |
| **Clark-Wilson** | integralność w systemach komercyjnych | dane CDI (kontrolowane) zmieniane tylko przez autoryzowane procedury TP, weryfikowane przez IVP; separacja obowiązków |

### Implementacje w systemach operacyjnych

| Mechanizm | System | Opis |
| :--- | :--- | :--- |
| **SELinux** (Security-Enhanced Linux) | Linux – RHEL/CentOS/Fedora | MAC oparty na **etykietach (context: użytkownik:rola:typ:poziom)** i regułach **type enforcement**; tryby `enforcing`, `permissive`, `disabled`; `getenforce`, `setenforce`, `ls -Z`, `semanage`, `restorecon`; każda usługa ma własną domenę (np. `httpd_t`) – nawet root nie obejdzie reguł; wykład W8: *SELinux w systemach Red Hat/CentOS ogranicza procesy do zdefiniowanych polityk* |
| **AppArmor** | Linux – Ubuntu/Debian/SUSE | MAC oparty na **profilach ścieżek** per program; prostszy w konfiguracji; tryby *enforce* i *complain*; `aa-status`, `aa-genprof` |
| **Windows MIC** (Mandatory Integrity Control) | Windows | **poziomy integralności** (Low, Medium, High, System) – proces o niższym poziomie nie może zapisywać do obiektów wyższego (podobne do Biby); podstawa **UAC**, trybu chronionego przeglądarki i **AppContainer** |
| **AppLocker / WDAC** | Windows | białe listy aplikacji (wykład W8, slajd 9) – kontrola, co może się uruchomić |
| **TrustedBSD/MAC Framework, Smack, TOMOYO, Trusted Solaris** | inne | |

| Zalety MAC | Wady |
| :--- | :--- |
| silna ochrona poufności/integralności, ograniczenie skutków kompromitacji, centralna polityka | **złożona konfiguracja i utrzymanie**, ryzyko blokady legalnych funkcji, wymaga wiedzy |

## 3. RBAC – Role-Based Access Control (kontrola oparta na rolach)

**Zasada (wykład MBK1, slajdy 23–28):** uprawnienia przypisuje się **rolom** (odpowiadającym stanowiskom/funkcjom), a role – użytkownikom. Użytkownik **dziedziczy** uprawnienia roli.

```
 użytkownik ─(członek)─▶ rola/grupa ─(ma uprawnienia)─▶ zasób / operacja
```

### Windows (slajd 25)
RBAC realizują **grupy bezpieczeństwa w Active Directory**: 1) utworzenie grupy odpowiadającej roli (np. „Dział_HR", „Administratorzy_Aplikacji", „Użytkownicy_VPN"), 2) przypisanie uprawnień grupie (przez ACL lub GPO), 3) dodanie użytkowników. **Grupy zagnieżdżone** tworzą hierarchię ról (Menedżerowie → Menedżerowie_IT, Menedżerowie_Sprzedaży).

### Linux (slajd 26)
Grupy użytkowników: `groupadd programisci`, `usermod -aG programisci jan`, `groups jan`, `chown :programisci /opt/projekt`, `chmod 770 /opt/projekt`; plik `/etc/group`; **`sudo` i `/etc/sudoers`** definiują role z prawem wykonywania poleceń jako root (precyzyjne uprawnienia administracyjne).

### Korzyści RBAC (slajd 28)

| | |
| :--- | :--- |
| **scentralizowane zarządzanie** | zmiana roli wpływa na wszystkich jej członków |
| **łatwość audytu** | wystarczy przejrzeć role i przypisania |
| **mniej błędów** | spójne uprawnienia w roli – mniej przypadkowych nadmiernych uprawnień |
| **skalowalność** | nowy użytkownik = przypisanie ról (np. 10 000 pracowników) |

***(uzupełnienie)*** Model **NIST RBAC**: RBAC0 (podstawowy), RBAC1 (hierarchie ról), RBAC2 (ograniczenia – separacja obowiązków: statyczna i dynamiczna), RBAC3. Wady: **„eksplozja ról"**, brak uwzględnienia kontekstu; rozszerzenie: **ABAC** (atrybuty użytkownika, zasobu, środowiska – np. polityki w chmurze).

## Porównanie DAC, MAC, RBAC

| Cecha | **DAC** | **MAC** | **RBAC** |
| :--- | :--- | :--- | :--- |
| Kto decyduje | **właściciel zasobu** | **administrator/polityka systemowa** | administrator przypisuje role |
| Podstawa decyzji | tożsamość i ACL | **etykiety** (poziomy, typy) | **rola** użytkownika |
| Elastyczność | wysoka | niska (sztywna) | średnia–wysoka |
| Poziom bezpieczeństwa | niski–średni | **wysoki** | średni–wysoki |
| Złożoność | niska | wysoka | średnia |
| Kontrola przepływu informacji | nie | **tak** | pośrednio |
| Przykłady | prawa Unix, NTFS DACL | **SELinux, AppArmor, MIC**, systemy wojskowe | grupy AD, sudoers, role w bazach i chmurze |
| Zastosowanie | zwykłe systemy, pliki użytkowników | rząd, wojsko, krytyczne usługi, kontenery | organizacje z wieloma użytkownikami |

## Współdziałanie modeli w praktyce

Systemy łączą modele: **DAC** (podstawowy) + **MAC** (dodatkowy filtr – SELinux/AppArmor) + **RBAC** (zarządzanie uprawnieniami przez grupy/role). Dostęp jest przyznany **tylko, gdy wszystkie warstwy go dopuszczą** (np. proces httpd ma `rwx` w DAC, ale polityka SELinux zabrania mu odczytu `/home`). To realizuje **obronę w głąb** i zasadę najmniejszych uprawnień.

### Przykłady poleceń *(uzupełnienie)*

```bash
chmod 750 /srv/app && chown app:devs /srv/app     # DAC
setfacl -m u:anna:r-x /srv/app                    # DAC rozszerzone (ACL)
getenforce; ls -Z /var/www; semanage fcontext -l  # MAC – SELinux
aa-status                                         # MAC – AppArmor
usermod -aG devs jan; visudo                      # RBAC – grupy i sudo
```

```powershell
icacls D:\Dane /grant "Dział_HR:(OI)(CI)R"        # DAC/ACL + RBAC przez grupę AD
Add-ADGroupMember -Identity "Dział_HR" -Members jan
```

## Podsumowanie

- **DAC** – właściciel decyduje (ACL, prawa rwx), elastyczny, ale podatny na błędy i trojany.
- **MAC** – polityka centralna i etykiety (Bell-LaPadula – poufność, Biba – integralność); **SELinux, AppArmor, Windows MIC**; silny, lecz złożony.
- **RBAC** – uprawnienia przypisane do ról/grup (grupy AD, `/etc/group`, `sudo`); skalowalny i łatwy do audytu; zagrożenie – eksplozja ról.
- Zwykle stosowane **razem** (DAC + MAC + RBAC).

---
[⬅️ Poprzedni temat](4_Zarządzanie_użytkownikami_uprawnieniami_i_kontrolą_dostępu_w_systemach_operacyjnych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_Mechanizmy_uwierzytelniania_hasła_klucze_SSH_2FA_IAM_i_SSO.md)