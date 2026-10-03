# Zarządzanie użytkownikami, uprawnieniami i kontrolą dostępu w systemach operacyjnych

## 1. Identyfikacja i Konta Użytkowników

* **Konta i Identyfikatory (UID / SID):** Każdy użytkownik i grupa w systemie operacyjnym posiada unikalny identyfikator numeryczny (UID w Linux, SID w Windows), na podstawie którego system weryfikuje tożsamość i przyznaje uprawnienia.
* **Konta standardowe vs uprzywilejowane:**
  * **Użytkownik standardowy:** Działa w trybie ograniczonym, nie mając praw do modyfikacji plików systemowych i konfiguracji.
  * **Superużytkownik (root / Administrator):** Posiada pełne, nieograniczone uprawnienia do zarządzania całym systemem operacyjnym.
* **Zasada minimalnych uprawnień (Least Privilege):** Przyznawanie użytkownikom i procesom wyłącznie takich uprawnień do plików i usług, jakie są niezbędne do wykonywania ich zadań.

---

## 2. Mechanizmy Podnoszenia Uprawnień

* **System Linux (`sudo` / `su`):**
  * Polecenie `sudo` pozwala zweryfikowanemu użytkownikowi uruchomić pojedyncze polecenie z prawami superużytkownika `root`.
  * Zapobiega stałej pracy na koncie `root` i umożliwia precyzyjne audytowanie wykonanych poleceń.
* **System Windows (UAC / Run as Administrator):**
  * **UAC (User Account Control):** Uruchamia aplikacje z prawami użytkownika standardowego.
  * Wykonanie zadania administracyjnego wymaga jawnego potwierdzenia lub podania poświadczeń administratora w oknie UAC.
  * **Praca na koncie administratora:** Uruchomienie złośliwego oprogramowania na koncie z uprawnieniami administracyjnymi sprawia, że kod dziedziczy pełne uprawnienia i uzyskuje dostęp do całego systemu.

---

## 3. Zarządzanie Kontrolą Dostępu do Plików i Zasobów

* **Model uprawnień Linux (UGO & Notation):**
  * **Podział podmiotów:** Właściciel (*User*), Grupa (*Group*), Inni (*Others*).
  * **Prawa podstawowe:** Odczyt (`r`=4), Zapis (`w`=2), Wykonanie (`x`=1) reprezentowane wartościami ósemkowymi (np. `755` lub `644`).
  * **Narzędzia:** Zmiana uprawnień poleceniem `chmod`, zmiana właściciela poleceniem `chown`.
* **Model uprawnień Windows (ACL / NTFS):**
  * Wykorzystuje **Listy Kontroli Dostępu (ACL)** przypisane do obiektów w systemie plików NTFS.
  * Pozwala na precyzyjne definiowanie praw (np. Pełna kontrola, Modyfikacja, Odczyt i wykonanie) dla poszczególnych użytkowników lub grup.

---

## 4. Centralne Zarządzanie i Wyliczanie Kont

* **Zarządzanie zasadami (Active Directory / Secpol):** Centralne wymuszanie polityk haseł i praw użytkowników w domenach (Active Directory) lub lokalnie na autonomicznych komputerach za pomocą Lokalnych Zasad Zabezpieczeń (`secpol.msc`).
* **Enumeracja użytkowników i grup:** Testerzy penetracyjni oraz atakujący wykorzystują skanowanie usług (np. SMB port 445 za pomocą Nmap NSE) do wyliczenia istniejących kont i grup w celu przygotowania ataków słownikowych lub *Brute Force*.

---

## 5. Podsumowanie do wypowiedzi na obronie

> *"Zarządzanie użytkownikami i kontrolą dostępu opiera się na podziale na konta standardowe i uprzywilejowane oraz konsekwentnym stosowaniu zasady minimalnych uprawnień. Kontrola dostępu do zasobów realizowana jest przez uprawnienia plików – model UGO w Linux oraz listy ACL w NTFS. Bezpieczne podnoszenie uprawnień odbywa się za pomocą mechanizmów `sudo` lub UAC, co zapobiega dziedziczeniu pełnych praw administracyjnych przez potencjalnie złośliwe oprogramowanie."*

## Podsumowanie

- Windows: **konta lokalne/domenowe, SID i token, UAC, DACL i SACL w NTFS, GPO i lokalne zasady zabezpieczeń (hasła, lockout), `net`, AppLocker**; unikać pracy na koncie administratora.
- Linux: **UID/GID, `/etc/passwd`, `/etc/shadow`, `/etc/group`, `sudo`**, prawa **rwx** (4-2-1; np. 764), SUID/SGID/sticky, **POSIX ACL** (`setfacl`/`getfacl`); root wyłączony dla SSH.
- Zasady: **najmniejsze uprawnienia, role/grupy, przeglądy i audyt, cykl życia kont**, MFA, kontrola kont usługowych.

---
[⬅️ Poprzedni temat](3_Jądro_systemu_separacja_przestrzeni_użytkownika_i_ochrona_pamięci.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Modele_kontroli_dostępu_DAC_MAC_i_RBAC.md)