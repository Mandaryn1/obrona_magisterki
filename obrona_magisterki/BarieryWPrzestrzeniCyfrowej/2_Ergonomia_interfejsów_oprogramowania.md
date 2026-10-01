# Ergonomia interfejsów oprogramowania – definicja, obszary, typy i przykłady

## Ergonomia – definicja

**Ergonomia** (wg IEA – International Ergonomics Association) to **dyscyplina naukowa zajmująca się zrozumieniem interakcji między ludźmi i innymi elementami systemu** oraz **zawód stosujący teorię, zasady, dane i metody projektowania** w celu optymalizacji **dobrostanu ludzi i ogólnej wydajności systemu**.

- Etymologia: gr. *ergon* (praca) + *nomos* (prawo, zasada, norma).
- Dyscyplina naukowa i praktyczna zajmująca się **organizacją pracy człowieka w układzie człowiek – maszyna – warunki otoczenia**.
- **Cel:** minimalizacja **biologicznego kosztu pracy** (ryzyka wypadków i chorób zawodowych) i zwiększenie jej efektywności.
- **Cel utylitarny:** polepszanie warunków pracy przez **dostosowanie ich do możliwości pracownika** oraz – odwrotnie – przez właściwy dobór i przygotowanie (np. edukację) pracownika do pracy.
- Powiązania z innymi naukami: antropologia, higiena, prakseologia, organizacja i zarządzanie, socjologia pracy, psychologia, fizjologia człowieka, ekonomia.

### Analiza systemowa i obciążenia

- Ergonomia bada **zachowanie złożonego systemu** wyodrębnionego z otoczenia, ale zachowującego z nim związki; szuka prawidłowości wynikających z elementów, ich właściwości i powiązań; zapewnia **kompatybilność z zachowaniem priorytetu człowieka**.
- **Obciążenia pracą:** fizyczne (praca mięśni) i psychiczne (praca mózgu); wysiłek psychofizyczny = pokonywanie uciążliwości pracy w określonych warunkach.
- **System człowiek–środowisko pracy**: realizuje zamierzone działanie, współpracuje z innymi systemami, przeciwdziała zakłóceniom, dostosowuje się do zmian, zużywa się i wymaga konserwacji.
- Problemy układu człowiek–praca: skutki prawne (BHP), ograniczenia biologiczne człowieka i ich przekraczalność, **zróżnicowanie ludzi** (brak możliwości unifikacji), wpływ wielu czynników (fizycznych, psychicznych, wiedzy, umiejętności) na efektywność.

## Obszary (domeny) ergonomii

Wg IEA – trzy obszary:

| Obszar | Czym się zajmuje | Tematy | Przykłady w interfejsach oprogramowania |
| :--- | :--- | :--- | :--- |
| **Ergonomia fizyczna** | cechy anatomiczne, antropometryczne, fizjologiczne, biomechaniczne i ich wpływ na aktywność fizyczną | pozycja przy pracy, obsługa, powtarzalne ruchy, zaburzenia mięśniowo-szkieletowe, układ stanowiska, bezpieczeństwo i zdrowie | układ klawiatury i myszy, wysokość monitora, rozmiar i rozmieszczenie przycisków dotykowych (zasięg kciuka), minimalizacja zbędnych kliknięć i ruchów |
| **Ergonomia poznawcza** | procesy myślowe: percepcja, pamięć, rozumowanie, reakcja motoryczna i ich rola w interakcji człowieka z systemem | obciążenie psychiczne, podejmowanie decyzji, niezawodność człowieka, stres, szkolenia | czytelność, jasna nawigacja, spójność, ograniczenie liczby elementów (pamięć krótkotrwała), komunikaty o błędach, rozpoznawanie zamiast zapamiętywania |
| **Ergonomia organizacyjna** | optymalizacja systemów **społeczno-technicznych**: struktury organizacyjne, polityki, procesy | komunikacja, zarządzanie zasobami załogi, projekt i czas pracy, praca zespołowa, projektowanie partycypacyjne, telepraca, zarządzanie jakością | organizacja procesów w aplikacji (workflow), wspólna praca nad dokumentem, role i uprawnienia, wsparcie telepracy |

Ergonomia fizyczna i organizacyjna są **dobrze zdefiniowane i znormalizowane**; ergonomia poznawcza „bada użytkownika, aby stworzyć bardziej odpowiednie interfejsy" – to ona jest najważniejsza dla projektowania interfejsów oprogramowania.

