# Podpis elektroniczny (cyfrowy) – własności bezpieczeństwa. Różnica między szyfrowaniem a podpisywaniem

## Czym jest podpis cyfrowy

**Podpis cyfrowy** to wynik operacji kryptograficznej na dokumencie (a właściwie na jego **skrócie**), wykonanej **kluczem prywatnym** podpisującego, który **każdy może zweryfikować kluczem publicznym** podpisującego. Jest elektronicznym odpowiednikiem podpisu odręcznego, ale **zależy od treści dokumentu** (zmiana choćby jednego bitu unieważnia podpis).

Schemat podpisu to trzy algorytmy:

1. **Generowanie kluczy** $(sk,pk)$,
2. **Podpisywanie** $\sigma=\text{Sign}_{sk}(m)$,
3. **Weryfikacja** $\text{Verify}_{pk}(m,\sigma)\in\{\text{ważny},\text{nieważny}\}$.

## Wymagane własności bezpieczeństwa

| Własność | Znaczenie |
| :--- | :--- |
| **Autentyczność (uwierzytelnienie nadawcy)** | podpis dowodzi, że pochodzi od właściciela klucza prywatnego |
| **Integralność** | każda zmiana treści po podpisaniu jest wykrywana (weryfikacja zawodzi) |
| **Niezaprzeczalność** (*non-repudiation*) | podpisujący nie może wiarygodnie wyprzeć się podpisu (tylko on zna klucz prywatny); dowód dla strony trzeciej (sąd) |
| **Niemożność sfałszowania** (*unforgeability*, EUF-CMA) | nie da się wytworzyć ważnego podpisu **nowej** wiadomości bez klucza prywatnego, nawet po obejrzeniu wielu podpisów innych wiadomości |
| **Niemożność przeniesienia** | podpis jest związany z konkretnym dokumentem – nie da się go „przykleić" do innego |
| *(uzupełnienie)* **Weryfikowalność publiczna** | każdy mający klucz publiczny (i certyfikat) może sprawdzić podpis |
| *(uzupełnienie)* **Znacznik czasu** | dowód, że podpis powstał przed pewnym momentem (usługa znakowania czasem) |

Podpis **nie zapewnia poufności** – dokument pozostaje jawny (chyba że dodatkowo zaszyfrowany).

## Jak działa (podpis ze skrótem)

Dokument bywa duży, a operacje asymetryczne są wolne, dlatego podpisuje się **skrót**:

```
 NADAWCA                                           ODBIORCA
 dokument m ─▶ h(m)  (skrót, np. SHA-256)          dokument m' ─▶ h(m')
              │                                                    │
        podpis kluczem PRYWATNYM  ──▶ σ  ───────▶  weryfikacja kluczem PUBLICZNYM
                                                    (czy σ odpowiada h(m')?)
```

1. Nadawca oblicza $H=h(m)$ (funkcja skrótu).
2. Podpisuje: $\sigma=\text{Sign}_{sk}(H)$ (np. RSA: $\sigma=H^d\bmod n$).
3. Wysyła $(m,\sigma)$.
4. Odbiorca oblicza $H'=h(m')$ i weryfikuje $\sigma$ kluczem publicznym; jeśli się zgadza – dokument nie zmieniony i podpisany przez właściciela klucza.

**Przykład (RSA, sprawdzony):** klucze jak w temacie 9 ($n=3233$, $e=17$, $d=2753$); skrót $H=65$; podpis $\sigma=65^{2753}\bmod3233=588$; weryfikacja: $588^{17}\bmod3233=65=H$ ✓.

## Algorytmy podpisu

| Algorytm | Podstawa | Uwagi |
| :--- | :--- | :--- |
| **RSA-PSS** (RSASSA-PSS), RSA PKCS#1 v1.5 | faktoryzacja | PSS – zalecany (probabilistyczny, z dowodem); v1.5 tylko zgodność |
| **DSA** | logarytm dyskretny | wycofywany |
| **ECDSA** | krzywe eliptyczne | krótkie podpisy; **wymaga unikalnego $k$** (powtórzenie → wyciek klucza) |
| **EdDSA (Ed25519, Ed448)** | krzywe skręcone Edwardsa | deterministyczny (brak ryzyka $k$), szybki, zalecany |
| **ML-DSA, SLH-DSA, FN-DSA** | kraty / skróty | postkwantowe (temat 15) |

## Różnica między szyfrowaniem a podpisywaniem

Oba używają pary kluczy, ale **kierunek użycia kluczy i cel są różne**:

