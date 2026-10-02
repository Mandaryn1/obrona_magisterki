# Szyfry klasyczne (podstawieniowe i przestawieniowe) a nowoczesne algorytmy kryptograficzne

## Szyfry klasyczne

### Szyfry podstawieniowe (substytucyjne)

Zamiana każdego znaku (lub grupy znaków) tekstu jawnego na inny znak zgodnie z regułą.

| Szyfr | Zasada | Przykład |
| :--- | :--- | :--- |
| **Cezara** (przesunięcie) | przesunięcie liter o stałe $k$: $c=(m+k)\bmod26$ | ATTACK, $k=3$ → **DWWDFN**; klucz: 25 możliwości |
| **monoalfabetyczny** (dowolne podstawienie) | permutacja alfabetu; klucz: $26!\approx4\cdot10^{26}$ ($\approx2^{88}$) | łamany **analizą częstości** liter |
| **polialfabetyczny – Vigenère** | kilka alfabetów Cezara wg słowa kluczowego | ATTACKATDAWN, klucz LEMON → **LXFOPVEFRNHR** |
| **Playfair** | podstawienie par liter (digramy) w tablicy 5×5 | łamany analizą digramów |
| **Hill** | mnożenie bloku liter przez macierz klucza mod 26 | wrażliwy na atak ze znanym tekstem (liniowy) |
| **Enigma** | wirnikowe, polialfabetyczne, zmienne podstawienie | złamana przez Rejewskiego i in. |

### Szyfry przestawieniowe (transpozycyjne)

Litery tekstu jawnego **zostają te same, zmienia się ich kolejność**.

- **płotowy (rail fence)** – zapis „zygzakiem" na $n$ liniach i odczyt wierszami,
- **kolumnowy** – tekst w tabeli, kolumny odczytywane w kolejności wg klucza,
- **skytale** (Sparta).

### Wspólne słabości szyfrów klasycznych

| Słabość | Opis |
| :--- | :--- |
| **zachowanie statystyk języka** | podstawienie monoalfabetyczne zachowuje częstość liter → **analiza częstości** (Al-Kindi, IX w.); transpozycja zachowuje częstości liter |
| **mały lub słaby klucz** | Cezar – 25 prób; Vigenère – okresowość klucza (**test Kasiskiego**, indeks koincydencji) |
| **liniowość** | Hill – wystarcza znany tekst jawny (układ równań liniowych) |
| **brak dyfuzji** | zmiana jednej litery wpływa na jedną literę szyfrogramu |
| **brak formalnych definicji** i dowodów | bezpieczeństwo „na wiarę", ocena ad hoc |
| **zależność od tajności metody** | częściowo opierały się na ukrywaniu algorytmu (por. zasada Kerckhoffsa) |
| **ręczne/mechaniczne wykonanie** | ograniczona złożoność |

**Przykład łamania Cezara:** szyfrogram `DWWDFN`; próba $k=1..25$ → dla $k=3$ pojawia się sensowne słowo „ATTACK".

## Nowoczesne algorytmy kryptograficzne

Opracowane od lat 70. XX w. (DES 1977, RSA 1977, DH 1976, AES 2001). Cechy:

- **dowolne dane binarne** (nie tylko litery), operacje na **bitach/bajtach**, implementowane w **komputerach**,
- **duże przestrzenie kluczy** (128–256 bitów symetryczne),
- **konstrukcje wielorundowe** łączące operacje nieliniowe (S-boksy), permutacje i mieszanie klucza (sieci SPN i Feistela),
- **zasady projektowe Shannona: dezorientacja (confusion)** – złożona, nieliniowa zależność szyfrogramu od klucza (S-boksy) i **dyfuzja (diffusion)** – zmiana jednego bitu tekstu/klucza zmienia ok. połowę bitów wyjścia (**efekt lawinowy**),
- **kryptografia asymetryczna** i funkcje skrótu, podpisy, MAC – funkcjonalności nieznane szyfrom klasycznym,
- **opierają się na trudnych problemach obliczeniowych** (faktoryzacja, logarytm dyskretny, krzywe eliptyczne, kraty),
- **publiczne standardy** i konkursy (AES, SHA-3, PQC).

## Jak zmieniło się podejście do bezpieczeństwa

