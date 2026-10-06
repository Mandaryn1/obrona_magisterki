# Na czym polega główna idea kryptografii krzywych eliptycznych? Dlaczego jest ona atrakcyjna w porównaniu z klasycznymi systemami asymetrycznymi?

> **💬 Gotowa wypowiedź ustna:**
> *"Kryptografia krzywych eliptycznych opiera się na własnościach krzywej eliptycznej nad ciałem skończonym. Punkty tej krzywej można ze sobą dodawać, a wybierając punkt bazowy G, możemy go dodać do siebie k razy, otrzymując punkt k·G. Liczba k jest kluczem prywatnym, a punkt k·G kluczem publicznym. Bezpieczeństwo opiera się na problemie logarytmu dyskretnego na krzywej eliptycznej: obliczenie k·G jest łatwe, ale mając G i k·G, praktycznie nie da się odzyskać k. Na tej podstawie działają protokoły ECDH do uzgadniania kluczy oraz ECDSA i EdDSA do podpisów.
>
> Atrakcyjność ECC wynika przede wszystkim z krótszych kluczy przy tym samym poziomie bezpieczeństwa. Klucz 256-bitowy odpowiada w przybliżeniu RSA 3072-bitowemu. Dla problemu krzywych eliptycznych nie znamy algorytmów podwykładniczych, w przeciwieństwie do faktoryzacji, więc klucz nie musi być tak długi. Dzięki temu operacje są szybsze, a klucze i podpisy mniejsze, co oszczędza pamięć, energię i pasmo i sprawia, że ECC dobrze nadaje się do urządzeń mobilnych i IoT. Jest też standardem w TLS, SSH i certyfikatach, a popularne krzywe to P-256 i Curve25519.
>
> Wadą jest większa złożoność implementacji. Na przykład w ECDSA powtórzenie lub przewidywalność losowej wartości k ujawnia klucz prywatny. Trzeba też pamiętać, że tak jak RSA, ECC łamie algorytm Shora na komputerze kwantowym."*

**Kryptografia krzywych eliptycznych (ECC)** opiera się na własnościach krzywej eliptycznej nad ciałem skończonym, zadanej równaniem w rodzaju y² = x³ + ax + b (mod p). Punkty tej krzywej tworzą grupę, w której można **dodawać punkty**. Wybiera się punkt bazowy G i oblicza kQ = k·G, czyli dodaje G do siebie k razy. Liczba k jest kluczem prywatnym, a punkt k·G kluczem publicznym.

**Podstawa bezpieczeństwa to problem logarytmu dyskretnego na krzywej eliptycznej (ECDLP).** Mnożenie punktu przez liczbę jest łatwe i szybkie, ale mając G i k·G, praktycznie nie da się odzyskać k. Na tym opierają się protokoły **ECDH** (uzgadnianie klucza), **ECDSA i EdDSA** (podpisy) oraz **ECIES** (szyfrowanie).

**Dlaczego ECC jest atrakcyjna w porównaniu z RSA i klasycznym DH:**

- **Krótsze klucze przy tym samym bezpieczeństwie.** Klucz 256-bitowy ECC odpowiada w przybliżeniu RSA 3072-bitowemu. Dla ECDLP nie znamy algorytmów podwykładniczych, w przeciwieństwie do faktoryzacji (GNFS), więc klucz nie musi być tak długi.
- **Szybkość i mniejsze zużycie zasobów.** Operacje są szybsze, a podpisy i klucze mniejsze, co oszczędza pamięć, energię i pasmo. Dlatego ECC dobrze nadaje się do urządzeń mobilnych i IoT.
- **Szerokie zastosowanie.** Jest standardem w TLS (ECDHE), SSH, certyfikatach i kryptowalutach. Popularne krzywe to P-256 i Curve25519.

**Wady:** implementacja jest bardziej złożona i łatwiej o błąd. Na przykład w ECDSA powtórzenie lub przewidywalność losowej wartości k ujawnia klucz prywatny (tak było w ataku na PlayStation 3). Ważny jest też wybór bezpiecznej krzywej.

**Uwaga:** tak samo jak RSA, ECC jest podatna na **algorytm Shora** na komputerze kwantowym.

## Podsumowanie

- **ECC** korzysta z grupy punktów krzywej eliptycznej; mnożenie punktu przez skalar $Q=kP$ jest łatwe, a odwrócenie (**ECDLP**) – trudne (atak wykładniczy).
- Daje te same funkcje co RSA/DH (**ECDH, ECDSA/EdDSA, ECIES**) przy **znacznie krótszych kluczach** (256 b. ≈ RSA 3072), szybkości i mniejszym zużyciu zasobów – idealna dla IoT, urządzeń mobilnych, TLS.
- Wady: złożoność implementacji, kwestie zaufania do krzywych, wrażliwość na losowość ECDSA; **nie jest odporna na komputery kwantowe**.

---
[⬅️ Poprzedni temat](13_Kody_MAC_i_porównanie_z_podpisem_cyfrowym.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](15_Komputery_kwantowe_a_kryptografia_kwantowa_i_postkwantowa.md)