# Czym są tryby pracy szyfrów blokowych i dlaczego są potrzebne? Omów ogólnie znaczenie takich trybów jak ECB, CBC, CFB, OFB, CTR lub GCM.

> **💬 Gotowa wypowiedź ustna:**
> *"Tryb pracy szyfru blokowego to sposób stosowania takiego szyfru, na przykład AES, do danych dłuższych niż jeden blok. Jest potrzebny z dwóch powodów: sam szyfr szyfruje tylko jeden blok o stałej długości, a dane mają dowolny rozmiar. Poza tym, gdyby każdy blok szyfrować niezależnie tym samym kluczem, identyczne bloki tekstu dawałyby identyczne szyfrogramy, co ujawnia wzorce w danych. Tryb wprowadza więc powiązania między blokami lub losowość.
>
> - **ECB:** każdy blok szyfruje się osobno. Jest prosty i można go liczyć równolegle, ale ujawnia wzorce. Klasycznym przykładem jest obraz pingwina, którego kontury widać po zaszyfrowaniu. Tego trybu nie należy używać.
> - **CBC:** każdy blok tekstu jest przed zaszyfrowaniem łączony operacją XOR z poprzednim szyfrogramem, a pierwszy z losowym wektorem inicjującym IV, co ukrywa wzorce. Szyfrowanie jest sekwencyjne i wymaga dopełnienia ostatniego bloku, co czyni ten tryb podatnym na atak typu padding oracle.
> - **CFB i OFB:** zamieniają szyfr blokowy w strumieniowy, więc nie wymagają dopełnienia. W CFB strumień klucza zależy od poprzedniego szyfrogramu, a w OFB jest generowany niezależnie od danych.
> - **CTR:** szyfruje kolejne wartości licznika, utworzone z nonce i numeru bloku, a wynik łączy z tekstem operacją XOR. Jest w pełni równoległy i pozwala odszyfrować dowolny blok bez przetwarzania pozostałych. Nonce nie może się powtórzyć dla tego samego klucza.
> - **GCM:** łączy tryb CTR z uwierzytelnieniem, dzięki czemu zapewnia jednocześnie poufność i integralność. Jest szybki i jest standardem w TLS. Powtórzenie nonce przy tym samym kluczu jest w nim katastrofalne, bo ujawnia dane i klucz uwierzytelniający.
>
> Warto pamiętać, że tryby inne niż GCM chronią tylko poufność, a nie integralność, więc zmodyfikowany szyfrogram da się wykryć dopiero po dodaniu kodu MAC. Dlatego w praktyce zaleca się AES-GCM albo ChaCha20-Poly1305."*

**Tryb pracy szyfru blokowego** to sposób, w jaki szyfr blokowy (np. AES, który szyfruje jeden blok 128 bitów) jest stosowany do danych dłuższych niż jeden blok. Są potrzebne z dwóch powodów:

- Sam szyfr szyfruje tylko jeden blok, a dane mają dowolną długość.
- Gdyby każdy blok szyfrować niezależnie tym samym kluczem, **identyczne bloki tekstu dawałyby identyczne szyfrogramy**, co ujawnia wzorce. Tryb wprowadza powiązania między blokami lub losowość.

**Omówienie trybów:**

- **ECB:** każdy blok szyfrowany osobno. Jest prosty i równoległy, ale **ujawnia wzorce** (znany przykład obrazka pingwina, którego kontury widać po zaszyfrowaniu). **Nie należy go używać.**
- **CBC:** każdy blok tekstu jest przed szyfrowaniem XOR-owany z poprzednim szyfrogramem, a pierwszy z **losowym wektorem IV**. Ukrywa wzorce, ale szyfrowanie jest sekwencyjne i wymaga dopełnienia (padding). Jest podatny na atak *padding oracle*.
- **CFB i OFB:** zamieniają szyfr blokowy w **szyfr strumieniowy**, więc nie wymagają dopełnienia. W CFB strumień zależy od poprzedniego szyfrogramu, a w OFB generowany jest niezależnie od danych.
- **CTR:** szyfruje kolejne wartości licznika (nonce + numer bloku) i wynik XOR-uje z tekstem. Jest **w pełni równoległy** i pozwala na swobodny dostęp do dowolnego bloku. Nonce nie może się powtórzyć dla tego samego klucza.
- **GCM:** łączy CTR z uwierzytelnieniem (GHASH), więc daje **szyfrowanie uwierzytelnione (AEAD)**: poufność i integralność w jednym. Jest szybki i stosowany m.in. w TLS. **Powtórzenie nonce przy tym samym kluczu jest katastrofalne**, bo ujawnia dane i klucz uwierzytelniający.

## Porównanie

| Tryb | Rodzaj | IV / nonce | Padding | Równoległość (szyfr. / deszyfr.) | Propagacja błędu | Uwierzytelnienie |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **ECB** | blokowy | brak | tak | tak / tak | tylko blok | nie |
| **CBC** | blokowy | losowy IV | tak | **nie** / tak | blok + 1 bit w następnym | nie |
| **CFB** | strumień (samosynchr.) | unikalny IV | nie | nie / tak | bieżący + następny blok | nie |
| **OFB** | strumień (synchroniczny) | unikalny IV | nie | nie / nie | brak (bit→bit) | nie |
| **CTR** | strumień | unikalny nonce+licznik | nie | **tak / tak** | brak (bit→bit) | nie |
| **GCM** | strumień + MAC (AEAD) | **unikalny nonce** | nie | tak / tak | brak (tag wykryje) | **tak** |

**Ważna uwaga:** same tryby ECB, CBC, CFB, OFB i CTR zapewniają tylko poufność, **nie integralność**. Zmodyfikowany szyfrogram da się wykryć dopiero po dodaniu MAC albo przy użyciu AEAD, jak GCM.

**W praktyce** zaleca się **AES-GCM** (lub ChaCha20-Poly1305).

## Podsumowanie

- Tryby pracy pozwalają szyfrować **dowolnie długie dane** blokowym szyfrem i dają **bezpieczeństwo semantyczne** (ECB go nie ma).
- **ECB** – bloki niezależnie (ujawnia wzorce, nie używać); **CBC** – łańcuchowanie z losowym IV, padding, ryzyko padding oracle; **CFB/OFB/CTR** – szyfr blokowy jako strumieniowy; **CTR** – równoległy, nonce unikalny; **GCM** – CTR + GHASH = **AEAD** (poufność + integralność), standard w TLS, ale nonce nie wolno powtarzać.

---
[⬅️ Poprzedni temat](5_Szyfry_klasyczne_a_nowoczesne_algorytmy_kryptograficzne.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Rola_klucza_w_kryptografii_symetrycznej_i_asymetrycznej.md)