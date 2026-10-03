# Przedstaw podstawową klasyfikację systemów kryptograficznych. Wyjaśnij różnice między systemami symetrycznymi i asymetrycznymi oraz strumieniowymi i blokowymi.

Systemy kryptograficzne można klasyfikować według dwóch niezależnych kryteriów: **rodzaju klucza** oraz **sposobu przetwarzania danych**.

## 1. Podział według klucza

- **Symetryczne:** nadawca i odbiorca używają **tego samego tajnego klucza** do szyfrowania i deszyfrowania (AES, ChaCha20, 3DES). Są szybkie i dobre do dużych ilości danych. Problemem jest **bezpieczna dystrybucja klucza** i liczba kluczy, która rośnie jak n(n-1)/2 przy n użytkownikach.
- **Asymetryczne:** każdy użytkownik ma **parę kluczy**, publiczny i prywatny (RSA, ElGamal, ECC). Klucz publiczny można jawnie rozpowszechniać, a prywatny pozostaje tajny. Rozwiązują problem dystrybucji kluczy i umożliwiają podpisy cyfrowe, ale są **znacznie wolniejsze**.

W praktyce stosuje się **systemy hybrydowe** (TLS, SSH, PGP): kryptografia asymetryczna służy do uzgodnienia klucza sesji, a symetryczna do szyfrowania właściwych danych.

## 2. Podział według sposobu przetwarzania (dotyczy szyfrów symetrycznych)

- **Blokowe:** dane dzielone są na **bloki o stałej długości** (np. 128 bitów w AES) i każdy blok jest szyfrowany tym samym kluczem. Muszą być używane w odpowiednim **trybie pracy** (CBC, CTR, GCM), a ostatni blok może wymagać dopełnienia. Budowane są na strukturach takich jak sieć Feistela (DES) lub SPN (AES).
- **Strumieniowe:** szyfrują dane **bit po bicie lub bajt po bajcie**. Generator tworzy pseudolosowy strumień klucza, który jest XOR-owany z tekstem jawnym (ChaCha20, RC4; wzorcem idealnym jest szyfr z kluczem jednorazowym). Są szybkie i dobre do danych płynących na bieżąco, ale **ponowne użycie tego samego strumienia klucza (nonce) jest katastrofalne**.

Tryby takie jak CTR czy OFB pozwalają zamienić szyfr blokowy w strumieniowy.

**Dodatkowo** szyfry dzieli się na deterministyczne i probabilistyczne (te drugie dla tego samego tekstu dają różne szyfrogramy), a oprócz szyfrowania w kryptografii wyróżnia się też funkcje skrótu, kody MAC i podpisy cyfrowe.

## Podsumowanie

- **Symetryczne** – jeden wspólny tajny klucz, szybkie, problem dystrybucji kluczy (AES, ChaCha20); **asymetryczne** – para kluczy, wolne, rozwiązują dystrybucję i podpisy (RSA, ECC, ElGamal).
- **Blokowe** – bloki stałej długości + tryb pracy (AES); **strumieniowe** – szyfrowanie bit/bajt strumieniem klucza (ChaCha20).
- W praktyce stosuje się **systemy hybrydowe**: asymetria do kluczy/podpisów, symetria do danych.

---
[⬅️ Poprzedni temat](1_Kryptografia_i_kryptoanaliza_cele_mechanizmów_kryptograficznych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️️](3_Zasada_Kerckhoffsa.md)