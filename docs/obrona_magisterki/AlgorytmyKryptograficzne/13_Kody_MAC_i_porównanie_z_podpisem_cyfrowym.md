# Do czego służą kody MAC? Porównaj ogólnie uwierzytelnianie wiadomości za pomocą MAC z podpisem cyfrowym.

> **💬 Gotowa wypowiedź ustna:**
> *"Kod MAC to krótki znacznik dołączany do wiadomości, obliczany z jej treści i tajnego klucza, który znają obie strony. Zapewnia integralność, czyli pewność, że wiadomość nie została zmieniona, oraz autentyczność, czyli pewność, że pochodzi od kogoś znającego klucz. Odbiorca liczy MAC samodzielnie i porównuje go z otrzymanym. Najczęściej stosuje się HMAC, na przykład HMAC-SHA-256. MAC nie zapewnia poufności, bo treść pozostaje jawna.
>
> Najważniejsza różnica względem podpisu cyfrowego dotyczy kluczy: MAC jest symetryczny, a podpis asymetryczny, czyli podpisuje się kluczem prywatnym, a weryfikuje publicznym. Dlatego podpis może zweryfikować każdy i zapewnia niezaprzeczalność, a MAC jej nie daje, bo obie strony znają klucz i każda mogłaby wygenerować taki sam znacznik. Za to MAC jest znacznie szybszy i nie wymaga PKI.
>
> W praktyce MAC wybieramy, gdy strony mają wspólny klucz i liczy się wydajność, na przykład w TLS czy IPsec. Podpis wybieramy, gdy potrzebna jest niezaprzeczalność albo wielu odbiorców, na przykład przy podpisie dokumentów."*

**Kod MAC (Message Authentication Code)** to krótki znacznik dołączany do wiadomości, obliczany z jej treści i **tajnego klucza współdzielonego** przez nadawcę i odbiorcę. Służy do zapewnienia **integralności** (wiadomość nie została zmieniona) oraz **autentyczności** (pochodzi od kogoś, kto zna klucz). Odbiorca liczy MAC ze otrzymanej wiadomości tym samym kluczem i porównuje go z dołączonym. Najczęściej stosuje się **HMAC** (np. HMAC-SHA-256), który łączy funkcję skrótu z kluczem. MAC jest też chroniony przed atakiem *length extension*, który dotyczy zwykłego skrótu. MAC nie zapewnia poufności, bo treść zostaje jawna.

**Porównanie MAC z podpisem cyfrowym:**

- **Klucze:** MAC jest **symetryczny** (jeden wspólny klucz), a podpis **asymetryczny** (klucz prywatny do podpisu, publiczny do weryfikacji).
- **Kto może zweryfikować:** MAC może zweryfikować tylko strona znająca klucz. Podpis może zweryfikować każdy, kto ma klucz publiczny.
- **Niezaprzeczalność:** MAC jej **nie daje**, bo obie strony znają klucz i każda mogłaby wygenerować taki sam znacznik. Podpis ją daje, bo klucz prywatny ma tylko autor.
- **Szybkość:** MAC jest znacznie szybszy i mniej kosztowny obliczeniowo. Podpis jest wolniejszy.
- **Dystrybucja kluczy:** MAC wymaga bezpiecznego uzgodnienia wspólnego klucza. Podpis wymaga infrastruktury PKI i certyfikatów.

**Kiedy co stosować:** MAC wybieramy, gdy dwie strony mają wspólny klucz i liczy się wydajność, np. w TLS, IPsec czy tokenach API. Podpis wybieramy, gdy potrzebna jest niezaprzeczalność lub gdy wiadomość ma weryfikować wielu odbiorców, np. przy podpisie dokumentów czy aktualizacji oprogramowania.

## Podsumowanie

- **MAC** = tag $\text{MAC}_k(m)$ liczony ze **wspólnym kluczem tajnym**; zapewnia **integralność i autentyczność** (wobec stron znających klucz), **bez niezaprzeczalności** i bez poufności.
- Najczęstsze: **HMAC-SHA-256**, CMAC, Poly1305/GMAC (w AEAD).
- **Podpis cyfrowy**: asymetryczny, publicznie weryfikowalny, daje **niezaprzeczalność**, ale jest wolniejszy; MAC – szybki, symetryczny, tylko między stronami z wspólnym kluczem.

---
[⬅️ Poprzedni temat](12_Funkcje_skrótu.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](14_Kryptografia_krzywych_eliptycznych.md)