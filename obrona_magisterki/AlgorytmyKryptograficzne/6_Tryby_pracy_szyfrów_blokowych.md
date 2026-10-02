# Tryby pracy szyfrów blokowych (ECB, CBC, CFB, OFB, CTR, GCM)

## Dlaczego tryby są potrzebne

**Szyfr blokowy** (np. AES) to funkcja szyfrująca **jeden blok o stałej długości** (AES: 128 bitów) kluczem: $E_k:\{0,1\}^{128}\to\{0,1\}^{128}$ – odwracalna permutacja. Rzeczywiste wiadomości są dłuższe, więc potrzebny jest **tryb pracy** (*mode of operation*) – sposób użycia szyfru blokowego do przetwarzania dowolnie długich danych.

Gdyby każdy blok szyfrować niezależnie tym samym kluczem (**tryb ECB**), identyczne bloki tekstu dawałyby identyczne szyfrogramy – ujawniałoby to **strukturę danych**. Dobry tryb musi zapewniać:

- **bezpieczeństwo semantyczne (IND-CPA)** – ten sam tekst szyfrowany dwukrotnie daje różne szyfrogramy (przez **IV/nonce**),
- obsługę **dowolnej długości** danych,
- często **równoległość**, dostęp swobodny, brak dopełnienia,
- (w trybach AEAD) **integralność i uwierzytelnienie**.

**IV** (wektor inicjujący) – wartość startowa; **nonce** – liczba używana raz. Wymagania zależą od trybu (zob. tabela).

## Opis trybów

Oznaczenia: $P_i$ – blok tekstu jawnego, $C_i$ – szyfrogramu, $E_k$ / $D_k$ – szyfrowanie/deszyfrowanie blokowe.

### ECB – Electronic Codebook

$$C_i=E_k(P_i)$$

