# Okulografia (eyetracking) – idea, istota, urządzenia, eksperyment, rezultaty

## Definicja

**Okulografia** (*eye tracking*, eye-tracking, eyetracking) – **technika rejestracji aktywności wzrokowej** przez **śledzenie ruchów gałek ocznych**: **pomiar, rejestracja i analiza położenia i ruchu oczu**. Eyetrackery (okulografy) pozwalają stwierdzić, **gdzie, jak długo i w jakiej kolejności** kierowany jest wzrok osoby badanej.

Okulografia dostarcza informacji o **aktywności wzroku, a NIE o zrozumieniu** postrzeganej informacji. Pozwala zbadać **postrzeganie** obiektu: na czym człowiek skupia wzrok i które obszary całkowicie pomija.

**Zakres zastosowań:** marketing i reklama, **ergonomia systemów informatycznych**, badania kierowców i pilotów, medycyna i psychologia, wspomaganie osób niepełnosprawnych (sterowanie wzrokiem), **HCI**, edukacja, sport i rozrywka, architektura i sztuka.

## Idea i istota

- **Ruchy oczu** – najczęstsze intencjonalne zachowanie człowieka: ok. **3 razy na sekundę**.
- Śledzenie ruchu oka umożliwia podążanie **ścieżką uwagi** obserwatora: co uznał za interesujące, co przyciągnęło jego uwagę i jak postrzegał scenę; daje dostęp do procesów poznawczych i zachowania.
- Dostarcza **ilościowych danych pomiarowych bez odwoływania się do subiektywnych, werbalnych reakcji** badanego.
- Pokazuje: które elementy sceny i **jak długo** przyciągają uwagę; treści najdłużej skupiające uwagę i takie, które **nie wywołują reakcji**; elementy, **do których wzrok wraca** (zainteresowanie); **kierunek skanowania**; czy badany był zdezorientowany czy zainteresowany.
- Hipoteza **Eye-Mind** (Just i Carpenter, 1980): **czas fiksacji = czas przetwarzania** informacji (natychmiastowość przejścia informacji od oka do umysłu).

### Rys historyczny (z wykładu)

| Rok | Wydarzenie |
| :-: | :--- |
| 1879 | **L. E. Javal** – czytanie ma charakter skokowy; pojęcia **fiksacji i sakad** |
| 1897/1898 | **E. B. Huey** – pierwsze urządzenie do rejestracji ruchów oka (nasadka mocowana do oka; inwazyjne, mało dokładne); odkrycie, że informacje rejestruje się tylko podczas **fiksacji**, nie sakad |
| 1901 | **Dodge i Cline** – metoda **fotoelektryczna** (światło odbite od gałki ocznej) |
| 1935 | **G. T. Buswell** – „How people look at pictures" |
| 1950–60 | **A. Ł. Yarbus** – proces poznawczy zależy od **celu** patrzenia (ruchy oczu nie są przypadkowe) |
| 1963 | **D. A. Robinson** – soczewki kontaktowe z cewką, indukcja elektromagnetyczna (systemy cewkowe) |
| 1970 | **E. Schott** – **elektrookulografia** (różnica potencjałów elektrycznych) |
| 1980 | **Just i Carpenter** – hipoteza Eye-Mind |
| 1984 | **S. K. Card** – pierwsze testy użyteczności z eyetrackerem (wyszukiwanie komend w menu) |
| 1990 | **wideo-eyetracking**: środek źrenicy + odbicie rogówkowe (podczerwień) |

## Zasada działania – wideo-eyetracking

