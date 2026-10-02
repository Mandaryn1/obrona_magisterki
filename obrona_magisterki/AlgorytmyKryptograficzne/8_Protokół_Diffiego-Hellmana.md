# Protokół Diffiego-Hellmana (DH)

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

Podsłuchujący (Ewa) widzi: $p,g,A,B$. Chce obliczyć $g^{ab}$.

- Wprost: znaleźć $a$ z $A=g^a$ – **problem logarytmu dyskretnego (DLP)**; dla dużych $p$ nie ma wydajnego algorytmu (najlepsze: sito ciała liczbowego – sub-wykładnicze; dla $p$ 2048 bitów nieosiągalne).
- Bezpieczeństwo protokołu opiera się formalnie na **założeniu Diffiego-Hellmana (CDH)**: z $g^a$ i $g^b$ trudno obliczyć $g^{ab}$ (nie jest znane, czy trudniejsze niż DLP, ale **w praktyce nie znaleziono lepszego ataku**). Wersja decyzyjna: **DDH**.
- Sekrety $a,b$ **nigdy nie opuszczają** stron; przez kanał idą tylko $A,B$.

## Wymagania na parametry

- $p$ – **bezpieczna liczba pierwsza** ($p=2q+1$, $q$ pierwsze), zalecane **≥ 2048 bitów** (standardowe grupy: RFC 3526, RFC 7919 *ffdhe*),
- $g$ generuje podgrupę dużego rzędu pierwszego,
- $a,b$ – losowe, o odpowiedniej długości (≥ 256 bitów),
- **Walidacja kluczy publicznych** (sprawdzenie, że $A\notin\{0,1,p-1\}$, należy do podgrupy), by uniknąć ataków na małe podgrupy.
- **Nie** stosować grup małych/ułomnych (DH-512, DH-1024 – atak *Logjam*).

## Główny problem: brak uwierzytelniania – atak „człowiek w środku" (MITM)

Czysty DH **nie uwierzytelnia stron**. Aktywny przeciwnik Mallory:

1. przechwytuje $A$ od Alicji, wysyła do Boba własne $M_1=g^{m_1}$,
2. przechwytuje $B$ od Boba, wysyła do Alicji $M_2=g^{m_2}$,
3. Alicja uzgadnia klucz $K_A=g^{a m_2}$ z Mallorym, Bob $K_B=g^{b m_1}$ z Mallorym – Mallory **odczytuje i przekazuje** wiadomości, a strony nie wiedzą o niczym.

**Obrona:** **uwierzytelnienie wymiany** – **podpisy cyfrowe** (certyfikaty) na wartościach DH (jak w TLS, IKE, SSH), MAC z kluczem wstępnie współdzielonym, hasła (PAKE).

## Warianty i zastosowania

| Wariant | Opis |
| :--- | :--- |
| **DHE / EDH** (ephemeral) | **efemeryczne** klucze DH (nowe $a,b$ dla każdej sesji) → **forward secrecy** (poufność przekazywania): kompromitacja klucza długoterminowego nie ujawnia dawnych sesji |
| **ECDH / ECDHE** | DH na **krzywych eliptycznych** (Curve25519, P-256) – krótsze klucze, szybsze (temat 14) |
| **Statyczny DH** | stałe klucze publiczne (np. w certyfikacie); brak forward secrecy |
| **DH z wieloma stronami** | uogólnienia (Burmester–Desmedt) |
| **X3DH, Double Ratchet** | protokoły komunikatorów (Signal) oparte na DH |
| **KEM (DH jako enkapsulacja klucza)** | w podejściu postkwantowym zastępowany przez ML-KEM |

**Zastosowania:** **TLS** (ECDHE), **SSH**, **IPsec/IKE**, VPN (WireGuard – Curve25519), Signal, WhatsApp.

## DH a RSA

| | DH | RSA |
| :--- | :--- | :--- |
| Funkcja | **uzgadnianie klucza** (obie strony wnoszą wkład) | szyfrowanie/podpis (klucz transportowany) |
| Problem trudny | **logarytm dyskretny / CDH** | faktoryzacja / problem RSA |
| Forward secrecy | **tak** (wersje efemeryczne) | nie przy transportowaniu klucza RSA |
| Podpis | nie | tak |

## Podsumowanie

- **DH** pozwala przez otwarty kanał uzgodnić wspólny sekret: $A=g^a$, $B=g^b$, $K=g^{ab}\bmod p$.
- Bezpieczeństwo: trudność **logarytmu dyskretnego** (CDH/DDH) – podsłuch nie wystarczy do wyliczenia klucza.
- **Brak uwierzytelniania → MITM**; trzeba podpisów/certyfikatów; wersje **efemeryczne** dają forward secrecy; ECDH – krótsze klucze.
