# Zastosowanie kryptografii w ochronie danych i komunikacji

> Wykład: MBK2 (slajdy 3–48: podstawy, PKI, ECC, PQC, klucze, szyfrowanie dysków), W5 web (slajdy 12–14: TLS i certyfikaty), MBK1 (TPM). Szczegóły algorytmów – przedmiot *Algorytmy kryptograficzne* (osobny folder); tu skupiam się na **zastosowaniach w OS i usługach**. Uzupełnienia – ***(uzupełnienie)***.

## Cele (wykład MBK2, slajd 3)

Kryptografia zapewnia: **poufność**, **integralność**, **uwierzytelnianie**, **niezaprzeczalność**. W systemach operacyjnych i usługach chroni dane **w spoczynku**, **w tranzycie** i **tożsamość/integralność oprogramowania**.

| Zastosowanie | Mechanizm kryptograficzny | Przykłady w OS/usługach |
| :--- | :--- | :--- |
| dane **w spoczynku** | szyfrowanie symetryczne (AES), klucze w TPM/HSM | BitLocker, LUKS, VeraCrypt, EFS, szyfrowanie baz |
| dane **w tranzycie** | TLS, SSH, IPsec, WPA3 + kryptografia asymetryczna do uzgodnienia klucza | HTTPS, SSH, VPN, e-mail TLS |
| **hasła** | funkcje skrótu z solą/KDF | `/etc/shadow`, bcrypt, Argon2, NTLM |
| **integralność** | skróty (SHA-2/3), MAC, podpisy | sumy plików, FIM, aktualizacje |
| **tożsamość i autentyczność** | podpisy cyfrowe, certyfikaty X.509 | PKI, podpisy kodu, sterowników, Secure Boot |
| **klucze** | KMS, HSM, TPM | zarządzanie sekretami |

## 1. Szyfrowanie symetryczne i asymetryczne (wykład, slajdy 4–8)

- **symetryczne:** jeden klucz, **szybkie** – **AES** (128/192/256; standard NIST 2001; TLS, dyski), **ChaCha20** (lepsze na urządzeniach bez AES sprzętowego); problem – **dystrybucja kluczy**,
- **asymetryczne:** para kluczy; **RSA, ECC, DH/ECDH, ElGamal** – wymiana kluczy i podpisy; wolniejsze,
- **podpisy cyfrowe** (slajd 7): skrót + klucz prywatny; weryfikacja kluczem publicznym; **funkcje skrótu** (slajd 8): SHA-256/3, **MD5 i SHA-1 złamane**,
- **podejście hybrydowe** (TLS, SSH, PGP): asymetria → klucz sesji, symetria → dane.

## 2. Szyfrowanie danych w spoczynku (wykład MBK2, slajdy 38–48)

### Poziomy szyfrowania (slajd 39)

| Poziom | Opis | Przykłady |
| :--- | :--- | :--- |
| **plików/folderów** (*file-level*) | selektywne, elastyczne | EncFS, gocryptfs, **EFS (Windows)** |
| **systemu plików** (*filesystem-level*) | transparentne dla aplikacji, lepsza wydajność | ext4 encryption, APFS, ZFS native |
| **cały dysk** (**FDE**) | szyfruje także system i dane tymczasowe | **BitLocker, LUKS/dm-crypt, VeraCrypt, FileVault** |
| **sprzętowe** (SED, OPAL) | szyfrowanie w kontrolerze dysku | self-encrypting drives (uwaga na błędy implementacji) |

### BitLocker (slajdy 40–41)

Pełne szyfrowanie dysków Windows (edycje Pro/Enterprise), **AES-128/256**; integracja z **TPM**, **klucz odzyskiwania 48-cyfrowy** (zapisywany w AD), **BitLocker To Go** (nośniki wymienne), zarządzanie przez GPO. Tryby ochrony:

| Tryb | Opis |
| :--- | :--- |
| **TPM only** | automatyczne odblokowanie przy zaufanym starcie; chroni przed kradzieżą dysku, **nie przed włączonym urządzeniem** |
| **TPM + PIN** | PIN przed startem – **najwyższe bezpieczeństwo dla laptopów**; ochrona przed DMA i cold boot |
| **Startup key (USB)** | klucz na pendrive'ie (komputery bez TPM) |
| **Password only** | najmniej bezpieczny (bez TPM) |

### VeraCrypt (slajdy 42–44)

Następca TrueCrypt (od 2013), open source, wieloplatformowy; szyfrowanie partycji systemowych i niesystemowych, **kaskadowe** (AES-Twofish-Serpent), **plik-kontener**, **ukryte woluminy i systemy** (*plausible deniability*).

### Linux – LUKS *(uzupełnienie)*

`cryptsetup luksFormat /dev/sdX`, `cryptsetup open`, szyfr `aes-xts-plain64`, wiele slotów kluczy, klucz w TPM (`systemd-cryptenroll`, MBK1 slajd 45).

### Ataki na FDE (slajd 46) i środki zaradcze

| Atak | Opis | Obrona |
| :--- | :--- | :--- |
| **Cold boot** | zamrożenie RAM i odczyt kluczy po wyłączeniu | zerowanie kluczy, hibernacja zamiast uśpienia, **TPM+PIN** |
| **Evil Maid** | modyfikacja bootloadera w czasie nieobecności właściciela | **Secure Boot, atestacja TPM** (PCR się zmienią – MBK1 slajd 47) |
| **DMA** | Thunderbolt/FireWire → bezpośredni dostęp do pamięci | IOMMU, blokada portów, Kernel DMA Protection |
| **malware/keylogger** | przechwycenie hasła/PIN po starcie | EDR, bezpieczny rozruch |

### Wdrożenie FDE w organizacji (slajd 47)

polityka szyfrowania (urządzenia, tryby, zgodność z RODO/ISO 27001, procedury kluczy odzyskiwania) → **pilotaż** → **rollout stopniowy** (GPO/MDM) → **zarządzanie ciągłe** (centralne przechowywanie kluczy odzyskiwania, monitoring statusu, procedury utraty hasła/urządzenia). Wydajność: AES-NI – niewielki narzut.

## 3. Szyfrowanie w tranzycie

### TLS (wykład W5 web, slajdy 12–14)

**Uzgadnianie (handshake):** Client Hello (wersje TLS, zestawy szyfrów, losowa wartość) → Server Hello → **certyfikat serwera** (klucz publiczny, wydany przez zaufane CA) → wymiana kluczy (**ECDHE** zapewnia **Perfect Forward Secrecy**) → klucz sesji → szyfrowanie symetryczne danych. TLS łączy asymetrię, symetrię i skróty; **TLS 1.2+ wymagany m.in. przez PCI DSS**, RODO (art. 32 – szyfrowanie jako środek techniczny); **TLS chroni przed MITM** (weryfikacja certyfikatu), buduje zaufanie (kłódka, HTTPS), przeglądarki oznaczają HTTP jako „Niezabezpieczone".

