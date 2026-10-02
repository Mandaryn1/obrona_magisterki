# Wyzwania dla kryptografii związane z komputerami kwantowymi. Kryptografia kwantowa a postkwantowa

## Komputer kwantowy – skrót

Komputer kwantowy wykorzystuje **kubity** (superpozycja, splątanie, interferencja). Dla niektórych problemów oferuje **przyspieszenie** względem komputerów klasycznych; nie jest „szybszy we wszystkim". Istotne dla kryptografii są dwa algorytmy:

## Zagrożenia dla obecnej kryptografii

### 1. Algorytm Shora (1994)

Rozwiązuje w **czasie wielomianowym** (wykładnicze przyspieszenie): **faktoryzację liczb całkowitych** oraz **logarytm dyskretny** (także na krzywych eliptycznych). Konsekwencje:

| Algorytm | Podstawa | Skutek |
| :--- | :--- | :--- |
| **RSA** | faktoryzacja | **złamany** |
| **DH, ElGamal, DSA** | logarytm dyskretny | **złamane** |
| **ECC (ECDH, ECDSA, EdDSA)** | ECDLP | **złamane** (krótsze klucze → mniejszy komputer kwantowy wystarcza) |

Zwiększanie długości klucza **nie ratuje** tych algorytmów (przyspieszenie wykładnicze), więc potrzebna jest **zmiana algorytmów** (kryptografia postkwantowa).

### 2. Algorytm Grovera (1996)

**Kwadratowe** przyspieszenie przeszukiwania: $2^n$ → $2^{n/2}$ operacji.

| Mechanizm | Skutek | Środek zaradczy |
| :--- | :--- | :--- |
| szyfry symetryczne (AES) | efektywna siła klucza **połowiona**: AES-128 → ok. 64 bity, AES-256 → ok. 128 bitów | **AES-256** (klucze 256 bitów) |
| funkcje skrótu | wyszukiwanie przeciwobrazu $2^{n/2}$ (kolizje – ok. $2^{n/3}$) | skróty ≥ 256 bitów (SHA-256, **SHA-384/512**, SHA-3) |
| MAC (HMAC) | j.w. | klucze ≥ 256 bitów |

Wniosek: **kryptografia symetryczna i skróty nie są zagrożone w sposób fundamentalny** – wystarczy podwoić parametry.

### 3. Konsekwencje praktyczne

