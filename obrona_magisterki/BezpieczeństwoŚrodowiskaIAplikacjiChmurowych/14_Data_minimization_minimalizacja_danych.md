# Data minimization (minimalizacja danych)

## Definicja

**Data minimization (minimalizacja danych)** to zasada ochrony danych osobowych, zgodnie z którą organizacje powinny **zbierać i przetwarzać tylko te dane, które są niezbędne do realizacji określonych celów** (slajd 35). Dane mają być **adekwatne, stosowne i ograniczone do tego, co niezbędne** w stosunku do celów przetwarzania (**RODO/GDPR, art. 5 ust. 1 lit. c**).

Zasada ta wynika z logiki: **dane, których nie zebrano, nie mogą wyciec**. Im mniej danych, tym mniejsze ryzyko, koszty i obowiązki.

## Miejsce w RODO (GDPR) – wykład, slajd 35

**GDPR (RODO)** reguluje przetwarzanie danych osobowych w UE. Podstawowe zasady wymienione w wykładzie:

| Zasada | Znaczenie |
| :--- | :--- |
| **Privacy by design** | ochrona prywatności uwzględniana **na każdym etapie projektowania** systemów (art. 25 – uwzględnianie ochrony danych w fazie projektowania i domyślna ochrona danych) |
| **Data minimization** | zbierać tylko **dane niezbędne** do określonych celów |
| **Right to be forgotten** | prawo do **usunięcia** danych osobowych („prawo do bycia zapomnianym", art. 17) |

*(uzupełnienie)* Powiązane zasady art. 5 RODO: **ograniczenie celu** (dane zbierane w konkretnych, uprawnionych celach), **ograniczenie przechowywania** (nie dłużej niż potrzeba), **prawidłowość**, **integralność i poufność**, **rozliczalność**; art. 32 – odpowiednie środki techniczne i organizacyjne (pseudonimizacja, szyfrowanie).

## Minimalizacja w chmurze

Wykład podkreśla, że **GDPR w chmurze wymaga szczególnej uwagi na lokalizację danych i transfery międzynarodowe** – dane osobowe muszą być przetwarzane zgodnie z wymaganiami niezależnie od miejsca przechowywania. Minimalizacja danych zmniejsza ten problem: mniej danych w chmurze = mniej ryzyka naruszeń i prostsza zgodność.

Dodatkowo w chmurze: współdzielona infrastruktura, łatwość replikacji i kopiowania danych (kopie w wielu regionach, backupy, logi), dane dostępne przez API – **ryzyko „rozlewania się" danych** (data sprawl) i trudność ich usunięcia w każdym miejscu.

## Jak wdrażać minimalizację (praktyka)

### 1. Na etapie zbierania

- zbierać **tylko pola potrzebne** do celu (np. do dostawy – adres, nie data urodzenia),
- unikać pól „na zapas", opcjonalne pola oznaczać jako dobrowolne,
- projektować formularze i API z minimalnymi odpowiedziami (**DTO** zamiast encji z wszystkimi polami; zwracać tylko potrzebne atrybuty).

### 2. Na etapie przetwarzania i przechowywania

- **klasyfikacja danych** (publiczne, wewnętrzne, poufne, ściśle tajne – slajd 21) i dopasowanie ochrony,
- **pseudonimizacja** (zastąpienie identyfikatora kluczem, dane identyfikujące osobno) i **anonimizacja** (trwałe usunięcie możliwości identyfikacji; wtedy RODO nie dotyczy),
- **agregacja** i uogólnianie danych do analityki,
- **maskowanie i redakcja danych wrażliwych w logach** (slajd 189: *„Dane PII – minimalizacja i szyfrowanie w spoczynku"*, *„redakcja danych wrażliwych w logach"*),
- **szyfrowanie** (spoczynek/tranzyt) i **kontrola dostępu** – tylko niezbędne osoby i systemy (najmniejsze uprawnienia).

### 3. Na etapie retencji i usuwania

- **polityki retencji** – automatyczne usuwanie/archiwizacja po upływie okresu (np. reguły cyklu życia w bucketach, TTL w bazie),
- obsługa **prawa do usunięcia** – także w kopiach zapasowych, logach, cache, replikach i systemach pochodnych,
- **audyt** (kto, co czytał/modyfikował – logi aplikacji i bazy; wykład przypomina o **kosztach i RODO** takiego logowania, slajd 223).

### 4. Narzędzia i techniki

- **DLP** (Data Loss Prevention) i automatyczne wykrywanie danych wrażliwych (slajd 21, 25),
- **tokenizacja** (numerów kart – PCI DSS), **szyfrowanie kolumnowe**,
- **Row Level Security** – użytkownik widzi tylko swoje wiersze (slajd 299),
- **ocena skutków dla ochrony danych (DPIA)** i rejestr czynności przetwarzania *(uzupełnienie)*.

## Przykład

Katalog produktów z kontami użytkowników:

| Przed | Po minimalizacji |
| :--- | :--- |
| rejestracja wymaga: imię, nazwisko, PESEL, data urodzenia, adres, telefon | tylko **e-mail i hasło** (adres/telefon dopiero przy zamówieniu) |
| endpoint `/users/{id}` zwraca całą encję | zwraca tylko pola potrzebne klientowi (DTO) |
| logi zawierają pełny adres e-mail i token | w logach: zanonimizowany identyfikator (`sub`), tokeny zamaskowane |
| dane nieaktywnych kont przechowywane bezterminowo | automatyczne usunięcie po 24 miesiącach nieaktywności |

## Korzyści

- mniejsza **powierzchnia ataku** i skutki wycieku,
- **zgodność** z RODO i innymi regulacjami (HIPAA, PCI DSS),
- niższe **koszty** (przechowywanie, backup, bezpieczeństwo),
- łatwiejsza realizacja praw osób, których dane dotyczą,
- większe **zaufanie** klientów.

## Podsumowanie

- **Data minimization** – zbieraj i przechowuj **tylko niezbędne dane** do konkretnych celów (RODO art. 5 ust. 1 lit. c).
- Wraz z **privacy by design** i **prawem do bycia zapomnianym** to kluczowe zasady GDPR w wykładzie.
- W chmurze szczególnie ważna ze względu na lokalizację danych, transfery międzynarodowe i łatwość replikacji.
- Realizacja: ograniczone pola, klasyfikacja, pseudonimizacja/anonimizacja, maskowanie w logach, retencja, szyfrowanie, kontrola dostępu.

---
[⬅️ Poprzedni temat](13_TDE_Transparent_Data_Encryption.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](15_OWASP_Top_10.md)