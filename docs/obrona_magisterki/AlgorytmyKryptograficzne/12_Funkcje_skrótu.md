# Wyjaśnij, czym są funkcje skrótu. Jakie cechy powinna mieć bezpieczna kryptograficzna funkcja skrótu?

**Funkcja skrótu (hashująca)** to funkcja, która przyjmuje dane o dowolnej długości i zwraca z nich ciąg o stałej długości, czyli skrót (odcisk). Na przykład SHA-256 zawsze daje 256 bitów, niezależnie od tego, czy skrócimy jedno zdanie, czy cały plik. Skrót służy jako cyfrowy „odcisk palca" danych. Używa się go do sprawdzania integralności plików, w podpisach cyfrowych, do przechowywania haseł i w kodach MAC (HMAC).

**Cechy bezpiecznej kryptograficznej funkcji skrótu:**

- **Deterministyczność i szybkość:** te same dane zawsze dają ten sam skrót, a jego obliczenie jest szybkie.
- **Efekt lawinowy:** zmiana jednego bitu na wejściu zmienia mniej więcej połowę bitów skrótu, więc podobne dane dają zupełnie różne skróty.
- **Odporność na odwracanie (jednokierunkowość):** mając skrót, nie da się praktycznie odtworzyć danych. Wymaga to ok. 2ⁿ operacji.
- **Odporność na drugi przeciwobraz:** mając dane m₁, nie da się znaleźć innych danych m₂ o tym samym skrócie. To też ok. 2ⁿ operacji.
- **Odporność na kolizje:** nie da się znaleźć dwóch różnych danych o tym samym skrócie. Z powodu **ataku urodzinowego** wymaga to tylko ok. 2ⁿ/² operacji, dlatego skrót musi być odpowiednio długi.

Kolizje istnieją zawsze, bo wejść jest nieskończenie wiele, a skrótów skończenie wiele. Chodzi o to, żeby nie dało się ich praktycznie znaleźć.

## Podsumowanie

- **Funkcja skrótu** zamienia dowolne dane na skrót stałej długości; **deterministyczna, szybka, efekt lawinowy**.
- Bezpieczeństwo = **odporność na odwracanie ($2^n$), na drugi przeciwobraz ($2^n$) i na kolizje ($2^{n/2}$, atak urodzinowy)**.
- Aktualnie: **SHA-256/384/512, SHA-3, BLAKE2/3**; **MD5 i SHA-1 złamane**.
- Zastosowania: integralność, podpisy, HMAC, hasła (z solą i KDF), blockchain, zobowiązania.

---
[⬅️ Poprzedni temat](11_Podpis_elektroniczny.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](13_Kody_MAC_i_porównanie_z_podpisem_cyfrowym.md)