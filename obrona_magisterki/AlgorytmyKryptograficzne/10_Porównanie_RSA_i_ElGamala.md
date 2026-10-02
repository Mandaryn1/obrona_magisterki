# Porównanie kryptosystemów RSA i ElGamala. Problemy matematyczne w podstawie ich bezpieczeństwa

## Podstawy matematyczne

| | **RSA** | **ElGamal** |
| :--- | :--- | :--- |
| Trudny problem | **faktoryzacja** dużych liczb ($n=pq$) / **problem RSA** (pierwiastek $e$-tego stopnia mod $n$) | **logarytm dyskretny (DLP)** w grupie $\mathbb{Z}_p^*$ (lub krzywej eliptycznej); semantyczne bezpieczeństwo – założenie **DDH** |
| Struktura | pierścień $\mathbb{Z}_n$, $n=pq$ | grupa cykliczna z generatorem $g$ |
| Zapadka (trapdoor) | rozkład $n$ na $p,q$ (a więc $\varphi(n)$) | znajomość wykładnika $x$ |
| Rok | 1977 (Rivest, Shamir, Adleman) | 1985 (Taher ElGamal) |

Oba są **nieodporne na komputery kwantowe** (algorytm Shora rozwiązuje zarówno faktoryzację, jak i logarytm dyskretny).

## RSA (skrót)

Klucze: $n=pq$, $e$ publiczny, $d=e^{-1}\bmod\varphi(n)$. Szyfrowanie: $c=m^e\bmod n$; deszyfrowanie: $m=c^d\bmod n$ (zob. temat 9).

## ElGamal – działanie

### Generowanie kluczy

1. Duża liczba pierwsza $p$ i generator $g$ grupy $\mathbb{Z}_p^*$ (parametry publiczne; mogą być wspólne dla wielu użytkowników).
2. **Klucz prywatny:** losowe $x\in\{2,\dots,p-2\}$.
3. **Klucz publiczny:** $y=g^x\bmod p$ (razem z $p,g$).

### Szyfrowanie wiadomości $m<p$

1. Wybierz **losowe, jednorazowe** $k\in\{2,\dots,p-2\}$.
2. Oblicz $c_1=g^k\bmod p$ oraz $c_2=m\cdot y^k\bmod p$.
3. Szyfrogram: **para $(c_1,c_2)$**.

### Deszyfrowanie

$$m=c_2\cdot\left(c_1^{\,x}\right)^{-1}\bmod p$$

Dlaczego działa: $c_1^x=g^{kx}=y^k$, więc $c_2/c_1^x=m\,y^k/y^k=m$. Mechanizm = **uzgodnienie klucza DH** ($y^k=g^{xk}$ jest wspólnym sekretem) + „maskowanie" wiadomości przez pomnożenie.

### Przykład liczbowy (sprawdzony)

$p=23,\ g=5,\ x=6$ → $y=5^6\bmod23=8$. Wiadomość $m=10$, $k=3$:

