# Rola klucza w kryptografii symetrycznej i asymetrycznej. Problemy rozwiązywane przez kryptografię klucza publicznego

## Klucz w kryptografii

**Klucz** to parametr (zwykle losowy ciąg bitów), który **steruje działaniem algorytmu** kryptograficznego. Zgodnie z **zasadą Kerckhoffsa** całe bezpieczeństwo systemu spoczywa na **tajności klucza** (symetryczne: klucz tajny; asymetryczne: klucz prywatny). Dlatego kluczowe są: **generowanie** (dobra losowość), **przechowywanie** (HSM, KMS), **dystrybucja**, **rotacja**, **unieważnianie**, **niszczenie**.

## Kryptografia symetryczna

- **Jeden wspólny klucz tajny** $k$ do szyfrowania i deszyfrowania (lub do generowania i weryfikacji MAC).
- **Zalety:** szybkość (setki MB/s–GB/s), krótkie klucze (128–256 bitów), prostota.
- **Role klucza:** szyfrowanie/deszyfrowanie; MAC; wyprowadzanie kluczy pochodnych (KDF).
- **Problemy:**
  1. **Dystrybucja kluczy** – obie strony muszą wcześniej **bezpiecznie** uzgodnić sekret (kanał zabezpieczony, kurier, spotkanie). Dla nieznajomych w Internecie jest to niemożliwe bez dodatkowej metody.
  2. **Skalowalność** – dla $n$ użytkowników potrzeba $\frac{n(n-1)}{2}$ kluczy (100 osób → 4950; 1000 → 499 500).
  3. **Brak niezaprzeczalności** – klucz mają obie strony, więc każda z nich mogła stworzyć wiadomość.
  4. **Zarządzanie** – wymiana, unieważnianie, przechowywanie wielu kluczy.

Odpowiedź tradycyjna: **centra dystrybucji kluczy** (KDC, Kerberos) – wymagają zaufanej trzeciej strony.

## Kryptografia asymetryczna (klucza publicznego)

Pomysł: Diffie i Hellman (1976), RSA (1977). Każdy użytkownik ma **parę kluczy**:

| Klucz | Dostępność | Zastosowanie |
| :--- | :--- | :--- |
| **publiczny** $pk$ | jawny, rozpowszechniany | szyfrowanie dla właściciela, **weryfikacja podpisu** właściciela |
| **prywatny** $sk$ | tajny, tylko u właściciela | deszyfrowanie, **składanie podpisu** |

