# Na czym opiera się bezpieczeństwo kryptosystemu RSA? Jaką rolę odgrywają w nim liczby pierwsze, arytmetyka modularna i trudność faktoryzacji?

> **💬 Gotowa wypowiedź ustna:**
> *"Bezpieczeństwo kryptosystemu RSA opiera się na trudności faktoryzacji, czyli rozkładu dużej liczby na czynniki pierwsze. Pomnożenie dwóch dużych liczb pierwszych jest łatwe, natomiast mając tylko wynik, bardzo trudno ustalić, z jakich liczb powstał.
>
> Liczby pierwsze odgrywają tu kluczową rolę. Wybieramy dwie duże, tajne liczby pierwsze, p i q, a ich iloczyn n jest jawny i wchodzi do klucza publicznego. Klucz prywatny można obliczyć tylko wtedy, gdy zna się p i q, więc atakujący musiałby rozłożyć n na czynniki.
>
> Drugim elementem jest arytmetyka modularna. Szyfrowanie i deszyfrowanie to potęgowanie modulo n, które jest szybkie do wykonania, ale praktycznie nieodwracalne bez klucza. Odpowiednio dobrane klucze, publiczny i prywatny, są wzajemnie odwrotne, dzięki czemu deszyfrowanie odtwarza oryginalną wiadomość. Na przykład dla p równego 61 i q równego 53 liczba 65 po zaszyfrowaniu daje 2790, a po odszyfrowaniu znowu 65.
>
> W praktyce stosuje się klucze co najmniej 2048-bitowe oraz wypełnienie OAEP, ponieważ czysty RSA jest deterministyczny. Trzeba też pamiętać, że RSA łamie algorytm Shora na komputerze kwantowym."*

# 9. Bezpieczeństwo kryptosystemu RSA

**RSA** to kryptosystem z kluczem publicznym. Każdy może zaszyfrować wiadomość kluczem publicznym, ale odszyfrować ją potrafi tylko właściciel klucza prywatnego.

## Na czym opiera się bezpieczeństwo

Na **trudności rozkładu dużej liczby na czynniki pierwsze (faktoryzacji)**. Pomnożenie dwóch dużych liczb pierwszych jest łatwe, ale mając tylko wynik, bardzo trudno odkryć, z jakich liczb powstał. Klucz prywatny można wyliczyć tylko wtedy, gdy zna się te dwie liczby pierwsze.

## Rola liczb pierwszych

- Wybieramy dwie duże, tajne liczby pierwsze, p i q (dziś po ok. 1024 bity każda).
- Ich iloczyn n = p·q jest jawny i wchodzi do klucza publicznego.
- Znajomość p i q pozwala obliczyć tajny składnik klucza prywatnego. Bez nich atakujący musiałby rozłożyć n na czynniki.

## Rola arytmetyki modularnej

- Wszystkie obliczenia robimy „na zegarze" o rozmiarze n, czyli liczymy tylko reszty z dzielenia przez n.
- Szyfrowanie i deszyfrowanie to **potęgowanie modulo n**. Jest szybkie, ale odwrócenie go bez klucza jest praktycznie niewykonalne.
- Działa to dzięki twierdzeniu Eulera: odpowiednio dobrane klucze (publiczny e i prywatny d) są wzajemnie odwrotne, więc deszyfrowanie „cofa" szyfrowanie.

## Jak to działa w skrócie

1. Wybieramy p i q, liczymy n = p·q.
2. Wybieramy liczbę publiczną e i wyliczamy z niej tajną d, do czego potrzebna jest znajomość p i q.
3. Klucz publiczny to (n, e), a prywatny to d.
4. **Szyfrowanie:** szyfrogram = wiadomość podniesiona do potęgi e (modulo n).
5. **Deszyfrowanie:** szyfrogram podniesiony do potęgi d (modulo n) daje z powrotem wiadomość.

## Przykład z małymi liczbami

p = 61, q = 53, więc n = 3233, e = 17, d = 2753. Wiadomość 65 po zaszyfrowaniu daje 2790, a po odszyfrowaniu znowu 65. W prawdziwym RSA liczby mają ponad 2000 bitów, więc rozkład n jest niewykonalny.

## W jednym zdaniu

Bezpieczeństwo RSA wynika z tego, że mnożenie liczb pierwszych jest łatwe, a rozkład wyniku na czynniki jest bardzo trudny.

Przy kolejnych tematach będę zaczynał notatkę od takiego bloku „Gotowa wypowiedź ustna": ciągły tekst do powiedzenia, bez wypunktowań i bez numerowanych instrukcji.

## Podsumowanie

- **RSA:** $n=pq$, $\varphi(n)=(p-1)(q-1)$, $ed\equiv1\bmod\varphi(n)$; $c=m^e\bmod n$, $m=c^d\bmod n$.
- **Liczby pierwsze** tworzą tajną zapadkę; **arytmetyka modularna** (twierdzenie Eulera, szybkie potęgowanie, odwrotność modularna) zapewnia poprawność i wydajność; bezpieczeństwo = **trudność faktoryzacji** (problem RSA).
- Wymaga kluczy ≥ 2048 bitów, losowych liczb pierwszych i **paddingu (OAEP/PSS)**; zagrożony przez komputery kwantowe (Shor).
- Komputery kwantowe (algorytm Shora) rozkładają liczby na czynniki szybko, więc RSA przestanie być bezpieczny. Dlatego powstają algorytmy postkwantowe.

---

[⬅️ Poprzedni temat](8_Protokół_Diffiego-Hellmana.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Porównanie_RSA_i_ElGamala.md)
