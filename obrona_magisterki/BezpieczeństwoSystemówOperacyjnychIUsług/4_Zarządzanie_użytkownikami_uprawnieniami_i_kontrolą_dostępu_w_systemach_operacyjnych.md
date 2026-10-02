# Zarządzanie użytkownikami, uprawnieniami i kontrolą dostępu w systemach operacyjnych

> Wykład: W1 (slajdy 45–48, 54–55, 80–82), W2 (slajdy 36–37), MBK1 (slajdy 5–13, 23–28), W8 (slajdy 6, 19). Szczegóły techniczne (UID, `/etc/shadow`, SUID, UAC, SID) – ***(uzupełnienie)***.

## Podstawy: identyfikacja, uwierzytelnianie, autoryzacja

Wykład (MBK1, slajdy 7–9): **uwierzytelnianie** = „kim jesteś?", **autoryzacja** = „co możesz zrobić?" – system porównuje żądanie z politykami i przyznaje lub odmawia. Przykłady autoryzacji: dostęp do pliku chronionego ACL, uruchomienie aplikacji wymagającej uprawnień administratora, zmiana ustawień systemowych.

## Windows

### Konta użytkowników (wykład W1, slajd 48)

- po instalacji tworzone jest **konto lokalne użytkownika**; w sieci korporacyjnej – **konta domenowe** w **Active Directory** (kontroler domeny, polityki, centralne zarządzanie),
- **najlepsza praktyka:** nie włączać konta wbudowanego **Administratora** ani nie logować się na konto z uprawnieniami administracyjnymi do codziennej pracy (slajd 45) – *każdy program uruchomiony na takim koncie dziedziczy uprawnienia administratora*, a **malware z takimi uprawnieniami ma pełny dostęp do plików i folderów**,
- gdy potrzebne są uprawnienia: **„Uruchom jako administrator"** (kliknięcie prawym przyciskiem lub wiersz polecenia administratora – slajdy 46–47).

***(uzupełnienie)*** Elementy modelu: **SID** (identyfikator bezpieczeństwa konta/grupy), **access token** procesu (SID użytkownika, grupy, uprawnienia/privileges), **UAC** (User Account Control – nawet administrator pracuje z tokenem standardowego użytkownika do czasu podniesienia uprawnień), **konta usługowe** (LocalSystem, LocalService, NetworkService, **gMSA**), grupy wbudowane (Administrators, Users, Remote Desktop Users, Backup Operators), **uprawnienia systemowe (privileges)** (np. `SeDebugPrivilege`).

### ACL w NTFS (wykład MBK1, slajd 11)

Każdy obiekt (plik, katalog, klucz rejestru, usługa, drukarka) ma **deskryptor bezpieczeństwa** z dwoma listami:

| Lista | Rola |
| :--- | :--- |
| **DACL** (Discretionary ACL) | określa, **którzy użytkownicy i grupy mają dostęp** i jakie operacje mogą wykonywać; właściciel może ją modyfikować według uznania |
| **SACL** (System ACL) | **audyt** – które operacje dostępu mają być rejestrowane w dzienniku zdarzeń; zarządzana przez administratorów |

Zarządzanie: GUI (Właściwości → Zabezpieczenia), `icacls`, PowerShell (`Get-Acl`/`Set-Acl`). Elementy listy: **ACE** (Access Control Entry): SID, typ (zezwól/odmów), uprawnienia (odczyt, zapis, wykonanie, usuwanie…). **Dziedziczenie** z katalogów nadrzędnych; **jawna odmowa ma pierwszeństwo**.

### Polityki zabezpieczeń i narzędzia administracyjne (wykład W1)

- **Zasady zabezpieczeń** – zestaw celów zapewniających bezpieczeństwo sieci, danych i systemów; dokument ewoluujący (slajd 80),
- **domena AD** – polityki domenowe stosowane do wszystkich komputerów; **Lokalne zasady zabezpieczeń** dla komputerów autonomicznych (slajdy 81–82): **zasady haseł**, **blokowanie konta (account lockout)** przeciw brute force, **blokada komputera po wygaszaczu ekranu**, prawa użytkowników, reguły zapory, **AppLocker** (ograniczenie plików, które mogą uruchamiać użytkownicy i grupy); funkcja *Eksportuj politykę* do powielenia konfiguracji,
- polecenia **`net`** (slajdy 54–55): `net accounts` (wymagania hasła i logowania), `net user`, `net localgroup`, `net session`, `net share`, `net start/stop`, `net use`,
- **GPO** – centralne wymuszanie konfiguracji; **WMI**, **PowerShell** do zarządzania zdalnego.

## Linux

### Użytkownicy i grupy *(uzupełnienie)*

| Plik | Zawartość |
| :--- | :--- |
| `/etc/passwd` | konta: nazwa, UID, GID, katalog domowy, powłoka (czytelny dla wszystkich) |
| `/etc/shadow` | **skróty haseł** (np. `$y$` yescrypt, `$6$` SHA-512-crypt) i terminy ważności; czytelny tylko dla root |
| `/etc/group` | grupy i ich członkowie (wykład MBK1, slajd 26) |
| `/etc/sudoers` | kto może wykonywać jakie polecenia jako inny użytkownik (edycja `visudo`) |

**UID 0 = root** (superużytkownik, absolutna władza – wykład W2, slajd 5). Konta usługowe z `nologin`. Polecenia: `useradd`/`adduser`, `usermod -aG`, `passwd`, `chage`, `groupadd`, `su`, **`sudo`** (wykład W2 slajd 11 – wykonanie polecenia jako superużytkownik), `id`, `groups`.

