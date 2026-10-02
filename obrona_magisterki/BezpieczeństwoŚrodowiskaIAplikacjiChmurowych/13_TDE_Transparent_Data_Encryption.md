# TDE (Transparent Data Encryption)

## Definicja

**TDE (Transparent Data Encryption – przezroczyste szyfrowanie danych)** to mechanizm szyfrowania danych **w spoczynku na poziomie bazy danych** (plików danych, logów transakcji, często kopii zapasowych), który **działa automatycznie i „przezroczyście" dla aplikacji**: dane są szyfrowane przy zapisie na dysk i deszyfrowane przy odczycie do pamięci przez silnik bazy danych.

Wykład (slajd 22): *TDE automatycznie szyfruje dane **bez wpływu na aplikacje**.*

## Zasada działania

```
 aplikacja ──(dane jawne)──▶ silnik bazy danych ──(szyfrowanie)──▶ pliki na dysku
                                  ▲                                  (zaszyfrowane)
                                  │ odszyfrowanie przy odczycie
                          klucz szyfrujący DEK
                                  ▲
                        klucz główny (KMS / HSM / portfel)
```

- Silnik bazy szyfruje **strony danych** (zwykle **AES-128/256**) kluczem **DEK** (*Database Encryption Key*).
- DEK jest chroniony **kluczem głównym** (master key / KEK) przechowywanym **poza bazą**: w portfelu kluczy, **HSM** lub usłudze **KMS** (np. AWS KMS, Azure Key Vault) – **szyfrowanie kopertowe**.
- Aplikacja i zapytania SQL **nie wymagają zmian** – stąd „transparentne".
- Dane w pamięci (bufory) są jawne; szyfrowane są **dane na nośniku**.

## Co chroni, a czego nie

| TDE chroni przed | TDE **nie** chroni przed |
| :--- | :--- |
| kradzieżą dysku, kopii zapasowej lub **snapshotu** | **uprawnionym użytkownikiem**/aplikacją, która odczytuje dane (dane są odszyfrowywane) |
| odczytem plików bazy z poziomu systemu operacyjnego/magazynu przez osobę bez klucza | **SQL injection**, przejęciem konta bazy danych lub aplikacji |
| nieautoryzowanym dostępem do nośnika w chmurze (dostawca, błąd konfiguracji magazynu) | administratorem bazy (DBA) z pełnymi uprawnieniami |
| wymogami zgodności (PCI DSS, RODO, HIPAA – szyfrowanie w spoczynku) | wyciekiem danych w tranzycie (potrzebny TLS) |

Dlatego TDE uzupełnia się o: **kontrolę dostępu (RBAC, RLS)**, **szyfrowanie kolumnowe/aplikacyjne** dla najbardziej wrażliwych pól, **audyt** i **TLS**.

## TDE a inne poziomy szyfrowania w spoczynku

| Poziom | Przykład | Zakres | Przezroczystość |
| :--- | :--- | :--- | :--- |
| dysk/wolumen | EBS, Azure Disk + KMS | cały wolumen | pełna (system plików) |
| **TDE** | Oracle, SQL Server, Azure SQL, RDS | **cała baza/pliki** | pełna dla aplikacji |
| **kolumnowe** | `pgcrypto` (`pgp_sym_encrypt`), `@ColumnTransformer` w Hibernate | wybrane kolumny | wymaga zmian w aplikacji/SQL |
| aplikacyjne/client-side | szyfrowanie w kodzie przed zapisem | wybrane dane | wymaga zmian; dostawca nie widzi jawnych danych |

W wykładzie (slajdy 221, 310): **TDE do całych tabel/plików bazy** (np. **EDB Postgres Advanced Server**, **Percona pg_tde** dla PostgreSQL; w chmurze RDS z KMS), **pgcrypto i Hibernate** do pól (kolumn), **AWS KMS** do zarządzania kluczami. Dla PII: „minimalizacja i szyfrowanie w spoczynku (DB TDE/kolumnowe)" (slajd 189).

## Implementacje

| Platforma | Uwagi |
| :--- | :--- |
| **Oracle Database** | Oracle TDE: szyfrowanie kolumn lub całych tablespace'ów; klucz główny w portfelu (Wallet) lub OKV/HSM |
| **Microsoft SQL Server / Azure SQL** | TDE na poziomie bazy (certyfikat w `master` → DEK); w Azure SQL domyślnie włączone, klucz zarządzany przez usługę lub **BYOK** (Key Vault) |
| **AWS RDS / Aurora** | szyfrowanie w spoczynku przez KMS (włączane przy tworzeniu instancji; obejmuje dane, logi, backupy, repliki) |
| **PostgreSQL** | wersja społecznościowa **nie ma wbudowanego TDE**; rozwiązania: **Percona pg_tde**, EDB, szyfrowanie dysku, pgcrypto |
| **MySQL / MariaDB** | szyfrowanie tablespace (InnoDB), keyring |

## Zarządzanie kluczami w TDE

- klucz główny **poza bazą**, w **KMS/HSM**; **oddzielnie od danych i backupów**,
- **rotacja** (zmiana klucza głównego bez ponownego szyfrowania wszystkich danych – dzięki szyfrowaniu kopertowemu),
- kontrola dostępu do klucza i **audyt jego użycia**,
- **kopia klucza i procedura odzyskiwania** – utrata klucza = **trwała utrata danych**,
- przy backupach i przenoszeniu bazy trzeba mieć dostęp do klucza (certyfikatu) docelowo.

## Wydajność i wdrożenie

- Narzut jest **niewielki** (sprzętowe AES-NI, szyfrowanie na poziomie stron I/O), zwykle rzędu kilku procent obciążenia CPU; wykład: wdrożenie wymaga **starannego planowania i testów**, aby szyfrowanie nie wpływało negatywnie na wydajność.
- Początkowe szyfrowanie istniejącej bazy może być czasochłonne.
- Dane zaszyfrowane **słabo się kompresują** (kompresję należy wykonać przed szyfrowaniem) i nie nadają się do indeksowania/porównywania bez odszyfrowania (przy szyfrowaniu kolumnowym).
- Zaszyfrowane są także pliki tymczasowe, dzienniki, **backupy** (zależnie od silnika).

## Zalety i wady

| Zalety | Wady |
| :--- | :--- |
| **brak zmian w aplikacji** | **nie chroni** przed dostępem przez aplikację/DBA/SQLi |
| szybkie wdrożenie, szyfrowanie całej bazy, w tym kopii | zależność od zarządzania kluczami; utrata klucza = utrata danych |
| spełnia wymagania regulacyjne (szyfrowanie w spoczynku) | niewielki narzut wydajności; początkowe szyfrowanie |
| w chmurze często włączane jednym ustawieniem | w niektórych silnikach (PostgreSQL community) nie jest dostępne natywnie |

## Podsumowanie

- **TDE** = automatyczne, przezroczyste dla aplikacji szyfrowanie plików bazy danych (dane w spoczynku), z kluczem chronionym w KMS/HSM.
- Chroni przed kradzieżą nośnika/kopii/snapshotu, **nie** przed uprawnionym dostępem i atakami na poziomie aplikacji.
- Uzupełnia się o RBAC, szyfrowanie kolumnowe, TLS i audyt; wymaga dobrego zarządzania kluczami.
