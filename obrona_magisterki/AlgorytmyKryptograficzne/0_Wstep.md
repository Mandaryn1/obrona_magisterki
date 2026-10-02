# Algorytmy kryptograficzne – wprowadzenie i ściągawka

> **Źródło:** opracowanie z własnej wiedzy (brak materiałów ze studiów). Przykłady liczbowe zostały sprawdzone obliczeniowo. Jeśli prowadzący używał innej notacji lub kolejności, dopasuj do niej odpowiedzi.

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

---
[⬅️ Poprzedni temat](AlgorytmyKryptograficzne_tytul.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](1_Kryptografia_i_kryptoanaliza_cele_mechanizmów_kryptograficznych.md)