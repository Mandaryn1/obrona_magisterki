# Wyjaśnij pojęcie tajności doskonałej. Dlaczego szyfr z kluczem jednorazowym jest doskonale tajny, ale ponowne użycie tego samego klucza jest niebezpieczne?

**Tajność doskonała (Shannon)** oznacza, że szyfrogram **nie daje żadnej informacji o tekście jawnym**. Formalnie: prawdopodobieństwo, że wiadomością jest M, po zobaczeniu szyfrogramu C jest takie samo jak przed jego zobaczeniem, czyli P(M|C) = P(M). Przeciwnik, nawet z nieograniczoną mocą obliczeniową, nie dowie się niczego o treści. Shannon udowodnił, że warunkiem jest klucz **co najmniej tak długi jak wiadomość** i **użyty tylko raz**.

**Szyfr z kluczem jednorazowym (OTP, one-time pad)** szyfruje przez XOR tekstu z kluczem: c = m ⊕ k. Klucz jest **losowy, równie długi jak wiadomość i używany jednorazowo**. Jest doskonale tajny, bo dla danego szyfrogramu każdy tekst jawny tej samej długości jest równie prawdopodobny: istnieje dokładnie jeden klucz, który go z tym szyfrogramem łączy, a klucz jest losowy. Szyfrogram wygląda więc jak losowy szum.

**Dlaczego ponowne użycie klucza jest niebezpieczne:** jeśli ten sam klucz zaszyfruje dwie wiadomości, to

c₁ ⊕ c₂ = (m₁ ⊕ k) ⊕ (m₂ ⊕ k) = m₁ ⊕ m₂

Klucz się **skraca** i zostaje XOR dwóch tekstów jawnych. Z niego można odtwarzać treść przez analizę statystyczną i zgadywanie fragmentów (tzw. *crib dragging*). Gdy poznamy jeden tekst, od razu mamy klucz i wszystkie pozostałe wiadomości. Tak złamano szyfrowaną korespondencję radziecką w projekcie **VENONA**, gdzie klucze częściowo użyto ponownie.

**Wady OTP w praktyce:** klucz musi być tak długi jak wiadomość, bezpiecznie rozdzielany i naprawdę losowy. Dodatkowo szyfr nie zapewnia **integralności**: zmiana bitu w szyfrogramie zmienia ten sam bit tekstu jawnego. Dlatego stosuje się szyfry strumieniowe, które naśladują OTP pseudolosowym strumieniem, ale z tym samym ograniczeniem: nonce nie wolno powtórzyć.

## Podsumowanie

- **Tajność doskonała:** szyfrogram nie wnosi żadnej informacji o tekście jawnym, $P(M|C)=P(M)$; wymaga klucza **losowego**, **nie krótszego** niż wiadomość i użytego **raz**.
- **OTP:** $c=m\oplus k$; doskonale tajny (Shannon).
- Ponowne użycie klucza: $c_1\oplus c_2=m_1\oplus m_2$ – klucz znika, tekst zostaje odsłonięty (Venona).
- Wady: klucz długości wiadomości, dystrybucja, prawdziwa losowość, brak integralności → w praktyce stosuje się szyfry obliczeniowo bezpieczne.

---
[⬅️ Poprzedni temat](3_Zasada_Kerckhoffsa.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️️](5_Szyfry_klasyczne_a_nowoczesne_algorytmy_kryptograficzne.md)