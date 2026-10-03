# Omów rolę klucza w kryptografii symetrycznej i asymetrycznej. Jakie problemy rozwiązuje kryptografia klucza publicznego?

## Klucz w kryptografii

**Klucz** to parametr (zwykle losowy ciąg bitów), który **steruje działaniem algorytmu** kryptograficznego. Zgodnie z **zasadą Kerckhoffsa** całe bezpieczeństwo systemu spoczywa na **tajności klucza** (symetryczne: klucz tajny; asymetryczne: klucz prywatny). Dlatego kluczowe są: **generowanie** (dobra losowość), **przechowywanie** (HSM, KMS), **dystrybucja**, **rotacja**, **unieważnianie**, **niszczenie**.

## Kryptografia symetryczna

- **Jeden wspólny klucz tajny** $k$ do szyfrowania i deszyfrowania (lub do generowania i weryfikacji MAC).
- **Zalety:** szybkość (setki MB/s–GB/s), krótkie klucze (128–256 bitów), prostota.
- **Role klucza:** szyfrowanie/deszyfrowanie; MAC; wyprowadzanie kluczy pochodnych (KDF).

Odpowiedź tradycyjna: **centra dystrybucji kluczy** (KDC, Kerberos) – wymagają zaufanej trzeciej strony.

## Kryptografia asymetryczna (klucza publicznego)

Każdy użytkownik ma **parę kluczy**:

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