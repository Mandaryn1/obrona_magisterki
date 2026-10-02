# Kody uwierzytelniania wiadomości (MAC). Porównanie z podpisem cyfrowym

## Czym jest MAC

**MAC (Message Authentication Code, kod uwierzytelniania wiadomości)** to krótki **znacznik (tag)** obliczany z wiadomości i **wspólnego tajnego klucza**:

$$t=\text{MAC}_k(m)$$

Odbiorca znający klucz $k$ oblicza znacznik dla otrzymanej wiadomości i porównuje z dołączonym. Zgodność oznacza, że wiadomość:

- **nie została zmieniona** (**integralność**),
- **pochodzi od posiadacza klucza** (**uwierzytelnienie źródła/autentyczność**).

MAC **nie szyfruje** wiadomości (nie zapewnia poufności) i **nie zapewnia niezaprzeczalności** (klucz mają obie strony).

## Do czego służą kody MAC

- ochrona **integralności i autentyczności** komunikatów w protokołach (TLS, IPsec, SSH),
- **uwierzytelnianie** API i usług (podpisy żądań HMAC – AWS SigV4, webhooks),
- tokeny sesji i ciasteczka podpisane (np. **JWT HS256**),
- **szyfrowanie uwierzytelnione** (Encrypt-then-MAC, tag w AES-GCM i ChaCha20-Poly1305 to w istocie MAC),
- wykrywanie manipulacji danymi w pamięci, plikach i kopiach.

## Konstrukcje MAC

| Konstrukcja | Opis |
| :--- | :--- |
| **HMAC** | oparty na funkcji skrótu: $\text{HMAC}(K,m)=H\big((K'\oplus opad)\,\|\,H((K'\oplus ipad)\,\|\,m)\big)$; $ipad=0x36\ldots$, $opad=0x5C\ldots$; np. **HMAC-SHA-256** (nieczuły na atak z rozszerzeniem długości) |
| **CMAC** (OMAC) | oparty na szyfrze blokowym (AES-CMAC) |
| **CBC-MAC** | tylko dla wiadomości stałej długości (niebezpieczny przy zmiennej) |
| **GMAC** | uwierzytelnianie GCM bez szyfrowania |
| **Poly1305** | szybki MAC (z jednorazowym kluczem) – ChaCha20-Poly1305 |
| **KMAC** | oparty na SHA-3 (Keccak) |

**Przykład (HMAC-SHA-256, sprawdzony):** klucz `key`, wiadomość `The quick brown fox jumps over the lazy dog` → tag `f7bc83f430538424b13298e6aa6fb143ef4d59a14946175997479dbc2d1a3cd8`. Zmiana jednego znaku wiadomości lub klucza daje zupełnie inny tag.

**Dlaczego nie $h(k\|m)$:** podatne na *length extension* (Merkle–Damgård); $h(m\|k)$ podatny na kolizje; dlatego standardem jest **HMAC**.

## Wymagane własności MAC

- **Niemożność fałszerstwa (EUF-CMA):** bez klucza nie da się wytworzyć poprawnego tagu dla nowej wiadomości, nawet po obejrzeniu tagów wielu wybranych wiadomości,
- odpowiednia długość klucza (≥ 128 bitów) i tagu (zwykle 128–256 bitów; skrócenie osłabia),
- **porównanie tagów w stałym czasie** (unikanie ataków czasowych),
- ochrona przed **powtórzeniem (replay)** – do wiadomości dodać nonce/numer sekwencyjny/znacznik czasu (MAC sam tego nie zapewnia).

## Porównanie: MAC a podpis cyfrowy

| Cecha | **MAC** | **Podpis cyfrowy** |
| :--- | :--- | :--- |
| **Rodzaj kryptografii** | **symetryczna** | **asymetryczna** |
| **Klucze** | **jeden wspólny tajny klucz** (nadawca i odbiorca) | para: **prywatny** (podpisuje) i **publiczny** (weryfikuje) |
| **Integralność** | tak | tak |
| **Uwierzytelnienie nadawcy** | tak, ale tylko **wobec strony znającej klucz** | tak, **wobec każdego** (klucz publiczny jawny, certyfikat) |
| **Niezaprzeczalność** | **nie** – obie strony mogą wytworzyć tag, więc nadawca może zaprzeczyć („to Bob go stworzył") | **tak** – tylko właściciel klucza prywatnego mógł podpisać |
| **Weryfikacja przez osobę trzecią** | **nie** (zna klucz → mogłaby też fałszować) | **tak** (publicznie weryfikowalny) |
| **Szybkość** | **bardzo szybka** (setki MB/s–GB/s) | wolniejszy (RSA, ECDSA – setki–tysiące razy) |
| **Rozmiar znacznika** | krótki (16–32 B) | dłuższy (64 B dla Ed25519/ECDSA-256; 256+ B dla RSA-2048) |
| **Dystrybucja kluczy** | konieczne bezpieczne uzgodnienie klucza | tylko klucz publiczny jawnie, potrzebna PKI |
| **Skalowalność** | $n(n-1)/2$ kluczy (para–para) | $n$ par kluczy |
| **Zastosowania** | komunikacja między stronami z wspólnym sekretem: TLS (rekordy), IPsec, API, tokeny wewnętrzne | dokumenty, e-podpis, certyfikaty, aktualizacje oprogramowania, umowy, blockchain |
| **Odporność na kwanty** | HMAC/CMAC – wystarczy podwojenie klucza/długości (Grover) | RSA/ECDSA złamane przez Shora; potrzebne ML-DSA, SLH-DSA |

### Kiedy co wybrać

- **MAC:** dwie strony ufają sobie i mają wspólny klucz; potrzebna szybkość; nie jest potrzebny dowód dla osób trzecich (typowe: sesja TLS, wewnętrzne API).
- **Podpis cyfrowy:** wielu odbiorców, brak wspólnego sekretu, wymagana **niezaprzeczalność** (umowy, publiczne ogłoszenia, kod).
- **Razem:** TLS używa podpisów (uwierzytelnienie serwera przy uzgadnianiu) i MAC/AEAD (ochrona rekordów).

## MAC a funkcja skrótu

- **Funkcja skrótu** – bez klucza; każdy może obliczyć skrót, nie zapewnia autentyczności (atakujący, który zmienia wiadomość, przelicza także skrót).
- **MAC** – skrót **z kluczem tajnym**; tylko posiadacz klucza stworzy poprawny tag.

## Podsumowanie

- **MAC** = tag $\text{MAC}_k(m)$ liczony ze **wspólnym kluczem tajnym**; zapewnia **integralność i autentyczność** (wobec stron znających klucz), **bez niezaprzeczalności** i bez poufności.
- Najczęstsze: **HMAC-SHA-256**, CMAC, Poly1305/GMAC (w AEAD).
- **Podpis cyfrowy**: asymetryczny, publicznie weryfikowalny, daje **niezaprzeczalność**, ale jest wolniejszy; MAC – szybki, symetryczny, tylko między stronami z wspólnym kluczem.

---
[⬅️ Poprzedni temat](12_Funkcje_skrótu.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](14_Kryptografia_krzywych_eliptycznych.md)