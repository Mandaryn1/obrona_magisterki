# Wyjaśnij, czym zajmuje się kryptografia, a czym kryptoanaliza. Jakie są podstawowe cele stosowania mechanizmów kryptograficznych?

## Podstawowe pojęcia

- **Kryptologia** – nauka o bezpiecznej komunikacji; obejmuje **kryptografię** i **kryptoanalizę**.
- **Kryptografia** (gr. *kryptos* – ukryty, *grapho* – piszę) – dziedzina zajmująca się **projektowaniem i analizą metod zabezpieczania informacji**: algorytmów szyfrowania, funkcji skrótu, podpisów, protokołów. Jej celem jest ochrona informacji przed nieuprawnionymi osobami (przeciwnikami).
- **Kryptoanaliza** – dziedzina zajmująca się **łamaniem** (lub badaniem odporności) systemów kryptograficznych: odzyskiwaniem tekstu jawnego lub klucza **bez znajomości klucza** albo wykrywaniem słabości algorytmów i protokołów. Kryptografia „buduje", kryptoanaliza „atakuje" – wzajemnie się napędzają (bezpieczny algorytm to taki, który **wytrzymał lata kryptoanalizy**).

## Podstawowe cele mechanizmów kryptograficznych

| Cel | Pytanie | Mechanizm |
| :--- | :--- | :--- |
| **Poufność** (*confidentiality*) | czy tylko uprawnieni odczytają treść? | **szyfrowanie** (symetryczne, asymetryczne) |
| **Integralność** (*integrity*) | czy dane nie zostały zmienione? | **funkcje skrótu**, **MAC**, podpis cyfrowy |
| **Uwierzytelnianie** (*authentication*) | kto jest autorem/nadawcą? czy to na pewno on? | **MAC**, **podpis cyfrowy**, certyfikaty, protokoły uwierzytelniania |
| **Niezaprzeczalność** (*non-repudiation*) | czy nadawca może się wyprzeć wysłania? | **podpis cyfrowy** (klucz prywatny tylko u nadawcy) |

W języku inżynierii: **CIA** (Confidentiality, Integrity, Availability) + uwierzytelnianie i niezaprzeczalność (AAA).

**Typowy komplet w praktyce (np. TLS, szyfrowany e-mail):** szyfrowanie (poufność) + MAC/AEAD (integralność i autentyczność) + certyfikaty i podpisy (tożsamość) + nonce/numery sekwencyjne (świeżość).

## Podsumowanie

- **Kryptografia** – projektowanie metod ochrony informacji; **kryptoanaliza** – ich łamanie i ocena odporności; razem **kryptologia**.
- Cele: **poufność, integralność, uwierzytelnianie, niezaprzeczalność** (+ świeżość).
- Modele ataku: **COA, KPA, CPA, CCA**; metody: brute force, analiza statystyczna, kryptoanaliza różnicowa/liniowa, ataki urodzinowe, kanały boczne, ataki na protokoły.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Klasyfikacja_systemów_kryptograficznych.md)