- Bloki szyfrowane **niezależnie**.
- **Wady:** **deterministyczny**; ujawnia powtórzenia (słynny przykład „pingwina" – obraz zaszyfrowany ECB zachowuje kształt); brak dyfuzji między blokami; możliwość **przestawiania/usuwania/powtarzania** bloków (cut-and-paste).
- **Zalety:** równoległy, brak propagacji błędów.
- **Wniosek: nie używać do danych** (dopuszczalny co najwyżej do szyfrowania pojedynczego bloku, np. klucza).

### CBC – Cipher Block Chaining

$$C_0=IV,\quad C_i=E_k(P_i\oplus C_{i-1})$$
$$P_i=D_k(C_i)\oplus C_{i-1}$$

- Każdy blok tekstu jest XOR-owany z poprzednim szyfrogramem.
- **IV musi być nieprzewidywalny (losowy)** i nie musi być tajny – inaczej atak (np. BEAST w TLS 1.0).
- Szyfrowanie **sekwencyjne**, deszyfrowanie **równoległe**.
- Wymaga **dopełnienia (paddingu)** (PKCS#7).
- Propagacja błędu: uszkodzenie $C_i$ psuje $P_i$ i zmienia jeden bit w $P_{i+1}$.
- **Zagrożenia:** **atak wyroczni dopełnienia (padding oracle)** – gdy serwer ujawnia, czy padding jest poprawny (Vaudenay, POODLE, Lucky13); brak integralności (możliwe manipulacje bitami); wymaga MAC (**Encrypt-then-MAC**).

### CFB – Cipher Feedback

$$C_i=P_i\oplus E_k(C_{i-1}),\quad C_0=IV$$

- Zamienia szyfr blokowy w **strumieniowy z samosynchronizacją**; można szyfrować porcje mniejsze niż blok (CFB-8).
- Szyfrowanie sekwencyjne, deszyfrowanie równoległe; **nie wymaga paddingu**; używa tylko operacji $E_k$ (nie potrzeba $D_k$).
- Błąd w szyfrogramie psuje bieżący i następny blok (samosynchronizacja).

### OFB – Output Feedback

$$O_0=IV,\quad O_i=E_k(O_{i-1}),\quad C_i=P_i\oplus O_i$$

- **Strumień klucza niezależny od tekstu** (generowany przez wielokrotne szyfrowanie IV) – tryb synchroniczny strumieniowy.
- **Nie wymaga paddingu**; błąd bitu w szyfrogramie → błąd tego samego bitu w tekście (brak propagacji).
- Strumień można wyliczyć z góry; generowania strumienia nie da się zrównoleglić.
- **IV musi być unikalny** (powtórzenie IV ujawnia XOR tekstów).

### CTR – Counter

$$C_i=P_i\oplus E_k(\text{nonce}\,\|\,\text{licznik}_i)$$

- Szyfr blokowy jako **generator strumienia klucza**: szyfrujemy kolejne wartości licznika.
- **W pełni równoległy** (szyfrowanie i deszyfrowanie), **swobodny dostęp** do dowolnego bloku, brak paddingu, tylko operacja $E_k$.
- **Krytyczne:** para **(klucz, nonce+licznik)** nigdy nie może się powtórzyć – inaczej ujawnienie $P_1\oplus P_2$ (jak w OTP).
- Brak integralności – bity w szyfrogramie można celowo zmieniać (**plastyczność**) → potrzebny MAC.
- Podstawa trybu **GCM**.

### GCM – Galois/Counter Mode (AEAD)

- **Szyfrowanie** w trybie CTR + **uwierzytelnienie** funkcją **GHASH** (mnożenie w ciele Galois $GF(2^{128})$), co daje **tag uwierzytelniający** (zwykle 128 bitów).
- **AEAD** (*Authenticated Encryption with Associated Data*): chroni poufność i integralność **oraz** uwierzytelnia dodatkowe dane jawne (nagłówki – AAD).
- Wejście: klucz, **nonce (zwykle 96 bitów, unikalny!)**, AAD, tekst jawny; wyjście: szyfrogram + tag. Odbiorca weryfikuje tag **przed** użyciem danych.
- **Wysoka wydajność** (równoległość, sprzętowe PCLMULQDQ/AES-NI); standard w **TLS 1.2/1.3**, IPsec, SSH.
- **Pułapki:** **powtórzenie nonce przy tym samym kluczu jest katastrofą** (wyciek klucza uwierzytelniającego, fałszowanie, odczyt XOR tekstów); limit liczby wiadomości na klucz przy losowych nonce (ok. $2^{32}$).
- Inne AEAD: **CCM** (CTR + CBC-MAC), **OCB**, **ChaCha20-Poly1305**, **AES-GCM-SIV** (odporny na powtórzenie nonce).

### Inne tryby *(uzupełnienie)*

- **XTS** – szyfrowanie dysków (nośniki, sektory),
- **CBC-MAC/CMAC** – uwierzytelnianie wiadomości (nie szyfrowanie).

## Porównanie

| Tryb | Rodzaj | IV / nonce | Padding | Równoległość (szyfr. / deszyfr.) | Propagacja błędu | Uwierzytelnienie |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **ECB** | blokowy | brak | tak | tak / tak | tylko blok | nie |
| **CBC** | blokowy | losowy IV | tak | **nie** / tak | blok + 1 bit w następnym | nie |
| **CFB** | strumień (samosynchr.) | unikalny IV | nie | nie / tak | bieżący + następny blok | nie |
| **OFB** | strumień (synchroniczny) | unikalny IV | nie | nie / nie | brak (bit→bit) | nie |
| **CTR** | strumień | unikalny nonce+licznik | nie | **tak / tak** | brak (bit→bit) | nie |
| **GCM** | strumień + MAC (AEAD) | **unikalny nonce** | nie | tak / tak | brak (tag wykryje) | **tak** |

## Dobór trybu w praktyce

1. **Preferuj AEAD:** **AES-GCM** lub **ChaCha20-Poly1305** (jednocześnie poufność i integralność).
2. Jeśli AEAD niedostępny: **CTR** (lub CBC z losowym IV) + **MAC w układzie Encrypt-then-MAC**.
3. **Nigdy ECB** do danych.
4. Zarządzaj **nonce/IV**: unikalne dla każdej wiadomości przy danym kluczu (licznik lub losowe o dostatecznej długości); IV nie jest tajny, ale musi być przekazany odbiorcy.
5. Uważaj na **oracle'e** (padding, błędy), stały czas porównania tagów.
6. Rotuj klucze, ograniczaj ilość danych na klucz.

## Podsumowanie

- Tryby pracy pozwalają szyfrować **dowolnie długie dane** blokowym szyfrem i dają **bezpieczeństwo semantyczne** (ECB go nie ma).
- **ECB** – bloki niezależnie (ujawnia wzorce, nie używać); **CBC** – łańcuchowanie z losowym IV, padding, ryzyko padding oracle; **CFB/OFB/CTR** – szyfr blokowy jako strumieniowy; **CTR** – równoległy, nonce unikalny; **GCM** – CTR + GHASH = **AEAD** (poufność + integralność), standard w TLS, ale nonce nie wolno powtarzać.

---
[⬅️ Poprzedni temat](5_Szyfry_klasyczne_a_nowoczesne_algorytmy_kryptograficzne.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Rola_klucza_w_kryptografii_symetrycznej_i_asymetrycznej.md)