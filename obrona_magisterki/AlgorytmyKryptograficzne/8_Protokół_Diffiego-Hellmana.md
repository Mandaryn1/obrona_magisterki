# Wyjaśnij ogólną ideę protokołu Diffiego-Hellmana. Dlaczego umożliwia on uzgodnienie wspólnego sekretu przez niezabezpieczony kanał komunikacyjny?

## Cel

**Protokół Diffiego-Hellmana** (Whitfield Diffie, Martin Hellman, 1976; niezależnie wcześniej Malcolm Williamson w GCHQ) to **pierwszy opublikowany protokół kryptografii klucza publicznego**. Pozwala dwóm stronom **uzgodnić wspólny tajny klucz** (sekret) przez **niezabezpieczony kanał**, bez wcześniejszego dzielenia się żadną tajemnicą. Uzgodniony sekret służy zwykle do wyprowadzenia klucza **symetrycznego** (np. AES).

> DH to protokół **uzgadniania klucza**, nie szyfrowania ani podpisu.

## Idea

Wykorzystuje **funkcję jednokierunkową**: potęgowanie modulo $p$ jest łatwe, a **odwrócenie** (logarytm dyskretny) – trudne.

## Przebieg protokołu

**Parametry publiczne** (uzgodnione jawnie): duża liczba pierwsza $p$ i generator $g$ (element rzędu wysokiego w grupie $\mathbb{Z}_p^*$).

| Krok | Alicja | Bob |
| :-: | :--- | :--- |
| 1 | wybiera **tajne** losowe $a$ | wybiera **tajne** losowe $b$ |
| 2 | oblicza $A=g^a\bmod p$ | oblicza $B=g^b\bmod p$ |
| 3 | wysyła $A$ ──▶ | ◀── wysyła $B$ |
| 4 | oblicza $K=B^a\bmod p$ | oblicza $K=A^b\bmod p$ |

Obie strony otrzymują ten sam sekret:

$$K=B^a=(g^b)^a=g^{ab}=(g^a)^b=A^b\pmod p$$

### Przykład liczbowy (sprawdzony)

Parametry: $p=23$, $g=5$.

- Alicja: $a=6$ → $A=5^6\bmod23=8$,
- Bob: $b=15$ → $B=5^{15}\bmod23=19$,
- Alicja: $K=19^6\bmod23=2$,
- Bob: $K=8^{15}\bmod23=2$.

Wspólny klucz: **$K=2$**. Podsłuchujący zna $p=23,\,g=5,\,A=8,\,B=19$, ale aby wyliczyć $K$, musiałby znaleźć $a$ lub $b$ (logarytm dyskretny). W praktyce $p$ ma ≥ 2048 bitów.

## Dlaczego jest bezpieczny wobec podsłuchu

Dlaczego to działa przez niezabezpieczony kanał: podsłuchujący widzi p, g, A i B, ale żeby policzyć gᵃᵇ, musiałby znać a lub b. Ich wyznaczenie z A lub B to problem logarytmu dyskretnego, który dla dużych liczb jest praktycznie nieobliczalny. Obliczenie potęgi modulo jest łatwe, a odwrócenie go jest trudne.

## Podsumowanie

- **DH** pozwala przez otwarty kanał uzgodnić wspólny sekret: $A=g^a$, $B=g^b$, $K=g^{ab}\bmod p$.
- Bezpieczeństwo: trudność **logarytmu dyskretnego** (CDH/DDH) – podsłuch nie wystarczy do wyliczenia klucza.
- **Brak uwierzytelniania → MITM**; trzeba podpisów/certyfikatów; wersje **efemeryczne** dają forward secrecy; ECDH – krótsze klucze.

---
[⬅️ Poprzedni temat](7_Rola_klucza_w_kryptografii_symetrycznej_i_asymetrycznej.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](9_Bezpieczeństwo_kryptosystemu_RSA.md)