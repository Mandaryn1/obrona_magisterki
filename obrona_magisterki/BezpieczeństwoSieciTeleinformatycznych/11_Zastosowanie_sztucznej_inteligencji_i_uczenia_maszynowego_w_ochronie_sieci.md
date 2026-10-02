# Zastosowanie sztucznej inteligencji i uczenia maszynowego w ochronie sieci teleinformatycznych

> Opracowanie oparte na wykładach: **IoT** (W10, slajdy 22, 32, 33 – ML w wykrywaniu anomalii, SOAR, Avast, AI-SOC, zagrożenia AI), **ryzyko** (W11, slajd 13 – SIEM, SOAR, AI), **Zapory/IDS** (slajdy 4, 12, 27, 29 – NGFW z AI, wykrywanie anomalii, alert fatigue), **kurs Cisco** (Moduł 4: analiza zachowania sieci – NBA) i trendy z W1 (*AI/ML Security*, AI Act). Uzupełnienia – ***(uzupełnienie)***. Liczby podane w przykładach z wykładu są **deklaracjami wykładu / producenta**, nie zweryfikowanymi wynikami.

## Dlaczego AI/ML w ochronie sieci

| Problem tradycyjnej ochrony | Jak pomaga AI/ML |
| :--- | :--- |
| **sygnatury nie wykrywają nowych ataków (zero-day)** | wykrywanie anomalii i zachowań bez sygnatury (wykład: metoda anomalii wykrywa nowe ataki) |
| **ogromne wolumeny danych** (>10 Gb/s, miliony zdarzeń) | automatyczna analiza, korelacja, redukcja szumu |
| **alert fatigue** – tysiące alertów dziennie, w większości fałszywych (wykład, slajd 29) | **priorytetyzacja i redukcja fałszywych alarmów** (wykład ryzyko: *ML redukuje fałszywe alarmy o ponad 50%* – deklaracja wykładu) |
| **ruch szyfrowany** (>90%, wykład, slajd 27) | analiza **metadanych** i wzorców (długości, czasy, JA3) zamiast treści |
| **szybkość ataków** (rozprzestrzenianie w minutach) | **automatyczna reakcja** (SOAR): skrócenie czasu z godzin do sekund/minut (wykład IoT i ryzyko) |
| **niedobór specjalistów** | automatyzacja rutynowych zadań SOC |
| **rozproszone środowiska** (chmura, IoT, zdalna praca) | analiza zachowania użytkowników i urządzeń niezależnie od lokalizacji |

## Podstawy: rodzaje uczenia w bezpieczeństwie *(uzupełnienie)*

| Rodzaj | Zasada | Zastosowania | Przykładowe algorytmy |
| :--- | :--- | :--- | :--- |
| **Nadzorowane (supervised)** | uczenie na **oznaczonych** danych (atak/normalny) | klasyfikacja ruchu, malware, phishing, spam, DGA | Random Forest, XGBoost, SVM, sieci neuronowe, regresja logistyczna |
| **Nienadzorowane (unsupervised)** | wykrywanie struktur i **odstających** bez etykiet | **anomalie** (nowe ataki), klasteryzacja zachowań | **Isolation Forest, autoenkodery**, k-średnich, DBSCAN, PCA (wykład IoT wymienia autoenkodery i isolation forest) |
| **Półnadzorowane / jednoklasowe** | uczenie tylko na ruchu „normalnym" | **one-class SVM**, wykrywanie odstępstw | |
| **Głębokie (deep learning)** | sieci wielowarstwowe | analiza pakietów/sekwencji (CNN, **LSTM**, Transformer), malware z kodu/obrazów | DNN (wykład: Avast – DNN analizujący ruch w czasie rzeczywistym) |
| **Ze wzmocnieniem (RL)** | uczenie przez nagrodę | adaptacyjne polityki, obrona automatyczna, symulacje | |
| **Uczenie federacyjne** | trening bez udostępniania danych (wykład IoT) | prywatność, IoT, wiele organizacji | |
| **Modele językowe (LLM)** *(uzup.)* | rozumienie tekstu i kodu | asystenci SOC, podsumowania alertów, generowanie zapytań i reguł, analiza phishingu | |

## Zastosowania w ochronie sieci

### 1. Wykrywanie anomalii i analiza zachowania sieci (NBA/NDR)

Kurs Cisco: **analiza zachowania sieci (NBA)** – analiza dużej ilości różnorodnych danych (cechy przepływu, pakietów, telemetria) technikami **statystycznymi i ML**, porównanie z **linią bazową (baseline)**; znaczące odchylenia = potencjalne **IOC**; wykrycie robaka po zachowaniu skanującym zainfekowanych hostów. Wykład IoT: *algorytmy ML uczą się normalnych wzorców zachowania urządzeń i sieci i identyfikują odstępstwa; metody nienadzorowane (autoenkodery, isolation forest) wykrywają nieznane wcześniej typy ataków (zero-day)*; przykład: żarówka IoT nagle skanuje porty innych urządzeń.

