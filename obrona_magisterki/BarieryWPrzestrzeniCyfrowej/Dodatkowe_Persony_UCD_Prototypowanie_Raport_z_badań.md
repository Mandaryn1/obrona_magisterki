# Materiały dodatkowe z wykładów (poza listą zagadnień)

> Treści z wykładów W3, W8 i W12, które **nie są wprost na liście zagadnień egzaminacyjnych**, ale pomagają zrozumieć pozostałe tematy (persony, UCD/UX, prototypowanie, adnotacje, raport z badań).

# Część A. Użytkownicy i persony (W3)

## Profil użytkownika

**Profil użytkownika** – zbiór **ról użytkownika** w systemie informatycznym, czyli zbiór **zadań**, które użytkownik może wykonać przy pomocy oprogramowania (kluczowe/wspomagające, mobilne/webowe itd.). Wyrażany jako **Aktor** (UML) lub **Persona** (UCD).

### Typy (poziomy) użytkowników

| Poziom | Potrzeby |
| :--- | :--- |
| **Początkujący** (nowicjusz) | wstępne wyjaśnienie, dodatkowi pomocnicy, **pomoc kontekstowa**, informacja zwrotna, mała liczba działań i koncepcji, przewodniki |
| **Średnio zaawansowany** (największa grupa) | **standard interfejsu i jego spójność**, ograniczenie możliwości popełnienia błędu, szczegółowa pomoc, indeks |
| **Ekspert** | **skróty**, szybkość dostępu, linie poleceń, **indywidualizacja** interfejsu |

## Persona

**Persona** – **egzemplifikacja użytkownika** w UCD: **fikcyjny przedstawiciel (obiekt) grupy użytkowników**; przeciwieństwo „średniego" użytkownika.

- **Osoba sztuczna, nieistniejąca**, model abstrakcyjny.
- Ma **imię i nazwisko**, **wygląd (zdjęcie)** i inne detale (wiek, płeć, pozycja socjalna, zawód), które ułatwiają zapamiętanie i odróżnienie od innych person; opis w przekrojach: **człowiek, cele, zachowanie**. Przykłady: *Pani Kasia z księgowości*, *Pan Zenek – majster*.

### Cele person

- Łatwiejsze **zapamiętywanie właściwości grup** użytkowników (uczłowieczenie fikcyjnego użytkownika).
- Ułatwienie **komunikacji w zespole** („ta funkcja jest dla Pani Kasi").
- Pozbycie się **paradoksu „typowego", uśrednionego użytkownika** – taki użytkownik łączy różne właściwości („słonio-żyrafo-orło-osioł") i **nie istnieje**.
- Połączenie w jedno charakterystyk z **trzech profili: użytkownika, zadaniowego i środowiska**.
- **Koncentracja na działaniach** użytkownika; zbieranie charakterystyk użytkowników, które mogą się przydać na późniejszych etapach analizy i projektowania.

**Szablon persony:** typ (podstawowa/drugorzędna/negatywna/…), imię i nazwisko, zdjęcie, tło (wiek, płeć, miejsce, zawód, poziom technologiczny), **cele** (praktyczne, osobiste, biznesowe), **frustracje i problemy**, scenariusze użycia, odniesienia.

W badaniach użyteczności persony służą do **doboru grup badawczych** (każda persona – osobna grupa 5–8 osób); w projektowaniu dostępnym warto uwzględniać persony z niepełnosprawnościami.

# Część B. Analiza wyników i raport z badań (W8)

## Dane eksperymentalne

Zapisy eyetrackera, zapisy **audio-wideo**, **notatki z wywiadów**, **ankiety**.

## Adnotacje (zapisy audio-wideo)

**Adnotacje** – akcje **oznaczone czasem**, powiązane z sekwencjami wideo, ułatwiające analizę; mają **kody (klasyfikatory)**; tworzenie jest **czasochłonne** (proces ręczny).