**Rodzaje certyfikatów (slajd 13):** **DV** (walidacja domeny – automatyczna, Let's Encrypt), **OV** (organizacji), **EV** (rozszerzona), **wildcard**, **wielodomenowe (SAN)**.

### Inne protokoły *(uzupełnienie)*

**SSH** (zdalna powłoka, SFTP, tunelowanie), **IPsec/WireGuard/OpenVPN** (VPN), **WPA2/WPA3** (Wi-Fi), **DNSSEC / DoT / DoH**, **SMTP/IMAP/POP3 z TLS**, **SMB 3 encryption**, **Kerberos** (szyfrowane bilety), **LDAPS**. Zasady: wyłączyć SSLv3 i TLS 1.0/1.1, preferować **TLS 1.3**, silne zestawy szyfrów (AES-GCM, ChaCha20-Poly1305), **HSTS**, certyfikaty zarządzane i odnawiane automatycznie.

## 4. PKI (wykład MBK2, slajdy 9–22)

| Element | Rola |
| :--- | :--- |
| **CA** | zaufany wystawca, podpisuje certyfikaty, zarządza cyklem życia |
| **RA** | weryfikacja tożsamości wnioskodawców |
| **VA** | status certyfikatów (OCSP) |
| **certyfikat X.509** | numer seryjny, wersja, algorytm podpisu, wydawca, podmiot, **klucz publiczny**, okres ważności, rozszerzenia |
| **hierarchia zaufania** | root CA (offline) → pośrednie CA → certyfikaty końcowe |
| **unieważnianie** | **CRL** (lista, co ~24 h; opóźnienia, rozmiar) i **OCSP** (zapytania o pojedynczy certyfikat; **OCSP Stapling**) |
| **cykl życia** | wniosek → wydanie → użycie → odnowienie → unieważnienie/wygaśnięcie |
| **ACME / Let's Encrypt** | automatyczne wydawanie i odnawianie certyfikatów (DV) |
| **Certificate Transparency** | publiczne dzienniki wydanych certyfikatów – wykrywanie nadużyć (i rozpoznanie subdomen – crt.sh) |

Zagrożenia PKI (slajdy 20–21): kompromitacja CA lub klucza prywatnego, fałszywe certyfikaty, słabe algorytmy, błędy zarządzania (wygasłe certyfikaty). Zastosowania w OS: **podpisywanie kodu i sterowników**, **Secure Boot**, 802.1X (EAP-TLS), VPN, e-mail (S/MIME), podpis elektroniczny, uwierzytelnianie urządzeń.

## 5. Zarządzanie kluczami (wykład, slajdy 33–37)

**Cykl:** bezpieczne **generowanie** (dobre źródło losowości), **przechowywanie** (**HSM** – FIPS 140-2/3, **TPM**, **Secure Enclave/TEE**, **chmurowy KMS**), **rotacja**, backup i odzyskiwanie, unieważnianie i niszczenie. Zasady: klucze **oddzielnie od danych**, **najmniejsze uprawnienia** dostępu do kluczy, audyt użycia, **szyfrowanie kopertowe**, nigdy w kodzie ani w repozytorium. TPM: klucze nie opuszczają chipu, PCR, Secure Boot (MBK1 slajdy 41–46).

## 6. ECC i kryptografia postkwantowa (wykład MBK2, slajdy 23–32)

- **ECC** – krótsze klucze przy równej sile (256-bit ECC ≈ RSA-3072), szybkie; krzywe NIST P-256, **Curve25519**, secp256k1; algorytmy ECDH, ECDSA, EdDSA,
- **zagrożenie kwantowe** – algorytm Shora łamie RSA/DH/ECC; Grover osłabia klucze symetryczne (AES-256),
- **PQC (NIST)** – ML-KEM (Kyber), ML-DSA (Dilithium), SLH-DSA (SPHINCS+); migracja, kryptoagilność, rozwiązania hybrydowe, wyzwania wdrożenia (rozmiary kluczy, wydajność, inwentarz kryptografii).

## Zagrożenia i błędy typowe *(uzupełnienie)*

stare algorytmy (DES, RC4, MD5, SHA-1, TLS 1.0), klucze w plikach konfiguracyjnych lub repozytoriach, **brak weryfikacji certyfikatu**, własne algorytmy, powtórne użycie nonce/IV, słaba losowość, brak rotacji, brak kopii klucza odzyskiwania (utrata danych), szyfrowanie bez integralności.

## Podsumowanie

- **W spoczynku:** FDE (**BitLocker** – TPM, TPM+PIN, klucz odzyskiwania; **LUKS**; **VeraCrypt**), szyfrowanie plików (EFS) – ochrona przed kradzieżą nośnika; zagrożenia: cold boot, Evil Maid, DMA.
- **W tranzycie:** **TLS** (handshake, ECDHE, certyfikaty, PFS), SSH, VPN, WPA3; **PKI** (CA, X.509, CRL/OCSP, ACME, CT).
- **Hasła/integralność:** skróty z solą i KDF, podpisy, **Secure Boot i podpisy kodu**; **klucze:** HSM/TPM/KMS, rotacja, rozdzielenie od danych; **PQC** – przygotowanie na komputery kwantowe.