- $c_1=5^3\bmod23=10$, $c_2=10\cdot8^3\bmod23=10\cdot6\bmod23=14$ → szyfrogram $(10,14)$,
- deszyfrowanie: $c_1^x=10^6\bmod23=6$ (ta sama „maska" co $y^k=8^3\bmod23=6$), $6^{-1}\bmod23=4$, więc $m=14\cdot4\bmod23=56\bmod23=10$ ✓.

### Podpis ElGamala (uzupełnienie)

ElGamal opisuje też **schemat podpisu** (inny algorytm niż szyfrowanie); z niego wywodzi się **DSA** i **ECDSA**. Wymaga unikalnego, tajnego $k$ dla każdego podpisu – powtórzenie $k$ ujawnia klucz prywatny (afera PlayStation 3, błędy w Bitcoin).

## Porównanie własności

| Cecha | **RSA** | **ElGamal** |
| :--- | :--- | :--- |
| Szyfrowanie | **deterministyczne** w wersji „podręcznikowej" (wymaga paddingu OAEP) | **probabilistyczne** z natury (losowe $k$ → różne szyfrogramy dla tego samego $m$) |
| Rozmiar szyfrogramu | równy modułowi $n$ (ok. 1:1) | **podwójny** – para $(c_1,c_2)$ (ekspansja 2:1) |
| Koszt szyfrowania | tani (mały $e$, np. 65537) | dwa potęgowania (drogie) |
| Koszt deszyfrowania | drogie (potęgowanie $d$; przyspieszenie CRT) | jedno potęgowanie + odwrotność |
| Generowanie kluczy | **kosztowne** (szukanie dużych liczb pierwszych) | tańsze (wybór $x$; parametry $p,g$ mogą być standardowe) |
| Rozmiar klucza (112–128 b. bezp.) | 2048–3072 bitów | 2048–3072 bitów ($\mathbb{Z}_p^*$); na ECC (EC-ElGamal) – 224–256 bitów |
| Podpis cyfrowy | tak (ten sam klucz) | oddzielny algorytm (ElGamal signature, DSA) |
| Zależność od losowości | tylko przy generowaniu kluczy i paddingu | **krytyczna przy każdym szyfrowaniu/podpisie** (powtórzenie $k$) |
| Homomorfia | **mnożeniowa** | **mnożeniowa** (w wersji podręcznikowej): $(c_1c_1',c_2c_2')$ to szyfrogram $mm'$ |
| Plastyczność | tak (bez paddingu) | tak (**malleable**) – brak IND-CCA, wymaga wzmocnień (np. Cramer-Shoup, hybrydy) |
| Bezpieczeństwo semantyczne (IND-CPA) | tylko z paddingiem | tak, przy założeniu DDH |
| Zastosowania | TLS (historycznie), podpisy, PGP, certyfikaty | PGP/GPG, protokoły głosowania, szyfrowanie homomorficzne, podstawa DSA/ECDSA/ECIES |
| Kwanty | zagrożony (Shor) | zagrożony (Shor) |

## Co jest wspólne

- obie metody oparte na **arytmetyce modularnej w dużych liczbach** i trudnych problemach teorioliczbowych,
- **kryptografia klucza publicznego**: para kluczy, klucz publiczny jawny,
- wolne – używane do **kluczy sesyjnych** w układzie hybrydowym,
- homomorficzna własność mnożeniowa w wersji bez paddingu,
- potrzebne długie klucze (≥ 2048 bitów dla $\mathbb{Z}_p^*$ / modułu).

## Różnice, które warto podkreślić

1. **Źródło trudności:** RSA – rozkład na czynniki; ElGamal – logarytm dyskretny. Przełom w jednym nie musi oznaczać przełomu w drugim (choć algorytmy ogólnych sit mają podobną złożoność, a algorytm Shora łamie oba).
2. **Losowość:** ElGamal wymaga losowego $k$ **przy każdym szyfrowaniu**, RSA (z OAEP) tylko losowego paddingu.
3. **Ekspansja szyfrogramu:** ElGamal podwaja rozmiar.
4. **Elastyczność:** ElGamal można przenieść na dowolną grupę (**krzywe eliptyczne** – ECIES), co daje krótsze klucze; RSA wymaga struktury $n=pq$.

## Podsumowanie

- **RSA:** podstawa – **faktoryzacja** (problem RSA); szyfrowanie $c=m^e\bmod n$; podstawowa wersja deterministyczna.
- **ElGamal:** podstawa – **logarytm dyskretny** (DDH); szyfrowanie $(g^k,\ m\,y^k)$; **probabilistyczny**, szyfrogram 2× większy; podstawa DSA/ECDSA/ECIES.
- Oba: wolne, wymagają długich kluczy i paddingu/wzmocnień, **łamane przez Shora** – w praktyce zastępowane przez ECC, a w przyszłości przez algorytmy postkwantowe.
