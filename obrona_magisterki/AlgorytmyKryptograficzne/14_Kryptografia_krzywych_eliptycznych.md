# Kryptografia krzywych eliptycznych (ECC)

## Główna idea

**ECC (Elliptic Curve Cryptography)** (Koblitz i Miller, 1985) to kryptografia klucza publicznego oparta na **grupie punktów krzywej eliptycznej** nad ciałem skończonym. Zamiast potęgowania modulo $p$ (jak w DH/ElGamal) używa się **mnożenia punktu przez skalar**, a trudnym problemem jest **logarytm dyskretny na krzywej eliptycznej (ECDLP)**.

## Krzywa eliptyczna

Nad ciałem skończonym $\mathbb{F}_p$ krzywa w postaci Weierstrassa:

$$y^2\equiv x^3+ax+b\pmod p,\qquad 4a^3+27b^2\not\equiv0$$

Zbiór punktów $(x,y)$ spełniających równanie wraz z **punktem w nieskończoności** $\mathcal{O}$ (element neutralny) tworzy **grupę abelową** z działaniem **dodawania punktów** (interpretacja geometryczna: prosta przez dwa punkty przecina krzywą w trzecim, odbicie względem osi $x$ daje sumę).

**Dodawanie punktów** $P=(x_1,y_1)$, $Q=(x_2,y_2)$, $R=P+Q=(x_3,y_3)$:

$$\lambda=\begin{cases}\dfrac{y_2-y_1}{x_2-x_1}&P\ne Q\\[2mm]\dfrac{3x_1^2+a}{2y_1}&P=Q\ (\text{podwajanie})\end{cases}\qquad x_3=\lambda^2-x_1-x_2,\quad y_3=\lambda(x_1-x_3)-y_1\ \ (\bmod p)$$

(dla $P=-Q$ wynik to $\mathcal{O}$).

### Mnożenie przez skalar

$$Q=k\cdot P=\underbrace{P+P+\dots+P}_{k}$$

Obliczanie jest szybkie (**double-and-add**, $O(\log k)$ operacji). Odwrócenie – **ECDLP**: mając $P$ i $Q=kP$, znaleźć $k$ – jest **trudne**; najlepsze znane ogólne algorytmy (Pollard rho) mają złożoność **wykładniczą $\sim\sqrt{n}$** dla grupy rzędu $n$ (brak sub-wykładniczych algorytmów, jak sito ciała liczbowego dla $\mathbb{Z}_p^*$/RSA).

### Przykład (sprawdzony)

Krzywa $y^2=x^3+2x+2$ nad $\mathbb{F}_{17}$, punkt bazowy $G=(5,1)$ o rzędzie $19$:

| $k$ | 1 | 2 | 3 | 4 | 5 | 6 | … | 18 | 19 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| $kG$ | (5,1) | (6,3) | (10,6) | (3,1) | (9,16) | (16,13) | … | (5,16) | $\mathcal{O}$ |

**ECDH:** klucz prywatny Alicji $a=3$ → publiczny $A=3G=(10,6)$; Boba $b=7$ → $B=7G=(0,6)$. Wspólny sekret: $a\cdot B=3\cdot(0,6)=21G=2G=(6,3)$ oraz $b\cdot A=7\cdot(10,6)=21G=(6,3)$ ✓ (rząd 19, więc $21G=2G$).

## Zastosowania ECC

| Mechanizm | Opis |
| :--- | :--- |
| **ECDH / ECDHE** | uzgadnianie klucza (efemeryczne – forward secrecy) – TLS 1.3, SSH, Signal, WireGuard (X25519) |
| **ECDSA** | podpis cyfrowy (certyfikaty, Bitcoin/Ethereum – secp256k1); wymaga unikalnego losowego $k$ |
| **EdDSA (Ed25519)** | deterministyczny, szybki podpis na krzywej Edwardsa |
| **ECIES** | szyfrowanie hybrydowe (analog ElGamala na krzywej) |
| Inne | ECMQV, uwierzytelnianie w kartach i paszportach (ePaszport), IoT, TLS, S/MIME, PGP |

### Popularne krzywe

| Krzywa | Uwagi |
| :--- | :--- |
| **NIST P-256 (secp256r1), P-384, P-521** | standard NIST/FIPS, najpowszechniejsze w TLS/certyfikatach |
| **secp256k1** | Bitcoin, Ethereum |
| **Curve25519 (X25519), Ed25519, Ed448** | Bernstein; projekt „sztywny" (rigid), szybkie i odporne na błędy implementacyjne – obecnie preferowane |
| **Brainpool** | alternatywa europejska (BSI) |