**Dobre praktyki (wykład W2, slajd 28):** **wyłączyć logowanie roota przez SSH**, używać `sudo` dla wybranych użytkowników (wykład W8, slajd 13: *„bezpośrednie logowanie jako root powinno być całkowicie wyłączone w systemie produkcyjnym"*).

### Prawa dostępu do plików (wykład W2, slajdy 36–37)

`ls -l` → `-rwxrw-r--  1 analyst staff 253 …`: pierwszy znak – typ (`-` plik, `d` katalog), następnie **trzy trójki: właściciel (u), grupa (g), inni (o)**; bity **r (odczyt) = 4, w (zapis) = 2, x (wykonanie) = 1**.

| Wartość ósemkowa | Symbolicznie | Znaczenie |
| :-: | :-: | :--- |
| 0 | `---` | brak dostępu |
| 1 | `--x` | tylko wykonanie |
| 2 | `-w-` | tylko zapis |
| 3 | `-wx` | zapis i wykonanie |
| 4 | `r--` | tylko odczyt |
| 5 | `r-x` | odczyt i wykonanie |
| 6 | `rw-` | odczyt i zapis |
| 7 | `rwx` | odczyt, zapis i wykonanie |

Przykład z wykładu: `-rwxrw-r--` ≡ **764** (właściciel `rwx`, grupa `rw-`, inni `r--`). Polecenia: `chmod` (zmiana uprawnień), `chown`/`chgrp`, `umask`.

> ***Korekta wykładu:*** slajd 37 mówi, że uprawnienia zmienia tylko root – w rzeczywistości **zmienić uprawnienia pliku może jego właściciel oraz root**.

Dodatkowe bity *(uzupełnienie)*: **SUID** (`4xxx` – proces działa z uprawnieniami **właściciela** pliku, np. `passwd`), **SGID** (`2xxx`), **sticky bit** (`1xxx` – w katalogu, np. `/tmp`: usuwać można tylko własne pliki). **Pliki SUID roota to cel eskalacji uprawnień** – należy je inwentaryzować (`find / -perm -4000`).

**Linki** (slajdy 38–39): **twarde** (inna nazwa tego samego pliku, licznik dowiązań) i **symboliczne** (wskazują ścieżkę; kruche, ale mogą wskazywać inne systemy plików) – *ataki przez dowiązania symboliczne (symlink race)*.

### Rozszerzone ACL w Linuksie (MBK1, slajd 12)

Model *właściciel–grupa–inni* uzupełniają **POSIX ACL**: `setfacl -m u:jan:rwx plik.txt`, `setfacl -m g:programisci:rx plik.txt`, `getfacl plik.txt`; wymagają wsparcia systemu plików (ext4, XFS, Btrfs).

## Porównanie ACL (wykład MBK1, slajd 13)

| | Windows | Linux |
| :--- | :--- | :--- |
| Zarządzanie | GUI (Eksplorator, MMC) + PowerShell, `icacls` | CLI (`setfacl`, `getfacl`, `chmod`) |
| Integracja | głęboka z NTFS, AD, GPO; ACL także dla rejestru, usług, drukarek | rozszerzenie modelu Unix; mniejsza integracja, większa elastyczność |
| Zalety | łatwość użycia, szczegółowa kontrola | prostota |
| Ograniczenia | złożoność w dużych środowiskach, konflikty reguł | prostota modelu bazowego, ograniczona granularność bez ACL |

## Zasady zarządzania uprawnieniami (wykład W8 + uzupełnienie)

1. **Najmniejsze uprawnienia**, konta standardowe do pracy; osobne konta administracyjne.
2. **Przypisywanie uprawnień grupom/rolom**, nie pojedynczym osobom (RBAC – temat 5).
3. **Separacja obowiązków**, **regularne przeglądy** i recertyfikacja uprawnień, **audyt**.
4. **Cykl życia konta:** tworzenie, zmiany, **natychmiastowe odebranie dostępu po odejściu** (*joiner–mover–leaver*), usuwanie kont nieaktywnych.
5. **Silne hasła i MFA** (temat 6), blokada konta po nieudanych próbach.
6. **Konta usługowe** z minimalnymi uprawnieniami, bez logowania interaktywnego, rotacja sekretów.
7. **Kontrola dostępu do danych wrażliwych** (szyfrowanie, DLP) i **dostępu fizycznego**.
8. **Logowanie i monitoring** użycia uprawnień (zdarzenia 4624/4625/4670/4663 w Windows, `auth.log`/auditd w Linuksie – wykład MBK1, slajd 49).

## Podsumowanie

- Windows: **konta lokalne/domenowe, SID i token, UAC, DACL i SACL w NTFS, GPO i lokalne zasady zabezpieczeń (hasła, lockout), `net`, AppLocker**; unikać pracy na koncie administratora.
- Linux: **UID/GID, `/etc/passwd`, `/etc/shadow`, `/etc/group`, `sudo`**, prawa **rwx** (4-2-1; np. 764), SUID/SGID/sticky, **POSIX ACL** (`setfacl`/`getfacl`); root wyłączony dla SSH.
- Zasady: **najmniejsze uprawnienia, role/grupy, przeglądy i audyt, cykl życia kont**, MFA, kontrola kont usługowych.

---
[⬅️ Poprzedni temat](3_Jądro_systemu_separacja_przestrzeni_użytkownika_i_ochrona_pamięci.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Modele_kontroli_dostępu_DAC_MAC_i_RBAC.md)