- Rezultat: **pliki tekstowe obserwacji** ze znacznikami czasu, np. `0:15 Użytkownik 3 rozpoczyna Zadanie 1`; `0:55 Użytkownik 3 klika „Zapisz"`; `1:23 Użytkownik 3 wyraził zdziwienie`.
- **Cele:** opis akcji ułatwiający analizę (obiektywnych – gest, czynność; subiektywnych – emocje, stres); **ilościowe określenie** akcji (czas, liczba); **automatyzacja** przetwarzania (klasyfikacja, zliczanie, uśrednianie); wydzielenie **wzorców** postępowań.
- **Typy akcji:** **zdarzenie** (w punkcie czasowym, bez czasu trwania, np. kliknięcie) i **działanie** (ma początek i koniec, np. rozpoczęcie/zakończenie zadania).
- **Klasyfikatory** powinny być **niezależne (ortogonalne)**, możliwe do uzupełnienia przez różne osoby, **predefiniowane, ale rozszerzalne**: użytkownik (U1, U2…), działanie (kliknięcie, wpisanie, wyszukiwanie, wycofanie), zadanie (Z1…), zachowanie werbalne (pytanie, negatywne, pozytywne, zaskoczenie), **typ błędu** (nawigacji, wyboru, wpisywania).
- **Struktura rekordu:** `czas_1 [czas_2] kod_użytkownika zadanie akcja [typ_błędu] [parametry]`, np. `1:15 n/a U1 Z2 klik n/a Wyślij`; obróbka np. w Excelu.
- **Schemat analizy:** cele i metryki → sposoby kodowania (klasyfikatory) → zbiory wartości klasyfikatorów → adnotacje → analiza i wartości metryk.
- **Notatki z wywiadów:** porządkowanie metodą adnotacji; analiza ilościowo-jakościowa.

## Analiza ankiet

Metoda SUS (temat 8); ankiety własne: **odrzucenie nieprawidłowo wypełnionych**, histogram, średnia, **mediana, odchylenie standardowe**, analiza **korelacji**.

## Rezultaty analizy i priorytety problemów