- **Dioda emitująca podczerwień** oświetla oko; **kamera wideo** rejestruje obraz oczu; oprogramowanie wyznacza **kierunek patrzenia**.
- **Oświetlenie podczerwone** zwiększa **kontrast między tęczówką a źrenicą** i tworzy charakterystyczny jasny punkt – **glint** (odbicie IR na rogówce).
- **Efekt jasnej i ciemnej źrenicy** zależy od położenia oświetlacza: **jasna źrenica** – oświetlacz blisko osi optycznej kamery (światło odbija się od siatkówki, jak „czerwone oczy"); **ciemna źrenica** – oświetlacz oddalony od osi.
- **Wyznaczenie punktu fiksacji:** **glint** jest punktem odniesienia niezmieniającym położenia przy ruchach gałki; **środek źrenicy** zmienia położenie; analiza ich **wzajemnego położenia** wyznacza punkt patrzenia (po **kalibracji**).

## Stany i ruchy oka

| Stan | Opis | Parametry |
| :--- | :--- | :--- |
| **Fiksacja** | **bezruch oka**, podczas którego następuje **przyjmowanie informacji** i planowanie następnego ruchu | czas od ok. **100 ms do 1500 ms**, typowo **200–600 ms**; częstość ok. 3 Hz; złożona z mikroruchów |
| **Sakada** | **szybki, skokowy, balistyczny** ruch obu oczu w tym samym kierunku; **przedziela fiksacje**; w czasie sakady przyjmowanie informacji jest **ograniczone** | czas **40–120 ms**, prędkość kątowa ok. 600°/s; maks. prędkość proporcjonalna do amplitudy |
| **Płynny pościg** | płynne śledzenie poruszającego się obiektu | |
| **Mikroruchy** | mikrosakady, dryft, drżenie | rejestrowane przy wysokiej częstotliwości próbkowania |
| **Mrugnięcia** | źródło **szumu**; skorelowane z **sennością** (rośnie okres); więcej mrugnięć przy dużym wysiłku poznawczym, mniej przy „uważnym przyglądaniu się" | wykorzystywane m.in. w badaniach reakcji emocjonalnych |
| **Rozszerzanie źrenicy** | zmiana wielkości otworu źrenicy – utrzymanie poziomu światła; świadczy też o **pobudzeniu** (obciążenie poznawcze, stres, emocje) | wymaga **rygorystycznej kontroli oświetlenia** |
| Inne | ruchy wergencyjne, odruch optokinetyczny, przedsionkowo-oczny, oczopląs poobrotowy | |

## Urządzenia

### Rodzaje eyetrackerów

| Typ | Charakterystyka | Zalety / wady |
| :--- | :--- | :--- |
| **Systemy zdalne** (bezkontaktowe) | wbudowane w monitor, pod monitorem lub obok; **kompensacja ruchów głowy** po kalibracji | brak kabli i unieruchamiania podbródka; komfort; typowe w badaniach interfejsów komputerowych |
| **Systemy nagłowne (mobilne)** | **okulary** z wbudowanym eyetrackerem i małym rejestratorem; **kamera sceny** rejestruje przestrzeń przed badanym; stałe położenie względem oczu | eliminują artefakty ruchów głowy; badania w terenie (aplikacje mobilne, kierowcy, wnętrza) |
| **Systemy montowane na wieży** (tower mounted) | podbródek i czoło unieruchomione | **bardzo duża dokładność 0,25–0,50°**, częstotliwość **1000–2000 Hz** (mikrosakady); **niewygodne**, nienaturalne, zwłaszcza przy długich sesjach |

### Parametry techniczne

**częstotliwość próbkowania** [Hz], **dokładność** [°], **swoboda ruchu głowy**, typ źrenicy (jasna/ciemna), kalibracja (5, 9, 16 punktów), obuoczne/jednooczne.

### Przykłady urządzeń (z wykładu)

| Urządzenie | Typ | Parametry |
| :--- | :--- | :--- |
| **Tobii TX300** | zdalny, obuoczny, zintegrowany z monitorem 23" | 300 Hz; ciemna źrenica; dokładność 0,4–0,8°; swoboda głowy 35,6 × 17,8 cm; kamera rejestrująca twarz |
| **Gazepoint GP3 HD** | zdalny, obuoczny | 150 Hz (60 Hz); jasna źrenica; dokładność 0,5–1,0°; kalibracja 5/9-punktowa; mocowanie do laptopa lub monitora ≤26" |
| **Pupil Invisible** | nagłowny (okulary wideo), obuoczny | 200 Hz; ciemna źrenica; dokładność 0,6°; kamera sceny 1088×1080 (82×82°); rejestracja na smartfonie |

### Oprogramowanie

- **Tobii Studio**: zarządzanie projektami, **projektowanie eksperymentu**, kalibracja, wiele rodzajów bodźców, rejestracja sesji, odtwarzanie i edycja nagrań, nagrywanie wywiadu, **wizualizacja**, statystyki, eksport surowych danych.
- **iMotions**: zgodny z niemal wszystkimi eyetrackerami; **integruje i synchronizuje czujniki biometryczne** (EDA/GSR, EEG, EKG, analiza mimiki), otwarte API.
- Wspólne funkcje: projektowanie eksperymentu, kalibracja i nagrywanie, odtwarzanie z nałożonym ruchem uwagi, **ścieżki wzroku i mapy termiczne**, łączenie danych wielu użytkowników, analiza statystyczna, eksport (xls, tsv – do Excel, SPSS, Statistica, Matlab).

## Eksperyment eyetrackingowy

### Struktura procesu badawczego

1. Przygotowanie: **problem badawczy, cele, hipotezy**.
2. **Projekt eksperymentu**: zadania dla respondentów, dobór próby, organizacja stanowiska.
3. **Pilotaż**.
4. Harmonogram badań.
5. **Realizacja** – nagrywanie uczestników.
6. **Analiza** materiału, testowanie hipotez, uogólnianie.
7. **Wnioski i rekomendacje**, raport.

### Projektowanie eksperymentu (w Tobii Studio)

- **Bodźce:** instrukcja, grafika, film, **strona internetowa**, aplikacja, obraz z zewnętrznej kamery, kwestionariusz; ułożone na **osi czasu** (kolejność, losowanie, różne media różnym uczestnikom); wiele testów w jednym projekcie.
- **Realistyczne zadania:** największy typowy błąd to badania **bez realistycznych zadań** („przejrzyj stronę i powiedz, co o niej myślisz") – dają bezwartościowe dane. Ludzie wykazują podobne wzorce patrzenia, gdy mają to samo zadanie i podobne doświadczenie.
- **Dobór próby:** analiza **jakościowa** – **5–10 osób**; **ilościowa** – **powyżej 30**; badania zachowań konsumenckich – 10–15. **Straty danych** nawet do **25%**; wysoki koszt respondentów; kryteria demograficzne, wady wzroku (np. jaskra); prośba o rezygnację z **makijażu oczu** (sztuczne rzęsy, maskara zafałszowują wyniki).

### Stanowisko badawcze

- Identyczne warunki dla każdego respondenta; **wyciszone pomieszczenie**, ograniczone światło słoneczne (nie na eyetracker i twarz badanego); wskazane **świetlówki** (niewiele podczerwieni).
- **Dwa ekrany:** dla respondenta (bodźce) i dla operatora.
- Wyłączyć oprogramowanie zużywające zasoby (wygaszacze, wyskakujące okienka, antywirusy); **odłączyć komputer od internetu** lub zapewnić stałą prędkość łącza.
- Stabilne biurko (brak wibracji), mysz i klawiatura na niezależnej powierzchni, **krzesło regulowane** (nieobrotowe).

### Przebieg badania

1. **Poinformowanie** uczestnika o celu i przebiegu, **zgoda** na udział, dane metrykalne.
2. Prawidłowe **ustawienie** uczestnika (odległość od ekranu, pozycja głowy).
3. Uruchomienie eksperymentu.
4. **Kalibracja.**
5. Badanie: kolejne instrukcje i plansze, **rejestracja** sesji.
6. Zakończenie i poinformowanie uczestnika o wynikach.

**Kalibracja:** eliminuje wpływ różnic w **kształcie i wielkości gałki ocznej**, w **załamywaniu światła** na rogówce i między **osią optyczną a wzrokową**. Uczestnik skupia wzrok na punktach pojawiających się na ekranie (**5, 9 lub 16**). Przy pominiętym punkcie – rekalibracja; przy dużych błędach – sprawdzić ustawienie eyetrackera, okularów, pozycję uczestnika.

**Pilotaż:** weryfikacja scenariuszy, aparatury, jakości i czytelności materiałów, zrozumiałości instrukcji, czasu ekspozycji i długości eksperymentu; korekta konfiguracji.

**Czynniki zakłócające:** niezrozumiała instrukcja; **zbyt długa sesja** (pomiar z przygotowaniem ≤ **1,5 godz.**); brak ustalonego czasu ekspozycji (badany sam przełącza ekrany); niestabilna pozycja głowy, podpieranie brody, gestykulacja; krzesło obrotowe; **zbyt duża interakcja moderatora** (dialog zmienia zachowanie); zasłonięcie gałki (powieka, śmiech), makijaż, okulary (zwłaszcza dwuogniskowe), soczewki, suche oczy.

## Analiza i wizualizacja danych

- **Odtwarzanie i edycja nagrań**, kodowanie zdarzeń, nagrywanie wywiadu.
- **RTA (Retrospective Think Aloud):** po teście badacz przeprowadza **wywiad**, w którym uczestnik wyjaśnia swoje zachowanie (może oglądać nagranie z mapami ciepła/ścieżkami).
- **AOI (Area of Interest)** – wydzielenie obszarów zainteresowania (półprzezroczyste nakładki, statyczne i dynamiczne; można grupować); metryki: moment pierwszej fiksacji, czas skupienia, liczba fiksacji w obszarze.

### Wizualizacje

| Wizualizacja | Opis |
| :--- | :--- |
| **Ścieżka fiksacji** (scan path, gaze plot) | **kolejność i położenie fiksacji**; **rozmiar kropki = czas trwania**, **liczba = kolejność**; wzorzec patrzenia jednego lub kilku uczestników; odmiana „**rój pszczół**" (bee swarm) – film z punktami wzroku |
| **Mapa cieplna** (heat map) | **zgrupowane fiksacje wielu użytkowników**; **kolory** ilustrują liczbę fiksacji lub czas; typy: **count** (liczba fiksacji), **absolute duration** (łączny czas), **relative duration** (czas fiksacji / czas ekspozycji bodźca) |
| **Mapa uwagowa** (focus map) | pokazuje tylko obszary, na które padał wzrok; **pomijane miejsca zaciemnione** |
| **Klaster** | obszary dużej koncentracji punktów spojrzenia (count, absolute / relative duration); mogą generować AOI automatycznie |

### Metryki eyetrackingowe

| Metryka | Interpretacja |
| :--- | :--- |
| **liczba fiksacji** | negatywnie skorelowana z **efektywnością poszukiwania wzrokowego**; dobra organizacja interfejsu zmniejsza liczbę fiksacji |
| **czas trwania fiksacji** (całkowity/średni) | im dłuższy, tym **trudniejsze zadanie** wzrokowe i **głębsze przetwarzanie** |
| **liczba sakad** | związana z **organizacją przestrzenną** informacji; częste przeskoki = niejasna organizacja |
| **długość i czas ścieżki skanowania** | im krótsza, tym lepiej zrealizowano wyszukiwanie; idealna ścieżka – proste linie między potrzebnymi elementami; **produktywność wyszukiwania wzrokowego** = rzeczywista długość / długość idealna |
| **średnia amplituda sakad** | różnicuje **ekspertów i nowicjuszy** |
| **powłoka wypukła ścieżki** | pole wielokąta wypukłego obejmującego wszystkie punkty ścieżki |
| **wskaźnik gęstości przestrzennej** | liczba komórek siatki zajętych przez ścieżkę / liczba wszystkich komórek; im bardziej skoncentrowane ścieżki, tym łatwiejszy interfejs |
| **gęstość przejść** | stosunek pełnych komórek macierzy przejść między AOI do wszystkich; więcej nawrotów = większe trudności z identyfikacją |
| **AOI: % fiksacji i czas** | zainteresowanie obszarem; zależne od ważności obszaru |
| **średnica źrenicy** | niezależna od świadomości; mierzy **subiektywne nastawienie** (nie kierunek: efekt bodźców pozytywnych i negatywnych podobny) oraz obciążenie pamięci krótkotrwałej |

Na obliczenia metryk wpływają ustawienia i typ **filtrów fiksacji**.

### Eksport

Dane (xls, tsv) do Excela, SPSS, Statistica, Matlab. Uwaga: Excel ma limit wierszy i działa wolno przy dużych zbiorach.

## Okulografia w badaniach jakości interfejsów

Eyetracking pozwala badać wszystkie trzy wymiary użyteczności wg **ISO 9241**:

- **skuteczność** – binarny wskaźnik realizacji zadania,
- **efektywność** – realizacja zadania w wymiarze czasowym/przestrzennym (czas, ścieżki),
- **satysfakcja** – np. różnice w wielkości źrenic.

Sprawdza m.in.: czytelność elementów, przyczyny **rozproszenia uwagi**, przyczyny niemożności znalezienia elementów, rozbieżność między **oczekiwaniami** a faktycznym położeniem obiektów, **kolejność skanowania** (czy rozmieszczenie jest prawidłowe), które elementy są najczęściej oglądane, jak **rozmiar i rozmieszczenie** elementów wpływają na zauważenie. Użytkownicy oczekują, że najważniejsze informacje będą w **miejscach priorytetowych**.

## Przykłady badań (rezultaty)

### Interfejsy komputerowe

- **Goldberg i Kotval:** porównanie interfejsu z **logicznie pogrupowanymi** przyciskami z interfejsem z **losowo rozmieszczonymi**; wymiary czasowy i przestrzenny; **istotne różnice wszystkich wskaźników** na korzyść logicznego grupowania.
- **Aplikacja do diagnozy niepłynności mowy:** analiza spektrogramów; eyetracking wykazał różnice w analizie wzrokowej **ekspertów** i **logopedów z małym doświadczeniem**; poszukiwanie wzorców skutecznego wyszukiwania niepłynności.

### Serwisy internetowe

- **Serwisy BIP** (Politechniki Lubelskiej oparty na CMS SSDIP vs prototyp PAD CMS): 10 takich samych stron; zadania dotyczyły elementów związanych z **dostępnością** (wyszukiwanie, czytelność menu, powiększenie czcionki, zrozumienie ikon); analiza **ilościowa** (czasy) i **jakościowa** (mapy cieplne, ścieżki).
- **Kontrast i czytelność:** niski kontrast tekstu i tła powoduje „błądzenie" wzroku w poszukiwaniu linku do rejestracji; zielona, niewyraźna czcionka na niebieskim tle.
- **Formularze:** **etykiety nad polami** pozwalają wykonać o **połowę mniej fiksacji** niż etykiety obok; etykiety po prawej stronie pola dezorientują; etykieta + podpowiedź = podwójne czytanie; brak instrukcji dla pól, rozpraszające okna dialogowe, brak przekierowania po rejestracji.
- **Wyszukiwarka i koszyk:** powinny być znalezione w **≤10 fiksacjach**; użytkownicy szukają koszyka w **prawym górnym rogu**.
- **Obrazy:** dobre obrazy wyjaśniają i budują emocje, złe są ignorowane i zwiększają obciążenie poznawcze; większa uwaga na obrazach powiązanych z tekstem; ludzie czytają podpisy przy zdjęciach.
- **Nawigacja:** link to obietnica; wskaźniki stanu nie powinny wyglądać jak przyciski.

### Interfejsy naturalne i przestrzeń

- **Gra „Zabytki architektoniczne Lublina"** (plansza z mapą, modele 3D, system rozpoznawania): metoda **testów A/B**; przestrzenno-czasowe ścieżki i AOI; wskaźniki: **liczba fiksacji (FC)** i **całkowity czas fiksacji (TDF)** – analiza asymetrycznej planszy dla graczy po lewej i prawej stronie.
- **Badania kierowców:** eyetracker mobilny w warunkach rzeczywistych; wskaźnik – stosunek czasu uwagi na **reklamach** do czasu na sytuacji drogowej → wykrywanie niebezpiecznych odcinków dróg.
- **Projektowanie przestrzeni do nauki:** wizualizacje sal lekcyjnych; miary: czas i liczba fiksacji, **reakcja źrenicy**, samoopis emocji; stonowana kolorystyka i luźne rozmieszczenie ławek – reakcje najbardziej **pozytywne**; jasne, kontrastowe kolory – najbardziej **negatywne**.

## Zalety i ograniczenia

| Zalety | Ograniczenia |
| :--- | :--- |
| **obiektywne, ilościowe** dane o uwadze | pokazuje **gdzie** patrzy, nie **czy zrozumiał** |
| wykrywa problemy, których użytkownicy nie potrafią zwerbalizować | **koszt sprzętu** i oprogramowania, czasochłonna analiza |
| wizualizacje (mapy, ścieżki) łatwe do komunikowania | straty danych do 25%, wrażliwość na okulary, makijaż, ruchy głowy |
| bez wpływu subiektywnych deklaracji | wymaga **realistycznych zadań** i kontrolowanych warunków (oświetlenie, stanowisko) |
| wiele zastosowań (UX, dostępność, medycyna, marketing) | małe próby (jakościowo 5–10) – ograniczone uogólnianie; zalecane łączenie z RTA/ankietą |

## Podsumowanie

- **Okulografia** = rejestracja i analiza ruchów gałek ocznych; pokazuje **gdzie, jak długo i w jakiej kolejności** patrzy użytkownik (nie jego zrozumienia).
- **Istota:** fiksacje (200–600 ms, odbiór informacji) i sakady (40–120 ms) + pozostałe stany (mrugnięcia, źrenica); hipoteza Eye-Mind.
- **Urządzenia:** zdalne, nagłowne, na wieży; parametry (Hz, dokładność °); oprogramowanie Tobii Studio, iMotions; wideo-eyetracking: IR + glint + środek źrenicy.
- **Eksperyment:** cel i hipotezy → projekt (bodźce, osie czasu, **realistyczne zadania**) → pilotaż → zgoda i **kalibracja** (5/9/16 pkt) → rejestracja → analiza.
- **Rezultaty:** **ścieżki fiksacji, mapy cieplne, focus map, klastry, AOI** + metryki (liczba i czas fiksacji, sakady, długość ścieżki, źrenica); przykłady: grupowanie przycisków, etykiety nad polami (½ fiksacji), BIP, kierowcy, gra „Lublin", przestrzenie do nauki.

---
[⬅️ Poprzedni temat](9_Ocena_heurystyczna_heurystyki_Nielsena-Molicha.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](Dodatkowe_Persony_UCD_Prototypowanie_Raport_z_badań.md)