## Dlaczego ECC jest atrakcyjna w porównaniu z RSA/DH

Najlepszy znany atak na ECDLP jest **wykładniczy**, a na faktoryzację/DLP w $\mathbb{Z}_p^*$ – **sub-wykładniczy**, dlatego dla tego samego poziomu bezpieczeństwa **klucze ECC są znacznie krótsze**:

| Poziom bezpieczeństwa (bity) | RSA / DH (moduł) | **ECC** |
| :-: | :-: | :-: |
| 80 | 1024 | 160 |
| 112 | 2048 | 224 |
| **128** | **3072** | **256** |
| 192 | 7680 | 384 |
| 256 | 15360 | 512 (521) |

**Korzyści:**

- **krótkie klucze i podpisy** (256 bitów ≈ RSA-3072): mniej pamięci i transmisji (certyfikaty, IoT, protokoły o małej przepustowości),
- **szybsze generowanie kluczy i podpisów** (szybkie wytwarzanie par kluczy; podpis ECDSA/Ed25519 wielokrotnie szybszy niż RSA-3072), mniejsze zużycie energii – urządzenia mobilne, karty, wbudowane,
- **lepsza skalowalność** wraz ze wzrostem poziomu bezpieczeństwa (RSA wymaga gwałtownie rosnących kluczy),
- bogata funkcjonalność (ECDH, podpisy, szyfrowanie hybrydowe), **forward secrecy** przy niskim koszcie,
- szeroka standaryzacja i wsparcie sprzętowe.

*(Uwaga: weryfikacja podpisu RSA z małym $e$ jest szybsza niż ECDSA; ECC wygrywa w generowaniu kluczy i podpisów oraz rozmiarze.)*

## Wady i zagrożenia

| Wada | Opis |
| :--- | :--- |
| **Złożoność implementacji** | łatwo o błędy: **ataki na niewłaściwie zwalidowane punkty** (*invalid curve attack*, małe podgrupy), kanały boczne; trzeba używać sprawdzonych bibliotek |
| **Zaufanie do parametrów krzywych** | stałe NIST budziły podejrzenia (pochodzenie parametrów; *Dual_EC_DRBG* – generator z backdoorem NSA) → preferencja dla Curve25519 |
| **Wrażliwość ECDSA na losowość** | powtórzony lub przewidywalny nonce $k$ ujawnia klucz prywatny (PlayStation 3, portfele Bitcoin na Androidzie) → **EdDSA / RFC 6979** (deterministyczny nonce) |
| **Patenty** (historycznie) | opóźniały adopcję |
| **Krzywe słabe** | krzywe anomalne, supersingularne (MOV), małe stopnie zanurzenia – należy używać standardowych krzywych |
| **Zagrożenie kwantowe** | **algorytm Shora rozwiązuje ECDLP w czasie wielomianowym** – ECC (i ECDH, ECDSA) zostanie złamana przez wystarczająco duży komputer kwantowy; krzywe z mniejszymi kluczami są wręcz **łatwiejsze** do złamania niż RSA o równoważnym poziomie (potrzeba mniej kubitów) → przejście na PQC (temat 15) |

## Porównanie ECC, RSA, DH

| | **RSA** | **DH (klasyczny)** | **ECC** |
| :--- | :--- | :--- | :--- |
| Trudny problem | faktoryzacja | DLP w $\mathbb{Z}_p^*$ | **ECDLP** |
| Najlepszy znany atak | GNFS (sub-wykł.) | GNFS (sub-wykł.) | **Pollard rho ($\sqrt n$, wykładniczy)** |
| Klucz 128-bit. bezp. | 3072 b. | 3072 b. | **256 b.** |
| Podpis | tak | – | tak (ECDSA/EdDSA) |
| Uzgadnianie klucza | RSA-KEM | DH | **ECDH** |
| Wydajność | szybka weryfikacja, wolne podpisy | wolny | **szybki, mało pamięci** |
| Kwanty | zagrożony | zagrożony | zagrożony |

## Podsumowanie

- **ECC** korzysta z grupy punktów krzywej eliptycznej; mnożenie punktu przez skalar $Q=kP$ jest łatwe, a odwrócenie (**ECDLP**) – trudne (atak wykładniczy).
- Daje te same funkcje co RSA/DH (**ECDH, ECDSA/EdDSA, ECIES**) przy **znacznie krótszych kluczach** (256 b. ≈ RSA 3072), szybkości i mniejszym zużyciu zasobów – idealna dla IoT, urządzeń mobilnych, TLS.
- Wady: złożoność implementacji, kwestie zaufania do krzywych, wrażliwość na losowość ECDSA; **nie jest odporna na komputery kwantowe**.
