# Modulacja szerokości impulsów PWM (Pulse Width Modulation)

## Idea

**PWM** to technika, w której generuje się **przebieg prostokątny o stałej częstotliwości**, a **informację/wartość sterującą koduje się w szerokości (czasie trwania) impulsu**. Zmieniając **stosunek czasu stanu wysokiego do okresu** (współczynnik wypełnienia), zmienia się **średnią wartość** sygnału – a więc średnią moc dostarczaną do odbiornika – bez zmiany amplitudy.

Zaleta: tranzystor sterujący pracuje jako **klucz** (całkowicie włączony lub całkowicie wyłączony), więc **straty mocy są niewielkie** (brak długotrwałego stanu pośredniego, w którym rozpraszałaby się duża moc, jak w regulacji liniowej). Do tego sygnał jest **cyfrowy** – łatwo go wytworzyć w mikrokontrolerze.

## Parametry sygnału PWM

```
 V  ┌────┐        ┌────┐        ┌────┐
    │    │        │    │        │    │
 0 ─┘    └────────┘    └────────┘    └────
    ◀─t_on─▶◀t_off▶
    ◀──────── T ──────▶
```

| Wielkość | Wzór | Znaczenie |
| :--- | :--- | :--- |
| **Okres** $T$ | $T=t_{on}+t_{off}$ | czas jednego cyklu |
| **Częstotliwość** $f$ | $f=\dfrac1T$ | liczba cykli na sekundę (stała) |
| **Współczynnik wypełnienia** $D$ (duty cycle) | $D=\dfrac{t_{on}}{T}\cdot100\%$ | procent okresu w stanie wysokim; 0% – stale wyłączony, 100% – stale włączony |
| **Wartość średnia** | $V_{sr}=D\cdot V_{max}$ | napięcie „odczuwane" przez odbiornik bezwładny |
| **Wartość skuteczna (RMS)** | $V_{RMS}=V_{max}\sqrt{D}$ | moc w rezystancji $P=D\cdot V_{max}^2/R$ |

**Przykład**: napięcie 12 V, $D=25\%$ → $V_{sr}=3$ V; $V_{RMS}=6$ V; moc w odbiorniku rezystancyjnym wynosi 25% mocy maksymalnej.

## Zasada działania w praktyce

Odbiornik (silnik, LED, grzałka) ma **bezwładność** (mechaniczną, cieplną, optyczną – oko) lub zawiera filtr (indukcyjność, pojemność), które **uśredniają** szybkie przełączanie. Jeśli częstotliwość PWM jest znacznie większa niż dynamika odbiornika, efektem jest **płynna regulacja** wielkości średniej:

- **LED**: jasność ∝ $D$ (oko uśrednia, wystarczy $f>100$–$200$ Hz, by uniknąć migotania),
- **silnik DC**: prędkość ∝ $D$ (indukcyjność uzwojeń wygładza prąd),
- **grzałka**: moc cieplna ∝ $D$.

## Generowanie PWM w mikrokontrolerze

Sprzętowy moduł **timera/licznika** i **komparatora**:

1. licznik zlicza od 0 do wartości maksymalnej **TOP** (określa okres),
2. gdy wartość licznika zrówna się z rejestrem porównania **CCR/OCR** (*compare*), wyjście zmienia stan (przełączenie),
3. po osiągnięciu TOP licznik się zeruje i cykl się powtarza.

Zmiana rejestru porównania zmienia współczynnik wypełnienia: $D=\dfrac{OCR}{TOP+1}$.

### Rozdzielczość i częstotliwość

- **rozdzielczość** $n$ bitów: $2^n$ poziomów wypełnienia (8-bit: 256 kroków; 10-bit: 1024) – krok $D$ wynosi $1/2^n$; dla zakresu 0–3,3 V w 8-bit krok ≈ 13 mV, w 10-bit ≈ 3,2 mV,
- **częstotliwość** zależy od taktowania i dzielnika (preskalera) oraz TOP:

$$f_{PWM}=\frac{f_{clk}}{N\cdot(TOP+1)}$$

  Przykład (ATmega328P, 16 MHz, preskaler $N=64$, 8-bit, *fast PWM*): $f=\dfrac{16\,\text{MHz}}{64\cdot256}\approx976{,}6$ Hz (domyślnie dla pinów 5 i 6 w Arduino Uno); w trybie *phase-correct* ($TOP=255$, licznik liczy w górę i w dół: $f=\dfrac{f_{clk}}{N\cdot510}$) otrzymujemy ok. 490 Hz.
- **kompromis**: większa rozdzielczość = niższa częstotliwość przy danym taktowaniu (i odwrotnie).

### Tryby

