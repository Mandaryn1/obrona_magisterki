# Zastosowanie kryptografii w ochronie danych i komunikacji

## 1. Podstawowe pojęcia i algorytmy kryptograficzne

Kryptografia zapewnia poufność, integralność, autentyczność oraz niezaprzeczalność danych w systemach informatycznych i sieciach.

* **Szyfrowanie symetryczne:**
  * Ten sam klucz służy do szyfrowania i odszyfrowywania danych.
  * **Cechy:** Bardzo wysoka wydajność i szybkość.
  * **Przykłady:** AES (Advanced Encryption Standard — standard branżowy z kluczami 128/256-bit), ChaCha20.
  * **Wada:** Problem bezpiecznego przekazania klucza odbiorcy.
* **Szyfrowanie asymetryczne (kryptografia klucza publicznego):**
  * Wykorzystuje parę matematycznie powiązanych kluczy: **klucz publiczny** (jawny, służy do szyfrowania lub weryfikacji podpisu) oraz **klucz prywatny** (tajny, służy do odszyfrowywania lub generowania podpisu).
  * **Przykłady:** RSA, ECC (Elliptic Curve Cryptography / Kryptografia krzywych eliptycznych), Diffie-Hellman (wymiana kluczy).
  * **Zastosowanie:** Bezpieczna wymiana kluczy symetrycznych, podpisy cyfrowe, uwierzytelnianie (np. SSH).
* **Funkcje skrótu (Hashing) i Podpis Cyfrowy:**
  * **Funkcje skrótu:** Jednokierunkowe przekształcenie danych w ciąg o stałej długości (np. SHA-256). Służą do weryfikacji integralności danych oraz bezpiecznego przechowywania haseł.
  * **Podpis cyfrowy:** Połączenie funkcji skrótu z szyfrowaniem asymetrycznym (podpisanie hashu pliku kluczem prywatnym). Gwarantuje autentyczność nadawcy i niezaprzeczalność.

---

## 2. Ochrona danych w spoczynku (Data at Rest)

Ochrona danych przechowywanych na nośnikach pamięci (dyskach, taśmach, w chmurze) przed fizyczną kradzieżą lub nieautoryzowanym odczytem:

* **Szyfrowanie pełnodyskowe (FDE — Full Disk Encryption):** Szyfrowanie całych partycji systemowych i danych (np. BitLocker w Windows, LUKS w Linux). Ochrona działa na poziomie blokowym — dane są nieczytelne bez podania PIN-u, hasła lub klucza z modułu TPM.
* **Szyfrowanie na poziomie plików i katalogów:** Szyfrowanie wybranych wrażliwych plików (np. EFS w NTFS).
* **Bezpieczne przechowywanie poświadczeń:** Przechowywanie haseł użytkowników wyłącznie w postaci zsolonych skrótów (*salted hashes*) z wykorzystaniem funkcji trudnych obliczeniowo (np. bcrypt, Argon2, PBKDF2), co uniemożliwia odczytanie haseł w przypadku wycieku bazy.

---

## 3. Ochrona danych w transmisji (Data in Transit)

Zabezpieczanie kanałów komunikacyjnych przed podsłuchem (sniffing), modyfikacją pakietów oraz atakami typu *Man-in-the-Middle* (MitM):

* **TLS/SSL (Transport Layer Security):**
  * Szyfruje ruch w warstwie aplikacji (np. HTTPS na porcie 443).
  * Łączy kryptografię asymetryczną (uwierzytelnienie serwera i negocjacja klucza sesyjnego) z szyfrowaniem symetrycznym (szyfrowanie właściwej transmisji danych).
* **SSH (Secure Shell — port 22):** Bezpieczny protokół zarządzania zdalnego, zastępujący niezaszyfrowany Telnet/FTP. Wykorzystuje klucze asymetryczne do uwierzytelniania i tunelowania ruchu.
* **IPsec (Internet Protocol Security):** Zestaw protokołów szyfrujących ruch na poziomie warstwy sieciowej (IP). Stosowany głównie do budowy tuneli **VPN** (Virtual Private Network) łączących odległe sieci lub użytkowników zdalnych z siecią firmową.

---

## 4. Infrastruktura Klucza Publicznego (PKI) i Certyfikaty

* **PKI (Public Key Infrastructure):** Środowisko zarządzające cyklem życia certyfikatów cyfrowych (wydawanie, odnawianie, unieważnianie).
* **Urzędy Certyfikacji (CA — Certificate Authority):** Zaufane podmioty trzecie wydające certyfikaty X.509, które wiążą tożsamość podmiotu z jego kluczem publicznym.
* **Weryfikacja certyfikatów:** Klienci sprawdzają ważność certyfikatu, podpis CA oraz status unieważnienia za pomocą list **CRL** (*Certificate Revocation List*) lub protokołu **OCSP** (*Online Certificate Status Protocol*).
* **Przejrzystość Certyfikatów (Certificate Transparency):** Publiczne rejestry wydanych certyfikatów, pozwalające na audytowanie nadużyć CA oraz wykorzystywane przez testerów penetracyjnych do pasywnego rozpoznania (np. wyszukiwanie subdomen na `crt.sh`).

---

## 5. Podsumowanie do wypowiedzi na obronie

> *"Kryptografia jest filarem ochrony danych zarówno w spoczynku, jak i w transmisji. Szyfrowanie symetryczne (np. AES) zapewnia szybką ochronę dysków (BitLocker, LUKS) oraz danych przesyłanych w sieci, podczas gdy szyfrowanie asymetryczne (RSA, ECC) rozwiązuje problem bezpiecznego przekazania kluczy i uwierzytelniania. W komunikacji sieciowej standardem są protokoły TLS, SSH oraz IPsec VPN, a zaufanie w sieci opiera się na Infrastrukturze Klucza Publicznego (PKI) i certyfikatach X.509 wydawanych przez urzędy CA."*

## Podsumowanie

- **W spoczynku:** FDE (**BitLocker** – TPM, TPM+PIN, klucz odzyskiwania; **LUKS**; **VeraCrypt**), szyfrowanie plików (EFS) – ochrona przed kradzieżą nośnika; zagrożenia: cold boot, Evil Maid, DMA.
- **W tranzycie:** **TLS** (handshake, ECDHE, certyfikaty, PFS), SSH, VPN, WPA3; **PKI** (CA, X.509, CRL/OCSP, ACME, CT).
- **Hasła/integralność:** skróty z solą i KDF, podpisy, **Secure Boot i podpisy kodu**; **klucze:** HSM/TPM/KMS, rotacja, rozdzielenie od danych; **PQC** – przygotowanie na komputery kwantowe.

---
[⬅️ Poprzedni temat](7_Zapobieganie_i_wykrywanie_zagrożeń_w_systemach_operacyjnych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](9_Najczęstsze_ataki_na_aplikacje_webowe_i_mechanizmy_ich_ograniczania.md)