- Zestawienia: **czas realizacji zadań**, **liczba kliknięć**.
- **Tabela problemów użyteczności:** numer problemu, opis, u których uczestników wystąpił (1–8), **istotność**: **KRYTYCZNE** (np. użytkownik nie zauważył komunikatu o rozpoczęciu wysyłania zlecenia), **istotny**, **małoistotny** (np. użycie przycisku „wstecz" przeglądarki).
- **Szczegółowa analiza i rekomendacje:** *problem – istotność – opis – rekomendacja*. Przykład (krytyczny): wprowadzenie kwoty z przecinkiem – system ignoruje część dziesiętną **bez komunikatu**.
- Dodatkowo: zauważone błędy, sugestie, opinie użytkowników (wywiady, ankiety).

## Raport z badań – struktura wg ISO 25062

**ISO 25062** – standardowy **format raportów z testów użyteczności** (wskazówki, nie szablon): ułatwia zrozumienie, poprawia **spójność i kompletność**.

1. **Strona tytułowa**.
2. **Streszczenie dla kierownictwa**.
3. **Wprowadzenie**: opis produktu, cele testów (mogą być powiązane z miarami **ROI**).
4. **Metodyka**: uczestnicy (profile, persony, dobór); **kontekst użycia** (scenariusze zadań i uzasadnienie, kryteria i wskaźniki wykonania, miejsce, środowisko komputerowe, narzędzia administratora); **plan eksperymentu** (procedura: metryki, zmienne niezależne i kontrolne, instrukcje, cały proces od wejścia do wyjścia z laboratorium); **metryki**: wydajność i efektywność (min. stopień ukończenia i czas; opcjonalnie błędy, efektywność nawigacji), **satysfakcja** (SUS lub inne).
5. **Rezultaty**: tabela wyników dla każdego zadania, wykresy, priorytetyzacja problemów, zalecenia, **globalna miara użyteczności**.

# Część C. UCD, UX i projektowanie interfejsu (W12)

## UCD – projektowanie ukierunkowane na użytkownika

**UCD (User-Centered Design)** – podejście, w którym **użytkownik jest w centrum całego procesu**. Fazy:

1. **analiza ukierunkowana na użytkownika** (UCA),
2. **projektowanie ukierunkowane na użytkownika** (UCD),
3. **implementacja** (zwykle budowa prototypu),
4. **testowanie użyteczności** (UT) – ocena jakości prototypu.

Model **spiralny** (cykliczne uszczegóławianie). Produkty: wymagania → projekt → prototyp → rezultaty testów.

### Kluczowe czynniki (standard ISO – ISO 9241-210)

- proces oparty na **dogłębnym zrozumieniu** użytkowników, celów, zadań i środowiska,
- użytkownicy **aktywnie zaangażowani** w cały proces,
- projektowanie **sterowane opiniami i ocenami** użytkowników,
- proces **cykliczny**,
- wykorzystanie **wszystkich wrażeń użytkowników (UX)**,
- **interdyscyplinarny zespół**.

### UCD w modelu kaskadowym

- **Faza 1 – projekt ogólny** (z UX): specyfikacja wymagań → koncepcja → struktura aplikacji → **prototyp interfejsu** → badania prototypu → raport z badań z użytkownikami → dokumentacja funkcjonalna interfejsu.
- **Faza 2 – projekt graficzny** (grafik): weryfikacja dokumentacji, zweryfikowane projekty graficzne – podstawa do implementacji.
- **Wady:** pracochłonna dokumentacja; ryzyko utraty spójności koncepcji (rozdzielenie faz); szybka dezaktualizacja dokumentacji; kolejni wykonawcy nie czują się współautorami.

## Metody projektowania interfejsu

| Metoda | Opis |
| :--- | :--- |
| **Czarnoksiężnik z Oz** (*Wizard of Oz*) | symulacja interfejsu: użytkownicy „obsługują system", a odpowiedzi generuje **ukryty operator**; użytkownicy nie wiedzą, czy odpowiada człowiek czy system; sesje cykliczne ze wspólną analizą; stosowana we **wczesnych fazach ustalania wymagań**; wady: dobór ludzi, koszty, trudności w analizie GUI (nadaje się do prostych interfejsów) |
| **Prototypowanie** | zob. niżej |
| **Storyboardy** | graficzny szkic nawigacji, „scenariusz filmowy w obrazkach" (szczególny przypadek diagramu nawigacji); papier lub **karteczki post-it** |
| **Standardy i zalecenia** | formalne: **ISO 9241** (110 – zasady dialogu, 210 – projektowanie ukierunkowane na człowieka); W3C (WCAG, WAI-ARIA, UAAG); zalecenia platform (iOS HIG, Android Design, GNOME HIG itd.); zbiory ikon |
| **Metody formalne** | **UIDL** (UIML, UsiXML, XAML, XUL) i projektowanie sterowane modelami (model abstrakcyjny → konkretny → końcowy interfejs); **rzadko stosowane** (duża komplikacja) |
| **Narzędzia wspomagające (CAID)** | sortowanie kart (OptimalSort, UserZoom, xSort, UXSort…), planowanie zadań (drzewa zadań CTT, narzędzie CTTE), tworzenie mockupów (Balsamiq, Axure, Moqups, MS Visio, Justinmind…) |

## Prototypowanie

### Typy

- **małej dokładności** (*low-fidelity*): tanie, wiele wariantów, ograniczone wykrywanie błędów i odwzorowanie nawigacji,
- **wysokiej dokładności** (*high-fidelity*): w pełni interaktywne, wyglądają jak produkt, ale drogie, dłuższy czas realizacji, kosztowne zmiany.

Prototyp = kompromis (koszt vs jakość, niska vs wysoka dokładność); **szeroki (poziomy)** – wiele funkcji; **głęboki (pionowy)** – dużo szczegółów.

### Techniki

| Technika | Opis |
| :--- | :--- |
| **Szkice** (*sketches*) | na papierze/tablicy (potem fotografia); projektowanie z użytkownikiem, ręczna symulacja; **szybko, tanio**, dowolność |
| **Makiety** (*wireframes*) | szkice struktury ekranów i nawigacji (nagłówek, pasek stanu, menu, pole robocze, stopka); **ujednolicają interfejs**, pokazują standard graficzny; także papierowe |
| **Atrapy** (*mockups*) | **komputerowe, interaktywne modele** interfejsu: projektowanie, ocena, demonstracja, szkolenia; realizacja w Visio, PowerPoint, HTML, szybkim programowaniu (RAD) |
| **Działające (dynamiczne) prototypy** | najbliższe produktowi finalnemu |
| **Przepływ nawigacji** | diagram pokazujący dynamikę interfejsu |

## Powiązania

UCD i prototypy powstają **przed** badaniami z tematów 6–10; persony służą do doboru uczestników; wyniki testów (SUS, eyetracking) wracają do kolejnych iteracji projektu.
