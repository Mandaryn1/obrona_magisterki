# Algorytmy kryptograficzne – wprowadzenie i ściągawka

> **Źródło:** opracowanie z własnej wiedzy (brak materiałów ze studiów). Przykłady liczbowe zostały sprawdzone obliczeniowo. Jeśli prowadzący używał innej notacji lub kolejności, dopasuj do niej odpowiedzi.

## Mapa przedmiotu

| Obszar | Zagadnienia z listy |
| :--- | :--- |
| Podstawy, cele, klasyfikacja, zasada Kerckhoffsa | 1, 2, 3 |
| Tajność doskonała, szyfry klasyczne vs nowoczesne | 4, 5 |
| Szyfry symetryczne i tryby pracy | 6, 7 |
| Kryptografia klucza publicznego (DH, RSA, ElGamal, ECC) | 7, 8, 9, 10, 14 |
| Uwierzytelnianie i integralność (podpis, hash, MAC) | 11, 12, 13 |
| Kryptografia kwantowa i postkwantowa | 15 |

## Podstawowe pojęcia

| Pojęcie | Znaczenie |
| :--- | :--- |
| **tekst jawny** (plaintext, $m$) | wiadomość przed zaszyfrowaniem |
| **szyfrogram** (ciphertext, $c$) | wiadomość zaszyfrowana |
| **klucz** ($k$) | tajna (lub publiczna) wartość sterująca algorytmem |
| **szyfrowanie / deszyfrowanie** | $c=E_k(m)$, $m=D_k(c)$ |
| **algorytm szyfrujący (szyfr)** | para algorytmów $(E,D)$ |
| **kryptosystem** | szyfr + przestrzenie $M$ (tekstów), $C$ (szyfrogramów), $K$ (kluczy) |
| **funkcja skrótu** | $h(m)$ – stała długość, bez klucza |
| **MAC** | skrót z kluczem tajnym |
| **podpis cyfrowy** | skrót + klucz prywatny nadawcy |
| **KDF** | funkcja wyprowadzania klucza (np. PBKDF2, Argon2, HKDF) |
| **nonce / IV** | jednorazowa lub losowa wartość, wymagana w trybach szyfrów |
| **PKI** | infrastruktura klucza publicznego (certyfikaty X.509, urzędy certyfikacji) |
| **PRNG / CSPRNG** | (kryptograficznie bezpieczny) generator liczb pseudolosowych |

## Bezpieczeństwo: dwa podejścia

- **teoretycznoinformacyjne** (bezwarunkowe) – odporność niezależna od mocy obliczeniowej atakującego (np. szyfr z kluczem jednorazowym),
- **obliczeniowe** – odporność przy założeniu, że atakujący ma ograniczone zasoby i że pewne problemy są trudne (faktoryzacja, logarytm dyskretny); tak działa niemal cała współczesna kryptografia.

**Poziom bezpieczeństwa** podaje się w bitach: ok. $2^{128}$ operacji dla ataku = „128 bitów bezpieczeństwa".

## Zalecenia (orientacyjnie, NIST SP 800-57 / aktualne praktyki)

| Cel | Aktualnie zalecane | Przestarzałe / niezalecane |
| :--- | :--- | :--- |
| szyfr symetryczny | **AES-128/256**, ChaCha20 | DES, 3DES (wycofany), RC4 |
| tryb | **GCM**, CCM, ChaCha20-Poly1305 (AEAD); CTR + MAC | **ECB**, CBC bez MAC |
| skrót | **SHA-256/384/512, SHA-3**, BLAKE2/3 | **MD5, SHA-1** |
| MAC | **HMAC-SHA-256**, CMAC, Poly1305 | MAC na MD5/SHA-1 |
| asymetryczne | **RSA ≥ 2048 (lepiej 3072)**, **ECC (P-256, Curve25519)**, DH ≥ 2048 | RSA < 2048, DH 1024 |
| podpis | **ECDSA, EdDSA (Ed25519), RSA-PSS** | RSA PKCS#1 v1.5 (tylko zgodność wsteczna) |
| hasła | **Argon2id, bcrypt, scrypt, PBKDF2** | zwykły SHA-256/MD5 |
| postkwantowe | **ML-KEM, ML-DSA, SLH-DSA** (standardy NIST) | – |

**Złota zasada:** nie wymyślaj własnych algorytmów ani protokołów – używaj sprawdzonych bibliotek (libsodium, OpenSSL, Bouncy Castle) i standardowych konstrukcji.
