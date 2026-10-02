# Tajność doskonała. Szyfr z kluczem jednorazowym

## Tajność doskonała (perfect secrecy)

Pojęcie wprowadzone przez **Claude'a Shannona** (1949, *Communication Theory of Secrecy Systems*). Szyfr ma **tajność doskonałą**, jeśli **szyfrogram nie dostarcza żadnej informacji o tekście jawnym** – wiedza atakującego o wiadomości po zobaczeniu szyfrogramu jest taka sama jak przed jego zobaczeniem, **nawet przy nieograniczonej mocy obliczeniowej**.

**Definicja formalna:** dla każdego rozkładu na wiadomościach, każdej wiadomości $m$ i szyfrogramu $c$ (o dodatniej wartości prawdopodobieństwa):

$$P(M=m\mid C=c)=P(M=m)$$

Równoważnie: $P(C=c\mid M=m_0)=P(C=c\mid M=m_1)$ dla dowolnych $m_0,m_1$ – rozkład szyfrogramu nie zależy od wiadomości. Czyli informacja wzajemna $I(M;C)=0$.

### Warunki konieczne (twierdzenie Shannona)

Przy $|M|=|K|=|C|$ szyfr ma tajność doskonałą wtedy i tylko wtedy, gdy:

1. **klucz jest wybierany jednostajnie losowo** z całej przestrzeni kluczy,
2. **każda para $(m,c)$** ma dokładnie jeden klucz przekształcający $m$ w $c$.

Wniosek: **długość klucza ≥ długość wiadomości** ($|K|\ge|M|$). Klucz krótszy od wiadomości **nie może** zapewnić tajności doskonałej.

## Szyfr z kluczem jednorazowym (one-time pad, OTP; szyfr Vernama)

**Gilbert Vernam** (1917) zaproponował XOR; **Joseph Mauborgne** zauważył, że klucz musi być losowy i jednorazowy. Shannon udowodnił doskonałą tajność.

### Algorytm

- klucz $k$ – **losowy ciąg bitów tej samej długości** co wiadomość, używany **tylko raz**,
- szyfrowanie: $c=m\oplus k$, deszyfrowanie: $m=c\oplus k$ (XOR jest swoją odwrotnością).

(Wersja alfabetowa: dodawanie mod 26, „one-time pad" z notesem papierowym.)

**Przykład** (8 bitów): $m=01001000$ („H"), $k=10110101$ → $c=m\oplus k=11111101$. Odszyfrowanie: $c\oplus k=01001000$.

### Dlaczego jest doskonale tajny

Dla każdego szyfrogramu $c$ i **każdego możliwego tekstu** $m'$ (tej samej długości) istnieje dokładnie jeden klucz $k'=c\oplus m'$, który go wygeneruje, a wszystkie klucze są **równie prawdopodobne**. Atakujący widzi więc szyfrogram zgodny z **każdą** wiadomością z równym prawdopodobieństwem – np. szyfrogram `11111101` może być zarówno „H", jak i dowolną inną literą. Brute force niczego nie daje (nie wiadomo, który wynik jest właściwy).

## Dlaczego ponowne użycie klucza jest niebezpieczne

Jeśli ten sam klucz $k$ posłuży do zaszyfrowania dwóch wiadomości:

$$c_1\oplus c_2=(m_1\oplus k)\oplus(m_2\oplus k)=m_1\oplus m_2$$

**Klucz znika z równania**, a atakujący otrzymuje XOR dwóch tekstów jawnych – bez żadnych tajemnic. Z tego można odtworzyć oba teksty metodami statystycznymi (analiza częstości, znane fragmenty, **crib dragging** – przesuwanie zgadywanego słowa po $c_1\oplus c_2$). Znając choćby jedną wiadomość, atakujący odzyskuje **cały klucz** ($k=m_1\oplus c_1$) i odczytuje wszystkie inne.

**Przykład** (sprawdzony): $m_1=$ `ATTACK AT DAWN`, $m_2=$ `RETREAT AT TEN`, ten sam klucz – obliczone $c_1\oplus c_2$ jest identyczne z $m_1\oplus m_2$. Z cząstkowej znajomości (np. „AT" w obu miejscach) rozpoczyna się łamanie.

Dodatkowo szyfr jest **plastyczny (malleable)**: atakujący może zmienić bit w szyfrogramie i zmieni dokładnie ten bit w tekście jawnym – **brak integralności**; potrzebny MAC.

### Przykłady historyczne

- **Projekt VENONA** (USA, 1943–1980): Sowieci ponownie użyli części stron notesów OTP; dzięki temu dało się częściowo odczytać radzieckie depesze.
- **WEP** (Wi-Fi) i błędne użycie strumieniowych szyfrów z powtarzanym strumieniem klucza (RC4 z krótkim IV) – ta sama klasa błędu (powtórzenie strumienia).
- **Powtórne użycie nonce w AES-GCM / ChaCha20** – ten sam efekt (odzyskanie XOR tekstów i klucza uwierzytelniającego).

## Wady praktyczne OTP

| Problem | Opis |
| :--- | :--- |
| **Długość klucza** | klucz tak długi jak wiadomość – trzeba go wcześniej bezpiecznie przekazać (**problem dystrybucji klucza przenosi się**: skoro potrafimy bezpiecznie przekazać klucz, to po co nie przekazać samej wiadomości?) |
| **Prawdziwa losowość** | klucz musi pochodzić z **prawdziwego** źródła losowego (nie z PRNG – wtedy to szyfr strumieniowy bez tajności doskonałej) |
| **Jednorazowość** | zarządzanie, niszczenie użytych fragmentów, synchronizacja stron |
| **Brak integralności** | podatny na modyfikację bez MAC |
| **Koszt** | duże ilości danych, ręczne zarządzanie |

Używany w ograniczonym zakresie: **gorąca linia Waszyngton–Moskwa** (historyczny), komunikacja wywiadowcza/dyplomatyczna wysokiego ryzyka; **QKD** (kryptografia kwantowa, temat 15) generuje klucze, które można użyć w OTP.

## Szyfry strumieniowe jako przybliżenie OTP

Zamiast prawdziwie losowego klucza stosuje się **krótki klucz + generator pseudolosowy (PRNG/szyfr strumieniowy)**, który rozwija go w długi strumień. Nie ma już tajności doskonałej (klucz krótszy od wiadomości), ale zachowuje się **bezpieczeństwo obliczeniowe** – ten sam wzór $c=m\oplus z$, lecz **bezpieczne tylko przy unikalnym nonce** dla każdej wiadomości.

## Podsumowanie

- **Tajność doskonała:** szyfrogram nie wnosi żadnej informacji o tekście jawnym, $P(M|C)=P(M)$; wymaga klucza **losowego**, **nie krótszego** niż wiadomość i użytego **raz**.
- **OTP:** $c=m\oplus k$; doskonale tajny (Shannon).
- Ponowne użycie klucza: $c_1\oplus c_2=m_1\oplus m_2$ – klucz znika, tekst zostaje odsłonięty (Venona).
- Wady: klucz długości wiadomości, dystrybucja, prawdziwa losowość, brak integralności → w praktyce stosuje się szyfry obliczeniowo bezpieczne.
