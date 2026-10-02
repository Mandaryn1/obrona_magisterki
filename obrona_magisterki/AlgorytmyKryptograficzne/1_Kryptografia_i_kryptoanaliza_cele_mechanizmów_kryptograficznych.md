# Kryptografia i kryptoanaliza. Cele stosowania mechanizmów kryptograficznych

## Podstawowe pojęcia

- **Kryptologia** – nauka o bezpiecznej komunikacji; obejmuje **kryptografię** i **kryptoanalizę**.
- **Kryptografia** (gr. *kryptos* – ukryty, *grapho* – piszę) – dziedzina zajmująca się **projektowaniem i analizą metod zabezpieczania informacji**: algorytmów szyfrowania, funkcji skrótu, podpisów, protokołów. Jej celem jest ochrona informacji przed nieuprawnionymi osobami (przeciwnikami).
- **Kryptoanaliza** – dziedzina zajmująca się **łamaniem** (lub badaniem odporności) systemów kryptograficznych: odzyskiwaniem tekstu jawnego lub klucza **bez znajomości klucza** albo wykrywaniem słabości algorytmów i protokołów. Kryptografia „buduje", kryptoanaliza „atakuje" – wzajemnie się napędzają (bezpieczny algorytm to taki, który **wytrzymał lata kryptoanalizy**).
- **Steganografia** – ukrywanie *faktu istnienia* wiadomości (np. w obrazie); odróżnić od kryptografii, która ukrywa *treść*.

## Podstawowe cele mechanizmów kryptograficznych

| Cel | Pytanie | Mechanizm |
| :--- | :--- | :--- |
| **Poufność** (*confidentiality*) | czy tylko uprawnieni odczytają treść? | **szyfrowanie** (symetryczne, asymetryczne) |
| **Integralność** (*integrity*) | czy dane nie zostały zmienione? | **funkcje skrótu**, **MAC**, podpis cyfrowy |
| **Uwierzytelnianie** (*authentication*) | kto jest autorem/nadawcą? czy to na pewno on? | **MAC**, **podpis cyfrowy**, certyfikaty, protokoły uwierzytelniania |
| **Niezaprzeczalność** (*non-repudiation*) | czy nadawca może się wyprzeć wysłania? | **podpis cyfrowy** (klucz prywatny tylko u nadawcy) |
| *(uzupełnienie)* **Świeżość** (*freshness*) | czy to nie powtórka starego komunikatu? | znaczniki czasu, **nonce**, liczniki (ochrona przed *replay*) |
| *(uzupełnienie)* **Dostępność** | czy usługa działa? | pośrednio (ochrona przed DoS, nie główny cel kryptografii) |

W języku inżynierii: **CIA** (Confidentiality, Integrity, Availability) + uwierzytelnianie i niezaprzeczalność (AAA).

**Typowy komplet w praktyce (np. TLS, szyfrowany e-mail):** szyfrowanie (poufność) + MAC/AEAD (integralność i autentyczność) + certyfikaty i podpisy (tożsamość) + nonce/numery sekwencyjne (świeżość).

## Model komunikacji

```
 Alicja ──(m)──▶ E_k ──(c)───── kanał niezabezpieczony ─────▶ D_k ──(m)──▶ Bob
                                       ▲
                                       │ podsłuch / modyfikacja
                                    Ewa (przeciwnik), Mallory (aktywny)
```

Przeciwnik **pasywny** (podsłuchuje) i **aktywny** (modyfikuje, wstrzykuje, przechwytuje i podszywa się – MITM).

## Kryptoanaliza – rodzaje ataków

### Ze względu na wiedzę przeciwnika (model ataku)

| Atak | Co ma przeciwnik | Cel |
| :--- | :--- | :--- |
| **Tylko szyfrogram** (ciphertext-only, COA) | tylko szyfrogramy | odtworzyć tekst/klucz |
| **Znany tekst jawny** (known-plaintext, KPA) | pary (tekst jawny, szyfrogram) | odtworzyć klucz / odszyfrować inne |
| **Wybrany tekst jawny** (chosen-plaintext, CPA) | może szyfrować dowolne teksty | j.w. (np. dostęp do „wyroczni" szyfrującej) |
| **Wybrany szyfrogram** (chosen-ciphertext, CCA) | może deszyfrować wybrane szyfrogramy | j.w. (np. *padding oracle*) |

Współczesne szyfry projektuje się jako odporne co najmniej na **CPA**, a najlepiej **CCA** (zob. temat 5).

### Metody ataku

- **brute force** (wyczerpujące przeszukiwanie przestrzeni kluczy; koszt $2^{n}$ dla klucza $n$-bitowego),
- **analiza częstości** liter (szyfry klasyczne), test Kasiskiego (Vigenère),
- **kryptoanaliza różnicowa i liniowa** (szyfry blokowe),
- **atak urodzinowy** (kolizje funkcji skrótu, $\approx2^{n/2}$),
- **atak słownikowy / tablice tęczowe** (hasła),
- **ataki na implementację – kanały boczne** (*side-channel*): pomiar czasu, poboru mocy, cache, błędy (*fault injection*) – łamią implementację, nie algorytm,
- **ataki na protokoły**: MITM, *replay*, *downgrade*,
- **inżynieria społeczna** (najsłabszym ogniwem bywa człowiek).

### Ocena odporności

Algorytm jest „złamany", jeśli istnieje atak **szybszy niż brute force** (nawet jeśli nadal niepraktyczny). Bezpieczeństwo mierzy się w bitach i przez **lata publicznej analizy** (np. konkurs AES, SHA-3).

## Przykład historyczny

**Enigma** (II wojna światowa) – elektromechaniczna maszyna szyfrująca; złamana dzięki pracy polskich kryptoanalityków (**Rejewski, Różycki, Zygalski**; Biuro Szyfrów) i później Bletchley Park (Turing). Przykład: słabość = **błędy operatorów** i znany tekst (tzw. *cribs*), a nie sam algorytm.

## Kryptografia a pokrewne dziedziny

- **Bezpieczeństwo informacji** szersze: kryptografia to narzędzie; potrzebne też kontrola dostępu, zarządzanie kluczami, procedury.
- **Kryptografia stosowana**: TLS/HTTPS, VPN, SSH, szyfrowanie dysków, podpis elektroniczny, kryptowaluty, komunikatory (Signal).

## Podsumowanie

- **Kryptografia** – projektowanie metod ochrony informacji; **kryptoanaliza** – ich łamanie i ocena odporności; razem **kryptologia**.
- Cele: **poufność, integralność, uwierzytelnianie, niezaprzeczalność** (+ świeżość).
- Modele ataku: **COA, KPA, CPA, CCA**; metody: brute force, analiza statystyczna, kryptoanaliza różnicowa/liniowa, ataki urodzinowe, kanały boczne, ataki na protokoły.
