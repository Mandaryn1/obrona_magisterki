# Na czym opiera się bezpieczeństwo kryptosystemu RSA? Jaką rolę odgrywają w nim liczby pierwsze, arytmetyka modularna i trudność faktoryzacji?

## Czym jest RSA

**RSA** – **kryptosystem klucza publicznego** służący do **szyfrowania** i **podpisów cyfrowych**. Jego bezpieczeństwo opiera się na **trudności rozkładu dużej liczby na czynniki pierwsze (faktoryzacji)**.

## Generowanie kluczy

1. Wybierz **dwie duże, losowe liczby pierwsze** $p$ i $q$ (każda ≥ 1024 bity przy kluczu 2048-bitowym).
2. Oblicz **moduł** $n=p\cdot q$.
3. Oblicz **funkcję Eulera** $\varphi(n)=(p-1)(q-1)$.
4. Wybierz **wykładnik publiczny** $e$ takie, że $1<e<\varphi(n)$ i $\gcd(e,\varphi(n))=1$ (zwykle $e=65537=2^{16}+1$).
5. Oblicz **wykładnik prywatny** $d\equiv e^{-1}\pmod{\varphi(n)}$ (rozszerzony algorytm Euklidesa), tzn. $e\cdot d\equiv1\pmod{\varphi(n)}$.

- **Klucz publiczny:** $(n,e)$. **Klucz prywatny:** $d$ (oraz $p,q,\varphi(n)$ tajne).

## Szyfrowanie i deszyfrowanie

$$
c=m^e\bmod n\qquad m=c^d\bmod n
$$

(dla wiadomości $0\le m<n$). Podpis: $s=h^d\bmod n$, weryfikacja: $s^e\bmod n=h$.

## Rola liczb pierwszych

- Liczby $p,q$ są **tajną „zapadką" (trapdoor)**: znajomość rozkładu $n=pq$ pozwala **łatwo obliczyć $\varphi(n)$**, a więc $d$.
- Mnożenie dużych liczb pierwszych jest **łatwe** ($n=p\cdot q$), a **rozkład** $n$ na czynniki jest **trudny** – to asymetria, na której opiera się funkcja jednokierunkowa RSA.
- Liczby pierwsze muszą być **losowe, duże i niezwiązane** (ani $p\approx q$ – atak Fermata, ani $p-1$ lub $p+1$ o małych czynnikach – atak Pollarda), generowane z dobrej losowości.
- **Testy pierwszości:** probabilistyczne **Miller-Rabin** (praktycznie standard), deterministyczny AKS (wolny). Generowanie klucza = wybór losowej liczby nieparzystej i test pierwszości (gęstość liczb pierwszych ok. $1/\ln N$).

## Rola arytmetyki modularnej

- Wszystkie operacje wykonywane są **modulo $n$** – wyniki mieszczą się w $[0,n)$ i „zawijają się", co daje **funkcję „zapadkową"** (obliczanie $m^e\bmod n$ jest szybkie, odwracanie bez $d$ – trudne).
- **Szybkie potęgowanie modularne** (*square-and-multiply*) – złożoność $O(\log e)$ mnożeń; dla $e=65537$ szyfrowanie jest szybkie. Deszyfrowanie jest wolniejsze ($d$ duże) – przyspiesza **CRT** (chińskie twierdzenie o resztach – liczenie mod $p$ i mod $q$ osobno, ok. 4× szybciej).
- **Odwrotność modularna** ($d=e^{-1}\bmod\varphi(n)$) – rozszerzony algorytm Euklidesa.
- **Twierdzenie Eulera / Fermata** gwarantuje poprawność deszyfrowania.

## Rola trudności faktoryzacji

Bezpieczeństwo RSA bazuje na dwóch powiązanych założeniach:

1. **Problem faktoryzacji:** mając $n=pq$, znaleźć $p,q$. Jeśli ktoś je znajdzie, wylicza $\varphi(n)$ i $d$ → **łamie RSA całkowicie**.
2. **Problem RSA:** mając $(n,e,c)$, znaleźć $m$ takie, że $m^e\equiv c\pmod n$. **Rozwiązanie faktoryzacji ⟹ rozwiązanie problemu RSA**; odwrotnie – nie wiadomo, czy to równoważne (problem RSA nie jest udowodnione tak trudny jak faktoryzacja).

[Komputer **kwantowy** z algorytmem **Shora** rozłożyłby $n$ w czasie wielomianowym – RSA przestanie być bezpieczny.](./15_Komputery_kwantowe_a_kryptografia_kwantowa_i_postkwantowa.md)

## Zastosowania i praktyka

- **Szyfrowanie kluczy symetrycznych** (RSA-KEM, RSA-OAEP), **podpisy** (RSA-PSS, RSASSA-PKCS1-v1_5), certyfikaty X.509, SSH, PGP/GPG.
- **Nie do szyfrowania dużych danych** – ograniczony rozmiar wiadomości ($<n$) i wolny; używa się **hybrydowo** (RSA + AES).
- W nowych systemach często zastępowany przez **ECC** (krótsze klucze) i docelowo przez algorytmy postkwantowe.

## Podsumowanie

- **RSA:** $n=pq$, $\varphi(n)=(p-1)(q-1)$, $ed\equiv1\bmod\varphi(n)$; $c=m^e\bmod n$, $m=c^d\bmod n$.
- **Liczby pierwsze** tworzą tajną zapadkę; **arytmetyka modularna** (twierdzenie Eulera, szybkie potęgowanie, odwrotność modularna) zapewnia poprawność i wydajność; bezpieczeństwo = **trudność faktoryzacji** (problem RSA).
- Wymaga kluczy ≥ 2048 bitów, losowych liczb pierwszych i **paddingu (OAEP/PSS)**; zagrożony przez komputery kwantowe (Shor).

---

[⬅️ Poprzedni temat](8_Protokół_Diffiego-Hellmana.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Porównanie_RSA_i_ElGamala.md)
