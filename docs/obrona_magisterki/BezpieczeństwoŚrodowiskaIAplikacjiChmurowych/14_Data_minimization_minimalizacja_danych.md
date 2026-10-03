# Co oznacza pojęcie Data minimization?

**Data minimization (minimalizacja danych)** to zasada, według której **zbieramy, przetwarzamy i przechowujemy tylko te dane, które są naprawdę potrzebne** do konkretnego, określonego celu, i **nie dłużej niż to konieczne**. Wynika z **RODO (art. 5 ust. 1 lit. c)**: dane muszą być „adekwatne, stosowne oraz ograniczone do tego, co niezbędne".

**Dlaczego to ważne w bezpieczeństwie chmury:** czego nie zebrano, tego nie da się wykraść ani ujawnić. Mniej danych oznacza **mniejszą powierzchnię ataku**, mniejsze skutki wycieku, niższe koszty przechowywania i łatwiejszą zgodność z prawem.

**Jak się to stosuje:**

- zbieranie tylko niezbędnych pól (np. bez numeru PESEL, gdy nie jest potrzebny),
- **określenie celu i okresu retencji**, a potem automatyczne usuwanie lub archiwizacja po jego upływie,
- **anonimizacja i pseudonimizacja**, **maskowanie** danych (np. w logach i środowiskach testowych),
- ograniczanie dostępu do danych osobowych (najmniejsze uprawnienia),
- **privacy by design** (ochrona prywatności już na etapie projektowania systemu),
- realizacja prawa do bycia zapomnianym (usuwanie danych na żądanie).

**Przykład:** sklep internetowy zbiera adres dostawy, ale nie wymaga daty urodzenia. Dane zamówień usuwa po upływie okresu wymaganego przez prawo.

## Podsumowanie

- **Data minimization** – zbieraj i przechowuj **tylko niezbędne dane** do konkretnych celów (RODO art. 5 ust. 1 lit. c).
- Wraz z **privacy by design** i **prawem do bycia zapomnianym** to kluczowe zasady GDPR w wykładzie.
- W chmurze szczególnie ważna ze względu na lokalizację danych, transfery międzynarodowe i łatwość replikacji.
- Realizacja: ograniczone pola, klasyfikacja, pseudonimizacja/anonimizacja, maskowanie w logach, retencja, szyfrowanie, kontrola dostępu.

---
[⬅️ Poprzedni temat](13_TDE_Transparent_Data_Encryption.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](15_OWASP_Top_10.md)