Cechy wejściowe *(uzupełnienie)*: czas trwania sesji, bajty/pakiety, porty, flagi TCP, liczba połączeń na host, entropia DNS, rozkład rozmiarów pakietów, godziny aktywności, pary host–host.

### 2. Rozszerzenia IDS/IPS i NGFW

Wykład (slajd 4): NGFW z **AI, DPI i threat intelligence**; (slajd 12) wykrywanie **anomalii** z użyciem ML w metodzie hybrydowej (sygnatury + anomalie); IPS dostraja się do ruchu; **analiza ruchu szyfrowanego** po metadanych (JA3, rozmiary i czasy pakietów); **identyfikacja aplikacji** klasyfikatorem ML.

### 3. SIEM, UEBA, SOAR i SOC

- **SIEM/UEBA:** analiza zachowań **użytkowników i jednostek** (kurs Cisco: *zaawansowane SIEM obejmują analizę zachowań, wzorce oparte na ludzkich odczuciach*), korelacja zdarzeń, **scoring ryzyka**,
- **SOAR:** **automatyczna reakcja** – izolacja zainfekowanych urządzeń, blokada ruchu, aktualizacja reguł zapory, powiadomienia administratorów (wykład IoT); wykład ryzyko: automatyzacja skraca czas reakcji z godzin do minut,
- **AI-SOC** (wykład IoT, studium inteligentnego miasta): centralny SOC analizujący dziesiątki milionów zdarzeń dziennie, głębokie sieci neuronowe wykrywają anomalie w czasie rzeczywistym, **automatyczna izolacja segmentów**, eskalacja do analityków, generowanie raportów; wykrywanie z godzin do sekund (deklaracja wykładu),
- **priorytetyzacja alertów** (ML ocenia istotność, kontekst, krytyczność aktywa) – walka z **alert fatigue** (wykład, slajd 29).

### 4. Wykrywanie malware, phishingu i domen złośliwych *(uzupełnienie)*

klasyfikacja plików (statyczna i dynamiczna w sandboxie), analiza behawioralna procesów (EDR), **wykrywanie phishingu** (cechy URL, treść wiadomości, wizualne podobieństwo stron), **DGA** (domain generation algorithms – wykrywanie losowych domen C2), reputacja IP/domen, wykrywanie **tunelowania DNS** (entropia, długości zapytań), **beaconingu C2**.

### 5. Uwierzytelnianie adaptacyjne i kontrola dostępu

