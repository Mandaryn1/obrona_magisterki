# Szyfrowanie danych w spoczynku i w tranzycie

## Trzy stany danych

Dane w chmurze mogą znajdować się w trzech stanach, a każdy wymaga innej ochrony:

| Stan | Gdzie | Ochrona |
| :--- | :--- | :--- |
| **w spoczynku** (*at rest*) | dyski, bazy danych, obiekty w magazynie, kopie zapasowe | szyfrowanie danych w spoczynku |
| **w tranzycie** (*in transit*) | przesyłane przez sieć między klientem a chmurą i między usługami | szyfrowanie transmisji (TLS) |
| **w użyciu** (*in use*) | przetwarzane w pamięci/procesorze | szyfrowanie danych w użyciu (confidential computing) |

## Szyfrowanie danych w spoczynku (slajd 22)

**Cel:** chronić dane **przechowywane** na dyskach, w bazach danych i w systemach backupu – przed kradzieżą nośnika, snapshotu, kopii zapasowej, nieuprawnionym dostępem do magazynu.

**Zasady (wykład):**

- w chmurze szyfrowanie jest **często zapewniane przez dostawcę**, ale klient może dodać **własne warstwy** szyfrowania,
- **klucze przechowywane oddzielnie od zaszyfrowanych danych** i **rotowane regularnie**,
- **TDE (Transparent Data Encryption)** automatycznie szyfruje dane bez wpływu na aplikacje (temat 13),
- implementacja wymaga **starannego planowania i testów** (wpływ na wydajność); szyfrowanie stosować **selektywnie**.

**Poziomy szyfrowania w spoczynku:**

| Poziom | Przykłady | Co chroni |
| :--- | :--- | :--- |
| **dysk/wolumen** (blokowe) | AWS EBS, Google Persistent Disk, Azure Disk – z **KMS** | utrata nośnika, snapshot |
| **obiekty** (storage) | S3 SSE-S3 / SSE-KMS, Azure Storage Service Encryption | dane w magazynie obiektowym |
| **baza danych** – TDE | Oracle TDE, SQL Server TDE, **Percona pg_tde**, EDB, RDS (KMS) | pliki bazy, logi, kopie |
| **kolumny/pola** | `pgcrypto` – `pgp_sym_encrypt(data, 'key')`, Hibernate `@ColumnTransformer`, JPA Converter | wybrane dane wrażliwe (PII) – chronią też przed użytkownikiem z dostępem do bazy |
| **aplikacja** (client-side) | szyfrowanie przed wysłaniem do chmury | dostawca nie widzi jawnych danych |
| **sekrety Kubernetes** | *Encryption at Rest* + provider KMS; w praktyce **Vault + CSI** | hasła, klucze API |

### Zarządzanie kluczami

- **KMS** (AWS KMS, Azure Key Vault, Google Cloud KMS) lub **Vault Transit** – centralna rotacja, kontrola dostępu i audyt użycia kluczy,
- **HSM** – sprzętowa ochrona kluczy głównych,
- **szyfrowanie kopertowe (envelope encryption)** *(uzupełnienie)*: dane szyfruje szybki klucz **DEK** (Data Encryption Key), a DEK jest szyfrowany kluczem głównym **KEK/CMK** w KMS; ułatwia rotację i ogranicza użycie klucza głównego,
- **zasada rozdzielenia uprawnień:** zespół aplikacyjny nie powinien móc odszyfrowywać kluczy poza ścieżką runtime; audyt użycia kluczy; **rotacja cykliczna i po incydentach**,
- opcje własności klucza: klucze zarządzane przez dostawcę, **CMK** (customer-managed keys), **BYOK** (bring your own key), HYOK/EKM (klucz poza chmurą).

> **Antywzorzec (slajd 312):** w przykładzie z wykładu klucz szyfrujący zapisano jawnie w `application.yaml` (`encryptionSecretKey: ...`). W praktyce **sekretów i kluczy nie trzyma się w plikach konfiguracyjnych ani w repozytorium** – używa się **Vault/KMS/Kubernetes Secrets z szyfrowaniem**.

## Szyfrowanie danych w tranzycie (slajd 23)

**Cel:** chronić dane **przesyłane między systemami** (klient–serwer, usługa–usługa, aplikacja–baza, region–region) przed podsłuchem, modyfikacją i podszywaniem się (MITM).

**Zasady (wykład):**

