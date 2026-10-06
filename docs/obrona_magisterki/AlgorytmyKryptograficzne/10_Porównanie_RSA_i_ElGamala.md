# Porównaj kryptosystemy RSA i Elgamala. Jakie problemy matematyczne leżą u podstaw ich bezpieczeństwa?

> **💬 Gotowa wypowiedź ustna:**
> *"RSA i ElGamal to dwa kryptosystemy z kluczem publicznym, ale oparte na różnych problemach matematycznych. Bezpieczeństwo RSA opiera się na trudności faktoryzacji, czyli rozkładu dużej liczby na czynniki pierwsze. Klucz publiczny zawiera iloczyn n dwóch dużych liczb pierwszych, a klucz prywatny można obliczyć tylko znając te liczby. Mnożenie jest łatwe, natomiast odwrócenie go dla odpowiednio dużych liczb jest praktycznie niewykonalne.
>
> Bezpieczeństwo ElGamala opiera się na problemie logarytmu dyskretnego. Potęgowanie modulo liczba pierwsza jest szybkie, ale mając wynik, bardzo trudno ustalić wykładnik, do którego podniesiono podstawę. Ten sam problem leży u podstaw protokołu Diffiego-Hellmana. Dla obu problemów nie znamy szybkiego algorytmu na zwykłych komputerach, ale oba łamie algorytm Shora na komputerze kwantowym.
>
> Różnice są następujące. Po pierwsze, czysty RSA jest deterministyczny, czyli ta sama wiadomość zawsze daje ten sam szyfrogram, dlatego w praktyce stosuje się wypełnienie OAEP. ElGamal jest probabilistyczny: przy każdym szyfrowaniu losuje nową wartość k, więc ten sam tekst daje za każdym razem inny szyfrogram. Po drugie, szyfrogram RSA ma rozmiar zbliżony do n, a w ElGamalu jest dwa razy dłuższy, bo składa się z pary liczb. Po trzecie, w ElGamalu losowe k musi być tajne i nigdy się nie powtarzać, bo jego powtórzenie pozwala odzyskać wiadomość. Po czwarte, RSA służy zarówno do szyfrowania, jak i do podpisów, a z ElGamala wywodzi się rodzina podpisów, w tym DSA.
>
> Podsumowując, oba systemy są bezpieczne tak długo, jak trudny pozostaje odpowiedni problem: faktoryzacja dla RSA i logarytm dyskretny dla ElGamala."*

RSA i ElGamal to dwa kryptosystemy z kluczem publicznym, ale oparte na różnych problemach matematycznych.

**RSA** opiera swoje bezpieczeństwo na **trudności faktoryzacji dużych liczb**. Klucz publiczny zawiera n = p·q, czyli iloczyn dwóch dużych liczb pierwszych. Każdy może go znać, ale żeby wyliczyć klucz prywatny, trzeba rozłożyć n na czynniki, a to jest praktycznie niewykonalne dla dużych liczb. Szyfrowanie polega na potęgowaniu modulo n, a deszyfrowanie na potęgowaniu kluczem prywatnym.

**ElGamal** opiera się na **problemie logarytmu dyskretnego**. Znając g, p oraz gˣ mod p, bardzo trudno jest wyznaczyć x. Tę samą trudność wykorzystuje protokół Diffiego-Hellmana, a ElGamal można traktować jako jego rozszerzenie do szyfrowania.

**Różnice między nimi:**

- **Losowość.** Czysty RSA jest deterministyczny, więc ten sam tekst jawny daje ten sam szyfrogram. Dlatego w praktyce stosuje się wypełnienie OAEP. ElGamal jest probabilistyczny: przy każdym szyfrowaniu losuje nowe k, więc ten sam tekst daje za każdym razem inny szyfrogram.
- **Rozmiar szyfrogramu.** W RSA ma on rozmiar zbliżony do n. W ElGamalu jest dwa razy dłuższy, bo składa się z pary (c₁, c₂).
- **Losowe k.** W ElGamalu każde szyfrowanie wymaga nowego, tajnego i nigdy niepowtarzanego k. Jeśli się powtórzy, można odzyskać wiadomość.
- **Zastosowania.** RSA służy do szyfrowania i podpisów. ElGamal służy do szyfrowania, a jego wariant stał się podstawą podpisów DSA.

## Podsumowanie

- **RSA:** podstawa – **faktoryzacja** (problem RSA); szyfrowanie $c=m^e\bmod n$; podstawowa wersja deterministyczna.
- **ElGamal:** podstawa – **logarytm dyskretny** (DDH); szyfrowanie $(g^k,\ m\,y^k)$; **probabilistyczny**, szyfrogram 2× większy; podstawa DSA/ECDSA/ECIES.
- Oba: wolne, wymagają długich kluczy i paddingu/wzmocnień, **łamane przez Shora** – w praktyce zastępowane przez ECC, a w przyszłości przez algorytmy postkwantowe.

---
[⬅️ Poprzedni temat](9_Bezpieczeństwo_kryptosystemu_RSA.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](11_Podpis_elektroniczny.md)