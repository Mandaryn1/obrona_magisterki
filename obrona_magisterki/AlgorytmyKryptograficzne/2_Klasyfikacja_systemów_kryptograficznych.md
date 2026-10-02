# Podstawowa klasyfikacja systemów kryptograficznych

## Kryterium 1: liczba i rodzaj kluczy

| | **Symetryczne** (z kluczem tajnym) | **Asymetryczne** (z kluczem publicznym) |
| :--- | :--- | :--- |
| Klucze | **jeden wspólny klucz tajny** $k$ do szyfrowania i deszyfrowania | **para kluczy:** publiczny $pk$ i prywatny $sk$ |
| Szyfrowanie | $c=E_k(m)$, $m=D_k(c)$ | $c=E_{pk}(m)$, $m=D_{sk}(c)$ |
| Szybkość | **bardzo szybkie** (sprzętowe AES-NI) | **wolne** (setki–tysiące razy wolniejsze) |
| Dystrybucja kluczy | **problem**: bezpieczne uzgodnienie wspólnego sekretu | **rozwiązuje** problem – klucz publiczny można jawnie rozpowszechniać |
| Liczba kluczy | $\frac{n(n-1)}{2}$ dla $n$ osób (np. 100 osób → 4950 kluczy) | $n$ par kluczy |
| Funkcje | szyfrowanie, MAC | szyfrowanie, **podpis cyfrowy**, uzgadnianie klucza |
| Przykłady | AES, ChaCha20, 3DES, DES | RSA, ElGamal, ECC (ECDH, ECDSA), DH, DSA |
| Typowy rozmiar klucza | 128–256 bitów | RSA 2048–4096, ECC 256–384 bitów |

**Systemy hybrydowe** (stosowane w praktyce – TLS, PGP, S/MIME): kryptografia asymetryczna służy do **uzgodnienia klucza sesji** (lub podpisu), a właściwe dane szyfruje się szybkim szyfrem **symetrycznym**. Łączą zalety obu (zob. temat 7).

**Poza podziałem:** **funkcje skrótu** (bez klucza), generatory liczb losowych, protokoły (DH, TLS), podpisy, MAC.

## Kryterium 2: sposób przetwarzania danych (szyfry symetryczne)

| | **Blokowe** (*block ciphers*) | **Strumieniowe** (*stream ciphers*) |
| :--- | :--- | :--- |
| Przetwarzanie | **bloki o stałej długości** (np. 128 bitów) | **bit po bicie / bajt po bajcie** |
| Zasada | permutacja zależna od klucza na bloku; długie wiadomości wymagają **trybu pracy** (ECB, CBC, CTR, GCM…) | **generator strumienia klucza** $z_i$ z klucza i nonce; szyfrogram $c_i=m_i\oplus z_i$ |
| Pamięć/stan | bezstanowe (pojedynczy blok), tryby dodają stan | stan wewnętrzny generatora |
| Rozmnażanie błędów | zależne od trybu | brak (błąd 1 bitu → błąd 1 bitu) |
| Wady | wymaga paddingu (w niektórych trybach), tryb ma znaczenie | **nigdy nie używać ponownie** tej samej pary (klucz, nonce) |
| Zalety | uniwersalne, dobrze zbadane, dobry sprzęt (AES-NI) | bardzo szybkie programowo, małe opóźnienie, dobre do strumieni (VoIP, TLS) |
| Przykłady | **AES** (blok 128 b, klucz 128/192/256), DES (64/56), 3DES, Blowfish, Twofish | **ChaCha20**, Salsa20, RC4 (**niezalecany**), A5/1 (GSM, złamany) |

Szyfr blokowy w trybie CTR/OFB/CFB zachowuje się jak strumieniowy (zob. temat 6).

## Inne klasyfikacje

### Historyczna / ze względu na przekształcenie

- **podstawieniowe** (substytucyjne) – zamiana symboli na inne (Cezar, Vigenère),
- **przestawieniowe** (transpozycyjne) – zmiana kolejności symboli (płotowy, kolumnowy),
- **produktowe (złożone)** – ich wielokrotne połączenie (współczesne szyfry blokowe: sieci SP, Feistela).

### Ze względu na poziom bezpieczeństwa

- **bezwarunkowo bezpieczne** (tajność doskonała – szyfr z kluczem jednorazowym),
- **obliczeniowo bezpieczne** (praktycznie wszystkie współczesne),
- **dowodliwie bezpieczne** (przy redukcji do trudnego problemu, np. RSA-OAEP, ElGamal przy DDH).

### Ze względu na strukturę szyfru blokowego

- **sieć Feistela** (DES, 3DES, Blowfish) – odwracalne niezależnie od funkcji rundowej,
- **sieć podstawieniowo-permutacyjna – SPN** (**AES**).

### Ze względu na losowość

- **deterministyczne** (ten sam tekst → ten sam szyfrogram; np. RSA bez paddingu, ECB),
- **probabilistyczne/randomizowane** (ElGamal, RSA-OAEP, CBC z losowym IV) – wymagane dla bezpieczeństwa semantycznego.

### Ze względu na funkcje

szyfrowanie, uwierzytelnianie (MAC, podpis), **AEAD** (szyfrowanie z uwierzytelnieniem: GCM, ChaCha20-Poly1305), wymiana kluczy (DH, KEM), skróty (hash), KDF, PRNG.

## Porównanie: symetryczne vs asymetryczne – kiedy co

| Zastosowanie | Wybór |
| :--- | :--- |
| szyfrowanie dużych ilości danych, dysków, plików, transmisji | **symetryczne** |
| bezpieczne uzgodnienie klucza z nieznajomym | **asymetryczne** (DH/ECDH, RSA/KEM) |
| podpis cyfrowy, certyfikaty | **asymetryczne** |
| uwierzytelnianie wiadomości między stronami z wspólnym kluczem | **MAC** (symetryczny) |
| TLS, PGP, S/MIME, SSH | **hybrydowo** |

## Podsumowanie

- **Symetryczne** – jeden wspólny tajny klucz, szybkie, problem dystrybucji kluczy (AES, ChaCha20); **asymetryczne** – para kluczy, wolne, rozwiązują dystrybucję i podpisy (RSA, ECC, ElGamal).
- **Blokowe** – bloki stałej długości + tryb pracy (AES); **strumieniowe** – szyfrowanie bit/bajt strumieniem klucza (ChaCha20).
- W praktyce stosuje się **systemy hybrydowe**: asymetria do kluczy/podpisów, symetria do danych.

---
[⬅️ Poprzedni temat](1_Kryptografia_i_kryptoanaliza_cele_mechanizmów_kryptograficznych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️️](3_Zasada_Kerckhoffsa.md)