- w chmurze **wszystkie połączenia powinny być szyfrowane**: komunikacja między usługami, dostęp do API, transfer danych,
- **TLS/SSL** to standard dla HTTPS; **wymuszać najnowsze wersje protokołów TLS**,
- **certyfikaty zarządzane centralnie i regularnie odnawiane**,
- zapewnić, że szyfrowanie **nie jest omijane** przez nieautoryzowane połączenia (wyłączanie HTTP, przekierowanie na HTTPS); **monitorowanie komunikacji** wykrywa próby obejścia.

**Środki praktyczne:**

| Środek | Opis |
| :--- | :--- |
| **HTTPS/TLS** na brzegu | Ingress/Gateway z certyfikatem, **HSTS** (`Strict-Transport-Security`) wymusza HTTPS w przeglądarce |
| **mTLS** (mutual TLS) | **obustronne** uwierzytelnienie certyfikatami między usługami (mesh: Istio, Linkerd) |
| **TLS do bazy** i cache (PostgreSQL, Redis) | włączone certyfikaty serwera, weryfikacja po stronie klienta |
| **VPN** (IPsec, WireGuard) | szyfrowanie ruchu site-to-site / client-to-site do zasobów chmurowych |
| **SSH, SFTP, HTTPS dla transferu plików** | zamiast protokołów jawnych (FTP, Telnet) |
| **Szyfrowanie między regionami** | dostawcy szyfrują ruch między centrami danych |
| **Zarządzanie certyfikatami** | automatyczne odnawianie (cert-manager, Let's Encrypt), krótkie terminy ważności |

*(uzupełnienie)* Zalecenia: **TLS 1.2 i 1.3** (wyłączyć SSLv3, TLS 1.0/1.1), silne zestawy szyfrów (AES-GCM, ChaCha20), **Perfect Forward Secrecy** (ECDHE), certyfikaty z zaufanego CA, kontrola łańcucha.

## Szyfrowanie danych w użyciu (slajd 24)

- Chroni dane **podczas przetwarzania w pamięci**.
- Technologie: **Intel SGX**, **AMD SEV** (confidential computing – enklawy/pamięć maszyn wirtualnych szyfrowana sprzętowo).
- Najtrudniejsze do wdrożenia, ale o najwyższym poziomie ochrony; ważne dla danych finansowych, medycznych i innych wrażliwych.
- *(uwaga)* Wykład wiąże to z **szyfrowaniem homomorficznym** (obliczenia na zaszyfrowanych danych bez deszyfrowania). Ściślej: **SGX/SEV to *confidential computing*** (zaufane środowiska wykonawcze, TEE), a **szyfrowanie homomorficzne** to odrębna, kryptograficzna technika (obliczenia na szyfrogramie – bardzo kosztowna obliczeniowo).

## Porównanie

| Cecha | W spoczynku | W tranzycie |
| :--- | :--- | :--- |
| Chroni przed | kradzieżą nośnika/snapshotu/backupu, nieuprawnionym dostępem do magazynu | podsłuchem i modyfikacją w sieci (MITM) |
| Technologie | AES-256 (dysk, TDE, kolumny), KMS/HSM | **TLS**, mTLS, VPN, SSH |
| Klucze | KMS/HSM, rotacja, oddzielnie od danych | certyfikaty i klucze sesji, odnawianie certyfikatów |
| Typowy błąd | brak szyfrowania backupów, klucz obok danych | HTTP zamiast HTTPS, wyłączona weryfikacja certyfikatu, stare wersje TLS |
| Wpływ na wydajność | niewielki (sprzętowe AES) | niewielki (TLS 1.3) |

**Ważne:** szyfrowanie w spoczynku **nie chroni** przed autoryzowanym użytkownikiem, który odczytuje dane przez aplikację/bazę (dane są deszyfrowane „transparentnie"). Potrzebne są też: kontrola dostępu, audyt, szyfrowanie kolumnowe/aplikacyjne dla najbardziej wrażliwych danych.

## Podsumowanie

- **W spoczynku:** szyfrowanie dysków, baz (TDE), pól, kopii zapasowych; klucze **oddzielnie** od danych, **rotowane**, w KMS/HSM.
- **W tranzycie:** **TLS** (najnowsze wersje), HTTPS wszędzie, mTLS między usługami, VPN; certyfikaty zarządzane centralnie i odnawiane; monitorowanie prób obejścia.
- **W użyciu:** SGX/SEV (confidential computing) – najtrudniejsze, najsilniejsze.
- Szyfrowanie wymaga planowania, testów wydajności i **dobrego zarządzania kluczami**.

---
[⬅️ Poprzedni temat](8_Mechanizmy_RBAC_i_ABAC.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_RPO_Recovery_Point_Objective.md)