# Na czym polega zasada Kerckhoffsa i dlaczego jest ważna przy projektowaniu bezpiecznych systemów kryptograficznych?

**Zasada Kerckhoffsa** (XIX w.) mówi, że **bezpieczeństwo systemu kryptograficznego powinno zależeć wyłącznie od tajności klucza, a nie od tajności algorytmu**. Należy zakładać, że przeciwnik zna dokładnie sposób działania systemu, i mimo to nie powinien być w stanie go złamać bez klucza. Claude Shannon ujął to krócej: „wróg zna system".

**Dlaczego jest ważna:**

- **Klucz łatwo zmienić, algorytmu nie.** Gdy klucz wycieknie, generujemy nowy. Gdyby tajny był algorytm, trzeba by go zastąpić całym nowym systemem.
- **Tajność algorytmu jest nietrwała.** Można go odtworzyć przez inżynierię wsteczną, wyciek lub przekupienie pracownika. Tak było z szyframi z pominięciem tej zasady: A5/1 w GSM, CSS w DVD czy MIFARE Classic zostały złamane po ujawnieniu ich działania.
- **Jawny algorytm można badać.** Publiczna analiza przez wielu kryptologów szybko ujawnia słabości. Dlatego standardy (AES, SHA-3, algorytmy PQC) wybiera się w otwartych konkursach.
- **Ułatwia standaryzację i interoperacyjność.** Jeden powszechnie znany algorytm mogą stosować wszyscy, a sprzęt i oprogramowanie mogą go implementować.
- **Zmniejsza ilość sekretów.** Do ochrony jest tylko klucz (krótki), a nie cały projekt.

**Wniosek praktyczny:** nie projektujemy własnych, tajnych algorytmów („security through obscurity" zawodzi), tylko używamy sprawdzonych, publicznych, a całą uwagę poświęcamy **dobremu zarządzaniu kluczami**: losowemu generowaniu, długości, przechowywaniu i rotacji.

## Podsumowanie

- **Zasada Kerckhoffsa:** system musi być bezpieczny, nawet jeśli przeciwnik zna wszystko poza **kluczem**.
- Powody: algorytmy się ujawniają, klucze można wymienić, otwartość umożliwia publiczną weryfikację i standaryzację.
- Przeciwieństwo: **security through obscurity** – zawodzi (A5/1, CSS, Crypto-1).
- Praktyka: standardowe algorytmy, ochrona i zarządzanie kluczami, kryptoagilność.

---
[⬅️ Poprzedni temat](2_Klasyfikacja_systemów_kryptograficznych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Tajność_doskonała_i_szyfr_z_kluczem_jednorazowym.md)