# Czym jest podpis elektroniczny i jakie własności bezpieczeństwa powinien zapewniać? Wyjaśnij różnicę między szyfrowaniem a podpisywaniem wiadomości.

Podpis elektroniczny to mechanizm kryptograficzny, który pozwala potwierdzić, **kto jest autorem wiadomości** i że **nie została zmieniona** po podpisaniu. Działa na zasadzie kryptografii klucza publicznego. Nadawca liczy skrót wiadomości (np. SHA-256) i podpisuje go swoim **kluczem prywatnym**. Odbiorca weryfikuje podpis **kluczem publicznym** nadawcy i porównuje go ze skrótem, który sam wylicza z otrzymanej wiadomości.

**Własności bezpieczeństwa, które powinien zapewniać:**

- **Autentyczność:** wiadomość pochodzi od konkretnej osoby, bo tylko ona zna klucz prywatny.
- **Integralność:** zmiana choćby jednego bitu sprawia, że weryfikacja się nie powiedzie.
- **Niezaprzeczalność:** nadawca nie może się wyprzeć, że podpisał, bo nikt inny nie ma jego klucza prywatnego.

Podpis **nie zapewnia poufności**, bo sama treść zwykle pozostaje jawna.

**Różnica między szyfrowaniem a podpisywaniem:**

- **Cel:** szyfrowanie chroni **poufność** (nikt niepowołany nie przeczyta treści), a podpis chroni **autentyczność, integralność i niezaprzeczalność**.
- **Kierunek kluczy:** przy szyfrowaniu nadawca używa **klucza publicznego odbiorcy**, a odbiorca odszyfrowuje swoim kluczem prywatnym. Przy podpisywaniu nadawca używa **własnego klucza prywatnego**, a każdy może zweryfikować podpis jego kluczem publicznym.
- **Kto może wykonać operację:** szyfrować może każdy, a odszyfrować tylko odbiorca. Podpisać może tylko autor, a zweryfikować każdy.

W praktyce stosuje się obie operacje razem, gdy ważna jest poufność i autentyczność.

**Dodatkowo:** prawdziwość klucza publicznego potwierdza **certyfikat** wydany przez zaufane CA (PKI). W prawie unijnym najsilniejszy jest **kwalifikowany podpis elektroniczny**, który ma skutek równoważny podpisowi własnoręcznemu.

## Podsumowanie

- **Podpis cyfrowy** = skrót dokumentu zaszyfrowany kluczem prywatnym (w ujęciu RSA) i weryfikowalny kluczem publicznym; zapewnia **autentyczność, integralność i niezaprzeczalność**, nie poufność.
- **Szyfrowanie** – kluczem publicznym **odbiorcy**, cel: **poufność**; **podpis** – kluczem prywatnym **nadawcy**, cel: **uwierzytelnienie i niezaprzeczalność**.
- Praktyka: podpis skrótu (RSA-PSS, ECDSA, Ed25519), certyfikaty (PKI), znakowanie czasem; prawnie – kwalifikowany podpis elektroniczny.

---
[⬅️ Poprzedni temat](10_Porównanie_RSA_i_ElGamala.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](12_Funkcje_skrótu.md)