- **Harvest now, decrypt later (HNDL)** – przeciwnik zbiera dziś zaszyfrowany ruch, by odszyfrować go po zbudowaniu komputera kwantowego. Dotyczy danych o długim czasie wrażliwości (tajemnice państwowe, medyczne, handlowe) → **pilna potrzeba wymiany uzgadniania kluczy (KEM)**.
- **Podpisy cyfrowe, certyfikaty, firmware, PKI** – potrzebna migracja (ryzyko podrabiania; „trust now, forge later" mniej pilne, ale długie cykle życia urządzeń i certyfikatów).
- **Skala migracji:** TLS, VPN, SSH, PKI, karty, IoT, blockchain – **kryptoagilność** (możliwość zmiany algorytmów) i **inwentaryzacja** używanej kryptografii (CBOM).
- **Kiedy?** Komputer kwantowy zdolny złamać RSA-2048 (rząd milionów fizycznych kubitów z korekcją błędów; szacunki zasobów systematycznie maleją) **jeszcze nie istnieje**, ale terminy są niepewne; dlatego migracja zaczyna się **wcześniej** (modele Mosca: czas wrażliwości danych + czas migracji > czas do powstania komputera kwantowego).

## Kryptografia kwantowa

**Kryptografia kwantowa** wykorzystuje **prawa fizyki kwantowej** do zapewnienia bezpieczeństwa. Najważniejsze zastosowanie: **kwantowa dystrybucja klucza (QKD)** – uzgadnianie tajnego klucza przez przesyłanie pojedynczych fotonów.

### Zasada (protokół BB84, Bennett i Brassard 1984)

- Alicja koduje bity w **stanach polaryzacji fotonów**, losowo wybierając jedną z dwóch baz (np. prostoliniową ⊕ lub ukośną ⊗); Bob mierzy w losowo wybranych bazach.
- Po transmisji strony publicznie porównują **bazy** (nie wyniki) i zachowują bity z zgodnych baz (**przesiewanie, sifting**).
- **Podsłuch jest wykrywalny**: pomiar kwantowy **zaburza stan** (**twierdzenie o zakazie klonowania**) – Ewa wprowadza błędy; strony szacują **QBER** (stopę błędów) i przy zbyt wysokim przerywają.
- Dalsze kroki: **korekcja błędów** i **wzmocnienie prywatności** (privacy amplification) → tajny klucz, który następnie służy np. do OTP lub AES.

**Bezpieczeństwo QKD** wynika z praw fizyki (bezwarunkowe, **teorio-informacyjne**), nie z założeń o trudności problemów obliczeniowych.

### Ograniczenia QKD

- wymaga **uwierzytelnionego kanału klasycznego** (podpis lub MAC z wstępnie uzgodnionym kluczem) – bez tego możliwy MITM,
- **dedykowany sprzęt** (światłowód, łącza satelitarne), ograniczony **zasięg** (rzędu 100 km w światłowodzie bez węzłów zaufanych) i niska szybkość,
- realne urządzenia mają **luki implementacyjne** (ataki na detektory, kanały boczne),
- zapewnia **tylko dystrybucję klucza** – nie rozwiązuje podpisów, uwierzytelniania, ogólnych potrzeb; trudna integracja,
- urzędy bezpieczeństwa (NSA, NCSC, ANSSI, BSI) **zalecają PQC** zamiast QKD jako główne rozwiązanie; QKD jako uzupełnienie w wybranych zastosowaniach,
- przykłady: łącza metropolitalne, satelita Micius, program EuroQCI.

Do kryptografii kwantowej zalicza się też m.in. **kwantowe generatory liczb losowych (QRNG)**, kwantowe zobowiązania, podpisy kwantowe (głównie teoretyczne).

## Kryptografia postkwantowa (PQC)

**Kryptografia postkwantowa** to **klasyczne algorytmy kryptograficzne** (działające na zwykłych komputerach, w oprogramowaniu), które są **uważane za odporne zarówno na ataki klasyczne, jak i kwantowe**. Opierają się na problemach, dla których nie znamy kwantowego algorytmu wielomianowego.

### Rodziny PQC

| Rodzina | Trudny problem | Przykłady | Uwagi |
| :--- | :--- | :--- | :--- |
| **Kratowe (lattice)** | LWE / Module-LWE, SIS, NTRU | **ML-KEM** (Kyber), **ML-DSA** (Dilithium), Falcon (**FN-DSA**) | efektywne; główna rodzina; większe klucze/podpisy niż ECC |
| **Oparte na skrótach** | odporność funkcji skrótu | **SLH-DSA** (SPHINCS+), XMSS/LMS (stanowe) | konserwatywne założenia; duże podpisy |
| **Oparte na kodach** | dekodowanie kodów liniowych | Classic McEliece, **HQC**, BIKE | bardzo duże klucze publiczne (McEliece) |
| Wielomiany wielu zmiennych | układy równań wielomianowych | Rainbow (**złamany 2022**) | |
| Izogenie krzywych | izogenie | SIKE (**złamany 2022**) | pokazuje ryzyko młodych konstrukcji |

### Standaryzacja NIST

- **13 sierpnia 2024** – opublikowano pierwsze standardy: **FIPS 203 (ML-KEM** – uzgadnianie klucza), **FIPS 204 (ML-DSA** – podpis), **FIPS 205 (SLH-DSA** – podpis hash-based),
- **FIPS 206 (FN-DSA, oparty na Falcon)** – podpis, **w fazie projektu** (wg dostępnych informacji; sprawdź aktualny status),
- **HQC** wybrany w **marcu 2025** jako dodatkowy KEM (kopia zapasowa, inna matematyka niż ML-KEM); projekt standardu spodziewany ok. 2026,
- rekomendacje wycofania algorytmów podatnych na kwanty (RSA, ECC): projekt **NIST IR 8547** – ograniczenie po 2030, zakaz po 2035 (terminy orientacyjne); w UE i innych krajach analogiczne mapy drogowe.
- **Wdrożenia hybrydowe** – łączenie klasycznego i postkwantowego mechanizmu (np. **X25519 + ML-KEM** w TLS 1.3), wdrażane już w przeglądarkach, CDN, komunikatorach (Signal PQXDH) – ochrona nawet przy późniejszym odkryciu słabości jednego z nich.

### Kompromisy PQC

- **większe rozmiary** kluczy, szyfrogramów i podpisów (np. ML-KEM-768: klucz publiczny ok. 1,2 kB, szyfrogram ok. 1,1 kB; ML-DSA-65 podpis ok. 3,3 kB; vs ECC: kilkadziesiąt bajtów),
- inne profile wydajności, wpływ na protokoły (fragmentacja pakietów TLS), IoT,
- **młodsza analiza bezpieczeństwa** (krótszy czas kryptoanalizy) – stąd podejście hybrydowe i kryptoagilność,
- potrzebna ochrona przed kanałami bocznymi w nowych implementacjach.

## Porównanie

| Cecha | **Kryptografia kwantowa (QKD)** | **Kryptografia postkwantowa (PQC)** |
| :--- | :--- | :--- |
| **Podstawa bezpieczeństwa** | **prawa fizyki kwantowej** (zakaz klonowania, zaburzenie pomiaru) | **trudność problemów matematycznych** (kraty, kody, skróty) |
| **Rodzaj bezpieczeństwa** | teorio-informacyjne (przy poprawnej implementacji) | obliczeniowe (założenia o trudności) |
| **Sprzęt** | **wymaga sprzętu kwantowego** (źródła fotonów, detektory, światłowód/satelita) | **zwykłe komputery** i oprogramowanie |
| **Funkcje** | tylko **dystrybucja klucza** | KEM (uzgadnianie klucza), **podpisy**, szyfrowanie |
| **Uwierzytelnianie** | wymaga wstępnie uzgodnionego klucza/podpisu | wbudowane (podpisy PQC) |
| **Zasięg/skalowalność** | ograniczona (dystans, szybkość, koszt) | pełna (Internet, urządzenia) |
| **Cel** | uzgodnienie klucza odpornego na podsłuch | ochrona przed **atakiem komputera kwantowego** na klucz publiczny |
| **Zależność od komputera kwantowego** | nie – działa bez niego; **chroni także przed** nim | to **odpowiedź** na jego zagrożenie |
| **Stan** | ograniczone wdrożenia | **standaryzowana (NIST)**, migracja trwa |

**Często mylone:** kryptografia kwantowa **nie** jest „kryptografią odporną na komputery kwantowe" (to PQC) ani „kryptografią uruchamianą na komputerze kwantowym".

## Podsumowanie

- **Zagrożenie:** algorytm **Shora** łamie **RSA, DH, ElGamal, ECC** (wielomianowo); algorytm **Grovera** zmniejsza efektywną siłę kluczy symetrycznych i skrótów o połowę – wystarczy **AES-256** i skróty ≥ 256 bitów.
- Szczególnie groźne: **„harvest now, decrypt later"** – dane zbierane dziś, odszyfrowane później.
- **Kryptografia kwantowa (QKD, BB84):** bezpieczeństwo z fizyki, dedykowany sprzęt, tylko dystrybucja klucza, wymaga uwierzytelnionego kanału.
- **Kryptografia postkwantowa:** klasyczne algorytmy odporne na kwanty (kraty: **ML-KEM, ML-DSA**; hash-based: **SLH-DSA**; kody: HQC); standardy NIST 2024; migracja i **kryptoagilność**, rozwiązania hybrydowe.