Własność: z $pk$ **nie da się praktycznie** wyliczyć $sk$ (trudny problem matematyczny – funkcja jednokierunkowa z „zapadką" (*trapdoor*)).

### Dwa podstawowe sposoby użycia pary kluczy

- **Szyfrowanie (poufność):** nadawca szyfruje kluczem **publicznym odbiorcy**, tylko odbiorca (klucz prywatny) odszyfruje.
- **Podpis cyfrowy (autentyczność, niezaprzeczalność):** nadawca podpisuje kluczem **prywatnym**, każdy weryfikuje kluczem **publicznym nadawcy**.

## Problemy rozwiązywane przez kryptografię klucza publicznego

| Problem | Jak rozwiązuje |
| :--- | :--- |
| **Dystrybucja klucza** | klucz publiczny można przesłać jawnie; **nie trzeba wcześniej uzgadniać sekretu**. Protokoły uzgadniania klucza (DH/ECDH) i enkapsulacji klucza (KEM) pozwalają ustalić klucz sesji przez otwarty kanał |
| **Skalowalność** | każdy użytkownik ma jedną parę; $n$ osób → $n$ par, a nie $\frac{n(n-1)}{2}$ kluczy |
| **Niezaprzeczalność** | podpis cyfrowy powstaje kluczem, który zna tylko podpisujący – nie można się wyprzeć (przy poprawnym zarządzaniu kluczem) |
| **Uwierzytelnianie nieznajomych** | certyfikaty (PKI) wiążą klucz publiczny z tożsamością; strony bez wcześniejszego kontaktu mogą ustalić tożsamość |
| **Komunikacja z nieznanymi stronami** | HTTPS, e-mail szyfrowany, logowanie |
| **Integralność i autentyczność dokumentów** | podpis cyfrowy (e-podpis, kod podpisany, aktualizacje oprogramowania) |
| **Forward secrecy** | efemeryczne DH – skompromitowanie klucza długoterminowego nie ujawnia dawnych sesji |

## Ograniczenia kryptografii asymetrycznej

- **Wolna** (setki–tysiące razy wolniejsza od symetrycznej) i **długie klucze** (RSA 2048–4096; ECC 256) → w praktyce **tylko do kluczy i podpisów**, dane szyfruje się symetrycznie (**system hybrydowy**).
- Nowy problem: **autentyczność klucza publicznego** – skąd wiedzieć, że klucz należy do właściwej osoby? Odpowiedź: **PKI** (urzędy certyfikacji, certyfikaty X.509), **sieć zaufania** (PGP), TOFU (SSH). Bez tego możliwy **MITM** (podmiana klucza).
- Zagrożona przez **komputery kwantowe** (Shor) – zob. temat 15.
- Zarządzanie kluczem prywatnym (utrata = utrata danych; wyciek = kompromitacja tożsamości).

## Systemy hybrydowe

```
 Alicja                                              Bob
   │  1. klucz publiczny Boba (z certyfikatu)  ◀──────│
   │  2. losowy klucz sesji K (symetryczny)           │
   │  3. K zaszyfrowany kluczem publicznym Boba ─────▶│  (lub uzgodnienie ECDH)
   │  4. dane szyfrowane AES-GCM kluczem K  ─────────▶│
```

TLS, PGP, S/MIME, SSH, Signal: asymetria do uzgodnienia klucza i uwierzytelnienia, symetria do danych.

## Siła klucza – porównanie rozmiarów (NIST SP 800-57, orientacyjnie)

| Poziom bezpieczeństwa (bity) | Symetryczny | RSA / DH (moduł) | ECC (rozmiar klucza) |
| :-: | :-: | :-: | :-: |
| 112 | 3DES (112) | 2048 | 224 |
| 128 | AES-128 | 3072 | 256 |
| 192 | AES-192 | 7680 | 384 |
| 256 | AES-256 | 15360 | 512 |

Klucze asymetryczne muszą być **znacznie dłuższe** przy tym samym poziomie bezpieczeństwa (ataki sub-wykładnicze na faktoryzację i logarytm dyskretny).

## Porównanie ról klucza

| | Symetryczna | Asymetryczna |
| :--- | :--- | :--- |
| Liczba kluczy na użytkownika | 1 na parę rozmówców | **1 para** (publiczny + prywatny) |
| Tajność | klucz musi być **tajny u obu stron** | tajny tylko prywatny; publiczny jawny |
| Szyfrowanie / deszyfrowanie | ten sam klucz | różne klucze |
| Podpis cyfrowy | niemożliwy (MAC bez niezaprzeczalności) | **możliwy** |
| Dystrybucja | trudna (kanał zabezpieczony) | łatwa (klucz publiczny jawnie; potrzeba PKI) |
| Szybkość | bardzo wysoka | niska |

## Podsumowanie

- **Klucz** to jedyny sekret systemu (zasada Kerckhoffsa); jego **generowanie, przechowywanie, rotacja i dystrybucja** decydują o bezpieczeństwie.
- **Symetryczna:** jeden wspólny klucz tajny, szybko, ale **problem dystrybucji** i $\frac{n(n-1)}{2}$ kluczy.
- **Asymetryczna:** para (publiczny + prywatny); rozwiązuje **dystrybucję, skalowalność, podpisy i niezaprzeczalność**; wolniejsza, wymaga PKI.
- W praktyce **hybrydowo**: asymetria do uzgodnienia klucza i podpisów, symetria do danych.

---
[⬅️ Poprzedni temat](6_Tryby_pracy_szyfrów_blokowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Protokół_Diffiego-Hellmana.md)