- **Fast PWM** – licznik tylko w górę (prosty, wyższa częstotliwość, impulsy wyrównane do krawędzi),
- **Phase-correct PWM** – licznik w górę i w dół (impulsy symetryczne, mniej zakłóceń, lepsze dla silników, połowa częstotliwości),
- PWM programowy (w pętli/przerwaniu) – mniej dokładny.

## Dobór częstotliwości PWM

| Zastosowanie | Typowa częstotliwość |
| :--- | :--- |
| LED (jasność) | 100 Hz – kilka kHz (powyżej progu migotania) |
| Serwomechanizmy RC | **50 Hz** (okres 20 ms) |
| Silniki DC | 1–20 kHz (często >20 kHz, aby uniknąć słyszalnego „piszczenia") |
| Zasilacze impulsowe (SMPS) | 50 kHz – kilka MHz |
| Wzmacniacze klasy D (audio) | 200 kHz – 1 MHz |
| ESC (regulatory dla silników BLDC) | kilka–kilkadziesiąt kHz |

Zbyt niska częstotliwość – migotanie, szarpanie silnika, słyszalne tony. Zbyt wysoka – większe **straty przełączania** w tranzystorach i problemy z pojemnościami pasożytniczymi (zob. temat 7).

## Zastosowania

- **regulacja prędkości silników DC** (z mostkiem H – także zmiana kierunku),
- **ściemnianie LED** i sterowanie oświetleniem,
- **serwomechanizmy**: szerokość impulsu 1–2 ms w cyklu 20 ms ustawia kąt (ok. 1,5 ms – położenie środkowe; typowo 1 ms ≈ 0°, 2 ms ≈ 180°, zależnie od modelu),
- **regulacja temperatury** (grzałki, wentylatory), moc w obwodach 230 V (SSR),
- **przetwornice i zasilacze impulsowe** (regulacja napięcia wyjściowego przez wypełnienie),
- **ładowarki** akumulatorów, sterowanie mocą paneli (MPPT),
- **DAC „na tanio"**: PWM + filtr dolnoprzepustowy RC → napięcie analogowe,
- **audio** (klasa D), transmisja informacji (modulacja PWM w telekomunikacji).

### PWM jako przetwornik cyfrowo-analogowy

Filtr RC uśrednia przebieg: napięcie wyjściowe ≈ $D\cdot V_{max}$. Częstotliwość graniczna filtra $f_c=\dfrac1{2\pi RC}$ musi być **znacznie niższa** niż $f_{PWM}$ (np. $R=1\,\text{k}\Omega$, $C=10\,\mu\text{F}$ → $f_c\approx16$ Hz). Zbyt mały filtr – tętnienia; zbyt duży – wolna reakcja.

## Zalety i wady

| Zalety | Wady |
| :--- | :--- |
| **wysoka sprawność** (klucz tranzystorowy), małe straty cieplne | **zakłócenia elektromagnetyczne (EMI)** od szybkich zboczy |
| prosta realizacja cyfrowa w mikrokontrolerze (timery) | tętnienia, konieczność filtrowania, gdy potrzebne jest napięcie ciągłe |
| precyzyjna regulacja w szerokim zakresie | możliwe słyszalne „piszczenie" silników przy niskiej $f$ |
| nie zmienia amplitudy – stała siła sygnału | ograniczenia związane z rozdzielczością i częstotliwością |
| praca z obciążeniami dużej mocy (przez drivery) | straty przełączania przy bardzo wysokiej częstotliwości |

## PWM a inne modulacje impulsowe

- **PWM** – zmienia się **szerokość** impulsu (stała częstotliwość),
- **PPM** (Pulse Position Modulation) – zmienia się **położenie** impulsu,
- **PAM** – zmienia się **amplituda**,
- **PFM** – zmienia się **częstotliwość** (stała szerokość).

## Podsumowanie

- PWM = przebieg prostokątny o **stałym okresie** i **zmiennym współczynniku wypełnienia** $D=t_{on}/T$; **średnia wartość** sygnału $V_{sr}=D\cdot V_{max}$.
- Generowany sprzętowo przez timer i komparator; parametry: częstotliwość $f=f_{clk}/(N(TOP+1))$, rozdzielczość $n$ bitów.
- Pozwala w sposób **energooszczędny** regulować moc: jasność LED, prędkość silników, temperaturę, położenie serwa (impuls 1–2 ms co 20 ms), a także realizować „DAC" z filtrem RC i zasilacze impulsowe.
- Wady: zakłócenia EMI, tętnienia, kompromis rozdzielczość–częstotliwość.

---
[⬅️ Poprzedni temat](9_Aktuatory.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../BarieryWPrzestrzeniCyfrowej/BarieryWPrzestrzeniCyfrowej_tytul.md)