| Cecha | **Szyfrowanie asymetryczne** | **Podpis cyfrowy** |
| :--- | :--- | :--- |
| **Cel** | **poufność** (ukrycie treści) | **autentyczność, integralność, niezaprzeczalność** |
| **Kto używa jakiego klucza** | nadawca szyfruje kluczem **publicznym odbiorcy**; odbiorca deszyfruje swoim **prywatnym** | nadawca podpisuje swoim kluczem **prywatnym**; każdy weryfikuje jego kluczem **publicznym** |
| **Kto może wykonać operację** | **każdy** może zaszyfrować (klucz publiczny jawny), **tylko odbiorca** odszyfruje | **tylko podpisujący** może podpisać, **każdy** może zweryfikować |
| **Co chronione** | treść wiadomości | pochodzenie i niezmienność treści |
| **Treść po operacji** | zaszyfrowana, nieczytelna | **jawna** (podpis dołączony) |
| **Wynik** | szyfrogram (da się odwrócić do tekstu) | podpis (weryfikowany, nie „odszyfrowywany") |
| **Wejście** | cała wiadomość (zwykle klucz sesji) | **skrót** wiadomości |
| **Zaprzeczenie** | nadawca może zaprzeczyć (każdy mógł zaszyfrować) | podpisujący nie może zaprzeczyć |

### Kolejność w praktyce

Aby uzyskać **poufność i autentyczność**, stosuje się oba mechanizmy – **podpisz, a potem zaszyfruj** (lub szyfrowanie uwierzytelnione). PGP/S-MIME: podpis nadawcy + szyfrowanie kluczem publicznym odbiorcy.

### Częsty błąd

Rozumienie podpisu jako „szyfrowania skrótu kluczem prywatnym" jest tylko **skrótem myślowym** dla RSA; w ECDSA/EdDSA nie ma operacji „szyfrowania". Nie należy też używać tego samego klucza i do szyfrowania, i do podpisu bez uzasadnienia (osobne klucze).

## Podpis cyfrowy a podpis elektroniczny (prawo)

- **Podpis cyfrowy** – pojęcie techniczne (kryptograficzne).
- **Podpis elektroniczny** – pojęcie prawne (rozporządzenie **eIDAS**, ustawa o usługach zaufania w Polsce). Rodzaje: **zwykły**, **zaawansowany**, **kwalifikowany podpis elektroniczny** (certyfikat kwalifikowany, urządzenie QSCD) – **równoważny prawnie podpisowi własnoręcznemu**. W Polsce także podpis zaufany (profil zaufany) i e-dowód.
- Formaty: **XAdES, PAdES, CAdES**; **znakowanie czasem** zapewnia ważność podpisu w czasie.

## Zaufanie do klucza publicznego – PKI

Podpis dowodzi tylko, że sygnatariusz posiada klucz prywatny pasujący do klucza publicznego. Aby powiązać klucz z **tożsamością**, stosuje się **certyfikaty X.509** wystawiane przez **urzędy certyfikacji (CA)** (podpisane przez CA), **listy CRL/OCSP** do unieważniania. Bez tego możliwy jest atak przez podmianę klucza (MITM).

## Zagrożenia

- **kradzież lub wyciek klucza prywatnego** (rozwiązanie: HSM, karty, hasło/PIN),
- **kolizje funkcji skrótu** (MD5, SHA-1) – podpisanie jednego dokumentu pozwala podrobić drugi,
- **słaba losowość** w ECDSA/DSA,
- **podpisywanie tego, czego się nie widzi** (WYSIWYS – *What You See Is What You Sign*),
- ataki na implementację i na PKI (skompromitowane CA),
- komputery kwantowe zagrażają RSA/ECDSA (Shor).

## Podsumowanie

- **Podpis cyfrowy** = skrót dokumentu zaszyfrowany kluczem prywatnym (w ujęciu RSA) i weryfikowalny kluczem publicznym; zapewnia **autentyczność, integralność i niezaprzeczalność**, nie poufność.
- **Szyfrowanie** – kluczem publicznym **odbiorcy**, cel: **poufność**; **podpis** – kluczem prywatnym **nadawcy**, cel: **uwierzytelnienie i niezaprzeczalność**.
- Praktyka: podpis skrótu (RSA-PSS, ECDSA, Ed25519), certyfikaty (PKI), znakowanie czasem; prawnie – kwalifikowany podpis elektroniczny.

---
[⬅️ Poprzedni temat](10_Porównanie_RSA_i_ElGamala.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](12_Funkcje_skrótu.md)