| Aspekt | Szyfry klasyczne | Nowoczesne |
| :--- | :--- | :--- |
| **Podstawa bezpieczeństwa** | tajność metody i „sprytu", ad hoc | **tajność klucza** + publiczny algorytm (**zasada Kerckhoffsa**) |
| **Ocena** | brak miary; „nikt nie złamał" | **poziom bezpieczeństwa w bitach**; odporność na znane klasy ataków (różnicowa, liniowa) |
| **Model przeciwnika** | zwykle tylko szyfrogram | **COA, KPA, CPA, CCA** – przeciwnik ma dostęp do wielu par i wyroczni |
| **Cel** | ukrycie treści | cele złożone: poufność, integralność, uwierzytelnianie, niezaprzeczalność (AEAD) |
| **Definicje i dowody** | brak | **ścisłe definicje** (IND-CPA, IND-CCA, EUF-CMA) i **dowody redukcyjne** do trudnych problemów |
| **Rodzaj bezpieczeństwa** | intuicyjne | **obliczeniowe** (przeciwnik o ograniczonych zasobach) lub teorioinformacyjne (OTP) |
| **Weryfikacja** | przez tajność | **publiczna kryptoanaliza**, konkursy, standaryzacja (NIST), długie lata analiz |
| **Klucz** | krótki, często słowo | losowy, długi, zarządzany (KMS, HSM, rotacja) |
| **Dyfuzja i nieliniowość** | brak | zaprojektowane świadomie |
| **Implementacja** | ręczna | oprogramowanie/sprzęt; problemy: **kanały boczne**, błędy implementacji |
| **Dystrybucja kluczy** | spotkanie/kurier | **kryptografia klucza publicznego**, PKI |
| **Przeciwnik** | człowiek z ołówkiem | komputery, klastry, GPU, ASIC (a w przyszłości kwantowe) |

### Nowe definicje bezpieczeństwa

- **IND-CPA** – atakujący nie odróżni szyfrogramów dwóch wybranych wiadomości (nawet mając wyrocznię szyfrującą) – wymaga **szyfrowania probabilistycznego**,
- **IND-CCA2** – j.w. z wyrocznią deszyfrującą (odporność na modyfikacje),
- **EUF-CMA** (podpisy) – nie da się sfałszować podpisu nawet po obejrzeniu wielu podpisanych wiadomości,
- **AEAD** – szyfrowanie uwierzytelnione jako standardowy cel.

### Czego uczą się z porażek klasycznych

- szyfr bezpieczny przy ataku COA nie musi być bezpieczny przy KPA/CPA,
- **nigdy nie polegać na „sprytnych" transformacjach**; bezpieczeństwo musi wynikać z dobrze zbadanych konstrukcji,
- nowoczesna kryptografia nie jest niezniszczalna – **MD5, SHA-1, DES, RC4** zostały złamane lub osłabione; stąd **kryptoagilność** i okresowe przeglądy.

## Porównanie przykładowe: Vigenère a AES

| | Vigenère (klasyczny) | AES (nowoczesny) |
| :--- | :--- | :--- |
| Alfabet | 26 liter | bajty/bity (blok 128 bit) |
| Klucz | słowo (np. 5 liter) | 128/192/256 bitów |
| Przestrzeń kluczy | $26^5\approx1{,}2\cdot10^7$ | $2^{128}\approx3{,}4\cdot10^{38}$ |
| Łamanie | test Kasiskiego + analiza częstości, minuty | brak praktycznego ataku lepszego niż brute force |
| Rundy | 1 | 10/12/14 (SubBytes, ShiftRows, MixColumns, AddRoundKey) |
| Dyfuzja | brak | pełna po kilku rundach |

## Podsumowanie

- **Klasyczne:** podstawieniowe (Cezar, Vigenère, Playfair, Hill) i przestawieniowe (płotowy, kolumnowy); łamane analizą statystyczną, małe lub okresowe klucze, brak formalnego uzasadnienia.
- **Nowoczesne:** operacje na bitach, duże klucze, S-boksy i permutacje (dezorientacja i dyfuzja), kryptografia asymetryczna, skróty, podpisy.
- **Zmiana podejścia:** od tajności metody do **tajności klucza** (Kerckhoffs), od intuicji do **formalnych definicji i dowodów**, od COA do **CPA/CCA**, od ukrywania do **publicznej weryfikacji** i bezpieczeństwa mierzonego w bitach.