**Adaptive authentication** (wykład IoT): dynamiczne dostosowanie wymagań uwierzytelnienia do oceny ryzyka (lokalizacja, urządzenie, zachowanie, godzina); **zero trust** z ciągłą oceną; **wykrywanie przejętych kont** (anomalie logowania, „niemożliwa podróż"); **UEBA** w NAC/ISE (profilowanie urządzeń algorytmami ML).

### 6. Wykrywanie ataków DDoS, botnetów i skanowania

modele predykcyjne na przepływach (NetFlow), wykrywanie **botnetów IoT** (np. Mirai-podobne zachowania), automatyczne reguły scrubbingu, adaptacyjne limity.

### 7. Zarządzanie podatnościami i ryzykiem *(uzupełnienie)*

priorytetyzacja podatności modelami predykcyjnymi (np. **EPSS – Exploit Prediction Scoring System**, uzupełniający CVSS: prawdopodobieństwo wykorzystania), korelacja z threat intelligence, symulacje ścieżek ataku (*attack path analysis*), **cyfrowe bliźniaki bezpieczeństwa** (wykład IoT: wirtualne reprezentacje do symulacji ataków bez ryzyka dla produkcji).

### 8. Threat hunting i threat intelligence

automatyczna ekstrakcja IOC/TTP z raportów (NLP), mapowanie na **MITRE ATT&CK**, **autonomiczne polowanie na zagrożenia** (wykład IoT: *AI proaktywnie szuka zagrożeń, identyfikuje IOC*), **predykcja** przyszłych ataków na podstawie wzorców (*predictive security*), **self-healing** – automatyczne usuwanie luk i dostosowanie konfiguracji (wykład IoT).

### 9. Bezpieczeństwo IoT i sieci brzegowych

lekkie modele na urządzeniach brzegowych, wykrywanie anomalii w telemetrii, **macierzowy model bezpieczeństwa** i wykluczanie niewiarygodnych sygnałów (wykład IoT); przykład: **Avast Smart Home Security** – głęboka sieć neuronowa analizująca ruch w czasie rzeczywistym (wykład podaje: 47 typów zagrożeń IoT, skuteczność 98%, 20+ typów urządzeń – deklaracje producenta).

### 10. Generatywna AI i LLM w SOC *(uzupełnienie)*

streszczanie incydentów i alertów, tłumaczenie surowych logów na język naturalny, generowanie zapytań (KQL, SPL) i reguł detekcji (Sigma, Snort), wsparcie w triage i raportach, wyszukiwanie w bazach wiedzy; przykłady rynkowe: asystenci w SIEM/XDR. **Ograniczenia:** halucynacje, wymóg weryfikacji przez człowieka, ryzyko wycieku danych do usług zewnętrznych.

## Schemat: potok ML w wykrywaniu zagrożeń sieciowych *(uzupełnienie)*

```
 zbieranie danych (pakiety, NetFlow, logi, DNS) → ekstrakcja cech → trening (na ruchu normalnym / oznaczonym)
   → wdrożenie (inline/out-of-band) → ocena i score anomalii → alert / automatyczna reakcja (SOAR)
        ▲                                                                       │
        └── informacja zwrotna analityków, aktualizacja modelu, drift ◀─────────┘
```

Metryki: **precyzja, czułość (recall), F1, FPR, AUC**; dla IDS kluczowy jest niski **FPR** (przy ruchu o dużej skali nawet 1% fałszywych alarmów oznacza tysiące dziennie).

## Ograniczenia i ryzyka

| Problem | Opis |
| :--- | :--- |
| **fałszywe alarmy** | anomalia ≠ atak (np. nowa legalna aplikacja); wymaga dostrajania |
| **jakość i reprezentatywność danych** | modele uczone na starych/nietypowych danych (**dryf danych – concept drift**), zbiory benchmarkowe (KDD, CIC-IDS) odbiegają od rzeczywistości |
| **„czarna skrzynka"** | brak wyjaśnialności decyzji (kierunek: **Explainable AI** – wykład IoT) |
| **zatruwanie modeli (data/model poisoning)** | wprowadzenie fałszywych danych treningowych, backdoory w modelach (wykład IoT: *zatruwanie danych treningowych powodujące backdoory*) |
| **ataki adwersarialne (evasion)** | spreparowane dane wejściowe omijają detekcję (wykład IoT: *adversarial attacks – manipulacja danymi wejściowymi*; potrzebna **odporność adwersarialna**) |
| **ekstrakcja modelu, wycieki danych treningowych** | kradzież modelu, ujawnienie wrażliwych informacji |
| **atakujący używają AI** | **AI-powered malware** adaptujące się do obrony, **deepfake'i** (głos/wideo w socjotechnice i BEC), automatyczne spear phishing, generowanie wariantów malware, szybkie rozpoznanie i exploit (wykład IoT; W1: *deepfakes i dezinformacja*) |
| **koszty i zasoby** | moc obliczeniowa, kompetencje, integracja |
| **prywatność i prawo** | RODO (profilowanie, monitoring pracowników), **AI Act** (UE – wymagania dla systemów AI wysokiego ryzyka, wykład W1 wymienia AI Act wśród regulacji), nadzór ludzki |
| **zależność od automatyzacji** | błędna automatyczna blokada = przestój; potrzebny **człowiek w pętli** i tryb „detekcja przed blokowaniem" |
| **ruch szyfrowany** | modele operują na metadanych, nie na treści |

**Zasada:** *„AI to podwójnie ostry miecz – może być najpotężniejszą bronią obronną, ale także najbardziej niebezpiecznym narzędziem ataku"* (cytat z wykładu IoT, S. Russell) – wyścig zbrojeń trwa.

## Dobre praktyki wdrożenia *(uzupełnienie)*

1. **Hybryda**: sygnatury + anomalie + reguły eksperckie + ML (wykład: metoda hybrydowa).
2. **Baseline i dostrajanie** na własnym ruchu; okres uczenia w trybie *monitor*.
3. **Człowiek w pętli** (analityk weryfikuje decyzje krytyczne); stopniowe zwiększanie autonomii SOAR.
4. **Jakość danych i telemetria** (pełne, zsynchronizowane logi i przepływy), ochrona danych treningowych.
5. **Wyjaśnialność** i audyt modeli, monitoring dryfu, retrening.
6. **Testy odporności** (red team AI, **MITRE ATLAS**, **OWASP Top 10 dla LLM**), ochrona modeli (kontrola dostępu, filtrowanie wejść).
7. **Zgodność** (RODO, AI Act, polityki wewnętrzne) i **szkolenia** SOC.
8. **Obrona przed AI-atakami**: anty-deepfake, weryfikacja wieloczynnikowa, szkolenia z socjotechniki.
9. Integracja z **SIEM/SOAR/EDR** i threat intelligence; mierzenie **MTTD/MTTR** i FPR.

## Podsumowanie

- **AI/ML** uzupełniają tradycyjne środki: **wykrywanie anomalii i zero-day** (nienadzorowane: autoenkodery, isolation forest; NBA/NDR), **redukcja fałszywych alarmów i alert fatigue**, **automatyczna reakcja (SOAR)**, UEBA i adaptacyjne uwierzytelnianie, wykrywanie malware/phishingu/DGA/tunelowania, priorytetyzacja podatności (EPSS), threat hunting i predykcja, ochrona IoT (edge ML), asystenci LLM w SOC.
- **Ograniczenia:** fałszywe alarmy i dryf danych, brak wyjaśnialności, **zatruwanie i ataki adwersarialne**, AI po stronie atakujących (deepfake'i, malware adaptacyjne), prywatność i regulacje (AI Act), potrzeba nadzoru człowieka.
- Zasada: **hybrydowe podejście, człowiek w pętli, dobre dane, ciągłe dostrajanie i testy odporności**.
