# Funkcje skrótu (haszujące) – cechy bezpiecznej kryptograficznej funkcji skrótu

## Definicja

**Funkcja skrótu (hash function)** $h$ przekształca dane **dowolnej długości** w ciąg bitów **stałej długości** (skrót, *digest*, *hash*, „odcisk palca" danych):

$$h:\{0,1\}^*\to\{0,1\}^n$$

Nie ma klucza. Skrót SHA-256 ma zawsze 256 bitów (64 znaki hex), niezależnie od rozmiaru wejścia.

**Przykład (SHA-256, sprawdzony):**

- `SHA-256("abc")` = `ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad`,
- zmiana jednego znaku (`"abd"`) daje zupełnie inny skrót: `a52d159f262b2c6d…` – **efekt lawinowy**.

Dla porównania: MD5(`abc`) = `900150983cd24fb0d6963f7d28e17f72` (128 b), SHA-1(`abc`) = `a9993e364706816aba3e25717850c26c9cd0d89d` (160 b).

## Cechy bezpiecznej kryptograficznej funkcji skrótu

### Cechy podstawowe (funkcjonalne)

| Cecha | Opis |
| :--- | :--- |
| **Deterministyczność** | ta sama wiadomość → zawsze ten sam skrót |
| **Stała długość wyjścia** | niezależnie od długości wejścia |
| **Wydajność** | szybkie obliczanie dla dowolnie dużych danych (dla haseł celowo wolne – KDF) |
| **Efekt lawinowy (avalanche)** | zmiana 1 bitu wejścia zmienia ok. 50% bitów skrótu; wyjście wygląda losowo |

### Cechy bezpieczeństwa (kryptograficzne)

| Cecha | Definicja | Koszt ataku (idealna funkcja $n$-bitowa) |
| :--- | :--- | :--- |
| **Odporność na odwracanie (preimage resistance, jednokierunkowość)** | mając skrót $y$, trudno znaleźć jakiekolwiek $m$, że $h(m)=y$ | $\approx2^{n}$ |
| **Odporność na drugi przeciwobraz (second-preimage resistance)** | mając $m_1$, trudno znaleźć $m_2\ne m_1$ z $h(m_2)=h(m_1)$ | $\approx2^{n}$ |
| **Odporność na kolizje (collision resistance)** | trudno znaleźć **jakąkolwiek** parę $m_1\ne m_2$ z $h(m_1)=h(m_2)$ | $\approx2^{n/2}$ (**atak urodzinowy**) |

**Hierarchia siły:** odporność na kolizje ⟹ odporność na drugi przeciwobraz (zwykle) ⟹ odporność na odwracanie (w szerokim sensie). Najtrudniej osiągnąć odporność na kolizje, bo koszt ataku to tylko $2^{n/2}$.

### Paradoks urodzin

W grupie ok. $\sqrt{N}$ elementów zachodzi kolizja z istotnym prawdopodobieństwem. Dla skrótu $n$-bitowego kolizję znajduje się po ok. $2^{n/2}$ wartościach (np. 64-bitowy skrót – po $2^{32}\approx4\cdot10^9$; 128-bitowy – $2^{64}$). Dlatego **do odporności na kolizje na poziomie 128 bitów potrzebny jest skrót 256-bitowy** (SHA-256).

### Cechy dodatkowe

- **Brak korelacji** wejścia i wyjścia, brak łatwej do przewidzenia struktury,
- **Odporność na ataki na długość** (*length extension*): konstrukcja Merkle–Damgård (MD5, SHA-1, SHA-2) pozwala obliczyć $h(m\|\text{pad}\|m')$ z $h(m)$ bez znajomości $m$ – dlatego do uwierzytelniania stosuje się **HMAC**, a nie $h(\text{klucz}\|m)$; SHA-3 i SHA-512/256 nie mają tej wady,
- **Losowość „wyroczni" (random oracle)** – idealizowany model analizy.

## Algorytmy

| Algorytm | Długość | Status |
| :--- | :-: | :--- |
| **MD5** | 128 | **złamany** (kolizje w sekundach); nie używać do bezpieczeństwa |
| **SHA-1** | 160 | **złamany** (SHAttered, 2017: kolizja); wycofywany |
| **SHA-2**: SHA-224/256/384/512, SHA-512/256 | 224–512 | **bezpieczne**, powszechnie stosowane |
| **SHA-3** (Keccak, konstrukcja **gąbki**) | 224–512 | bezpieczne; standard NIST (2015), inna konstrukcja niż SHA-2 |
| **BLAKE2/BLAKE3** | 256–512 | szybkie, bezpieczne |
| **RIPEMD-160** | 160 | starszy, rzadko używany (Bitcoin) |

Konstrukcje: **Merkle–Damgård** (MD5, SHA-1, SHA-2: blok po bloku z funkcją kompresji i wektorem stanu), **gąbka** (SHA-3).

## Zastosowania

| Zastosowanie | Opis |
| :--- | :--- |
| **Integralność plików i danych** | sumy kontrolne (SHA-256 przy pobieraniu programów), systemy kontroli wersji (Git – SHA-1/SHA-256) |
| **Podpisy cyfrowe** | podpisuje się skrót dokumentu (temat 11) |
| **Uwierzytelnianie wiadomości** | **HMAC** (temat 13) |
| **Przechowywanie haseł** | skrót z **solą** (unikalna losowa wartość) i **wolna funkcja KDF**: **Argon2id, bcrypt, scrypt, PBKDF2** (nie zwykły SHA-256/MD5 – zbyt szybkie dla ataków słownikowych/GPU) |
| **Drzewa Merkle'a, blockchain** | skróty łańcuchowo łączące bloki (Bitcoin: podwójny SHA-256), certyfikat przejrzystości |
| **Zobowiązania kryptograficzne (commitments)** | $h(m\|r)$ – ukrywa $m$ i wiąże do niego |
| **Wyprowadzanie kluczy, losowość** | HKDF, generatory pseudolosowe |
| **Deduplikacja i adresowanie treścią** | systemy plików, IPFS |
| **Dowód pracy** | Bitcoin (szukanie skrótu z zerami na początku) |

## Przykłady ataków

- **Kolizje MD5** (Wang 2004) – fałszywe certyfikaty X.509 (2008), złośliwe oprogramowanie Flame (2012),
- **Kolizje SHA-1** (2017, Google/CWI): dwa różne pliki PDF z tym samym skrótem; chosen-prefix collision (2019/2020),
- **Tablice tęczowe i ataki słownikowe** na nasolone skróty – obrona: sól + KDF,
- **Atak z rozszerzeniem długości** na $h(k\|m)$ – obrona: HMAC.

## Podsumowanie

- **Funkcja skrótu** zamienia dowolne dane na skrót stałej długości; **deterministyczna, szybka, efekt lawinowy**.
- Bezpieczeństwo = **odporność na odwracanie ($2^n$), na drugi przeciwobraz ($2^n$) i na kolizje ($2^{n/2}$, atak urodzinowy)**.
- Aktualnie: **SHA-256/384/512, SHA-3, BLAKE2/3**; **MD5 i SHA-1 złamane**.
- Zastosowania: integralność, podpisy, HMAC, hasła (z solą i KDF), blockchain, zobowiązania.