## Typy ergonomii (ze względu na moment działania)

| Typ | Idea | Cele / cechy |
| :--- | :--- | :--- |
| **Ergonomia korekcyjna** | analiza **istniejących** układów człowiek–praca i wprowadzanie zmian usuwających wykryte usterki w eksploatacji | poprawa warunków pracy (oświetlenie, mikroklimat, czynniki szkodliwe), eliminacja nadmiernych obciążeń fizycznych i psychicznych, zwiększenie wydajności układu |
| **Ergonomia koncepcyjna** | wymuszenie stosowania prawidłowych rozwiązań **już na etapie projektowania** układów człowiek–maszyna | mechanizm **norm i dobrych praktyk**; etap: projektowanie systemów |

**Wady (ograniczenia) ergonomii korekcyjnej** – jest „naprawianiem po fakcie":

- wykorzystanie starych maszyn zaprojektowanych bez ergonomii,
- **przenoszenie błędów konstrukcyjnych** na nowe maszyny i linie (utrwalanie nieergonomicznych rozwiązań),
- oszczędnościowa polityka realizacji inwestycji,
- nowe technologie eliminujące jedne problemy, ale wprowadzające inne.

Wniosek: dla oprogramowania **tańsze i skuteczniejsze jest projektowanie ergonomiczne od początku** (koncepcyjne), zgodne z ideą projektowania uniwersalnego (temat 1) i UCD; testowanie i poprawianie gotowych interfejsów to ergonomia korekcyjna.

## Interakcja człowiek–komputer (HCI)

**HCI (Human–Computer Interaction)** – dziedzina badająca **relacje między człowiekiem a komputerem**; koncentruje się na **projektowaniu, ewaluacji i wdrażaniu interaktywnych systemów komputerowych** używanych przez człowieka oraz wszystkich aspektach relacji system–człowiek. Interakcja może być **bezpośrednia** (natychmiastowa informacja zwrotna) lub pośrednia.

## Koncepcja interfejsu

- **Interfejs** to element służący jako **wspólna granica (ograniczenie) między kilkoma komunikującymi się jednostkami**. Aby komunikacja była możliwa, musi istnieć **fizyczny związek** między jednostkami oraz **identyczne dla wszystkich znaczenie formalizmów** używanych przez te jednostki.
- **Interfejs użytkownika (UI)** – **punkt interakcji między komputerem a ludźmi**; obejmuje dowolny sposób interakcji (grafika, dźwięk, pozycja, ruch, ...), w którym dane są przesyłane między użytkownikiem a systemem.
- *(uzupełnienie)* Rodzaje interfejsów użytkownika: wiersza poleceń (CLI), graficzny (GUI), dotykowy, głosowy (VUI), naturalny (NUI: gesty, ruch, wzrok), rzeczywistości rozszerzonej/wirtualnej.

## Ergonomia interfejsu – przykłady dobrych i złych rozwiązań

| Obszar | Dobra praktyka | Błąd ergonomiczny |
| :--- | :--- | :--- |
| fizyczny | duże, odległe cele dotykowe; skróty klawiszowe | mikroskopijne linki blisko siebie; zmuszanie do wielu kliknięć |
| poznawczy | spójna nawigacja, etykiety nad polami, informacja zwrotna o stanie | przeciążenie ekranu, nietypowe ikony, ukryte funkcje, brak komunikatu po akcji |
| organizacyjny | proces zgodny z faktycznym przebiegiem pracy | system narzuca nienaturalną kolejność działań |

*Przykład korekcyjny:* zmiana układu formularza po testach użyteczności, które wykazały błędy użytkowników. *Przykład koncepcyjny:* zaprojektowanie formularza zgodnie z zasadami ergonomii i dostępności przed implementacją.

## Podsumowanie

- Ergonomia = nauka i praktyka dopasowania systemów do człowieka (dobrostan + wydajność systemu); trzy obszary: **fizyczna, poznawcza, organizacyjna**; dla interfejsów kluczowa jest **poznawcza**.
- Typy: **korekcyjna** (naprawianie istniejących układów) i **koncepcyjna** (zasady od etapu projektu).
- Interfejs = punkt styku i wspólny „język" komunikujących się jednostek; interfejs użytkownika = punkt interakcji człowiek–komputer (HCI).
