# Zasada Kerckhoffsa i jej znaczenie przy projektowaniu bezpiecznych systemów kryptograficznych

## Sformułowanie

**Zasada Kerckhoffsa** (Auguste Kerckhoffs, *La cryptographie militaire*, 1883): **bezpieczeństwo systemu kryptograficznego nie może zależeć od tajności algorytmu – jedyną tajemnicą powinien być klucz.** Przeciwnik zna wszystko o systemie (algorytm, implementację, protokół) z wyjątkiem klucza.

Wersja **Shannona** (tzw. *Shannon's maxim*): „*przeciwnik zna system*" – należy zakładać, że wróg zna używany system.

Oryginalnie Kerckhoffs podał 6 wymagań dla szyfrów wojskowych; najczęściej cytowane jest drugie: system **nie powinien wymagać tajności** i powinien móc wpaść w ręce wroga bez szkody. Pozostałe dotyczą m.in.: praktycznej nieodszyfrowywalności, łatwej zmiany klucza, przenośności, wygody obsługi, prostoty zasad.

## Dlaczego jest ważna

| Powód | Wyjaśnienie |
| :--- | :--- |
| **Algorytm i tak się wycieknie** | oprogramowanie można dezasemblować, urządzenia kupić i rozebrać, ludzie zdradzają; tajność algorytmu jest krucha i **jednorazowa** – po ujawnieniu cały system jest bezużyteczny |
| **Klucz łatwo wymienić** | po kompromitacji wystarczy zmienić klucz; zmiana algorytmu oznacza wymianę całego systemu |
| **Klucz jest małym, dobrze chronionym sekretem** | chronić kilkadziesiąt bajtów (np. w HSM) jest łatwiej niż utrzymać w tajemnicy duży system |
| **Weryfikacja przez społeczność** | otwarty algorytm może być **analizowany publicznie** przez kryptografów; wady są znajdowane i usuwane; zaufanie rośnie z latami odpornosci (konkursy AES, SHA-3) |
| **Standaryzacja i interoperacyjność** | wspólne standardy (AES, TLS) mogą być wdrażane przez wielu producentów |
| **Pojedyncza tajemnica do zarządzania** | prostszy model zagrożeń: bezpieczeństwo = bezpieczeństwo klucza |
| **Brak „fałszywego poczucia bezpieczeństwa"** | zachęca do projektowania z założeniem silnego przeciwnika |

## „Bezpieczeństwo przez zaciemnianie" (security through obscurity)

**Zaciemnianie** – poleganie na ukryciu działania systemu zamiast na jego wytrzymałości. Zasada Kerckhoffsa jest jego **zaprzeczeniem**. Takie tajne algorytmy własnej produkcji często okazywały się słabe, gdy tylko je poznano:

- **A5/1, A5/2** (GSM) – tajne szyfry strumieniowe, po wycieku i analizie złamane,
- **CSS** (DVD), **MIFARE Classic** (Crypto-1), **KeeLoq**, **RC4** (po ujawnieniu w 1994 r. odkryto stopniowo jego wady),
- **Dual_EC_DRBG** – generator z potencjalnym backdoorem w stałych (afera NSA).

Kontrprzykłady zaufania: **AES** (publiczny konkurs NIST 1997–2000, algorytm Rijndael), **SHA-3** (konkurs 2007–2012) – analizowane publicznie, bez znanych praktycznych ataków.

## Co wolno trzymać w tajemnicy

- **klucze** (prywatne, symetryczne), hasła, sekrety sesji,
- **parametry zmienne** i losowość (nonce nie musi być tajny, ale unikalny),

natomiast **algorytm, protokół, format, kod źródłowy** mogą być jawne bez uszczerbku. Dodatkowe zaciemnianie (np. ukrycie typu systemu) może być **dodatkową warstwą**, ale nigdy jedyną.

## Konsekwencje dla projektowania systemów

1. **Używać standardowych, publicznie zweryfikowanych algorytmów** (AES, SHA-2/3, RSA/ECC); **nie wymyślać własnych** („don't roll your own crypto" – Schneier: każdy potrafi stworzyć szyfr, którego sam nie umie złamać).
2. **Bezpieczeństwo skupić na kluczach:** generowanie z dobrego źródła losowości, bezpieczne przechowywanie (HSM, KMS), rotacja, kontrola dostępu, unieważnianie.
3. **Otwarte implementacje** (OpenSSL, libsodium) – audyty, programy bug bounty.
4. **Projektować z założeniem kompromitacji** (*assume breach*): wymienialne klucze, forward secrecy, krótkie czasy życia kluczy.
5. **Kryptoagilność** – możliwość wymiany algorytmu (np. przejście na algorytmy postkwantowe).
6. Protokoły i implementacje także poddawać weryfikacji (formalnej, fuzzing), bo słabość może tkwić w implementacji (kanały boczne).

## Ograniczenia i niuanse

- Zasada **nie zabrania** utrzymywania w tajemnicy szczegółów w pewnych sytuacjach (np. systemy wojskowe klasyfikowane – zestawy Suite A NSA), ale **nie można polegać wyłącznie na tym**.
- Dotyczy **algorytmów**, a nie danych: nie oznacza jawności wszystkiego (konfiguracja, dane wewnętrzne – mogą być tajne dla zasady minimalizacji).
- Otwarty kod **nie gwarantuje** bezpieczeństwa (np. Heartbleed w OpenSSL) – potrzebny rzeczywisty przegląd.

## Podsumowanie

- **Zasada Kerckhoffsa:** system musi być bezpieczny, nawet jeśli przeciwnik zna wszystko poza **kluczem**.
- Powody: algorytmy się ujawniają, klucze można wymienić, otwartość umożliwia publiczną weryfikację i standaryzację.
- Przeciwieństwo: **security through obscurity** – zawodzi (A5/1, CSS, Crypto-1).
- Praktyka: standardowe algorytmy, ochrona i zarządzanie kluczami, kryptoagilność.
