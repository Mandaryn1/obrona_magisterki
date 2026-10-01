# Warianty należące do zbioru wariantów optymalnych w sensie Pareto

## Dominacja

Niech wszystkie kryteria są maksymalizowane. Wariant $x$ **dominuje** wariant $y$ (zapis $x\succ y$), jeśli:

$$f_k(x)\ge f_k(y)\ \ \text{dla wszystkich } k=1,\dots,s\quad\text{oraz}\quad f_k(x)>f_k(y)\ \ \text{dla co najmniej jednego } k$$

Czyli $x$ jest **nie gorszy** na każdym kryterium i **lepszy** na co najmniej jednym. Wariant zdominowany nie ma sensu rozważać – istnieje wariant, który jest od niego wszędzie nie gorszy, a gdzieś lepszy.

(Dla kryteriów minimalizowanych nierówności odwracamy.)

## Optymalność w sensie Pareto

Wariant $x'\in D$ jest **optymalny w sensie Pareto** (inaczej: **sprawny, efektywny, niezdominowany**), jeżeli **nie istnieje** żaden inny wariant $x\in D$, który poprawiałby wartość co najmniej jednego kryterium **bez pogorszenia** wartości żadnego z pozostałych.

Równoważnie: **żaden inny wariant go nie dominuje**.

- zbiór wszystkich takich wariantów to **zbiór Pareto** $P$ (w przestrzeni decyzji),
- ich obrazy w przestrzeni kryteriów $\big(f_1(x),\dots,f_s(x)\big)$ tworzą **front Pareto** (granicę Pareto).

Nazwa: **Vilfredo Pareto** (ekonomista); stan, w którym nie można poprawić sytuacji jednego podmiotu bez pogorszenia sytuacji innego, to optimum Pareto.

## Charakterystyka wariantów ze zbioru Pareto

1. **Niezdominowane** – żaden inny wariant nie jest od nich lepszy w sensie dominacji.
2. **Wzajemnie nieporównywalne** – dla dowolnych dwóch wariantów z $P$ jeden jest lepszy na jednym kryterium, drugi na innym. Przejście od jednego do drugiego to **kompromis** (wymiana: zysk na jednym kryterium kosztem straty na innym, ang. *trade-off*).
3. Dla każdego wariantu **spoza** $P$ (zbiór skończony) istnieje wariant z $P$, który go dominuje – zbiór Pareto „zasłania" wszystkie pozostałe.
4. Zbiór Pareto jest **kandydatem na zbiór rozwiązań** – decydent wybiera jeden spośród nich (rozwiązanie kompromisowe) zgodnie ze swoimi preferencjami, np. za pomocą wag, kryterium głównego, punktu idealnego.
5. Wiele kryteriów oznacza zwykle **wiele** wariantów Pareto; w skrajnym przypadku **każdy** wariant może być sprawny (kryteria przeciwstawne). Im więcej kryteriów, tym zwykle większa część zbioru jest niezdominowana.
6. **Jedno rozwiązanie Pareto** istnieje tylko wtedy, gdy wszystkie optima cząstkowe znajdują się w tym samym punkcie (kryteria zgodne) – jest to wtedy również optimum całego problemu, równe punktowi idealnemu.
7. Wariant będący **jedynym optimum jednego kryterium** (np. najtańszy, gdy cena jest jedyna) jest zawsze Pareto-optymalny; wszystkie „ekstrema” kryteriów leżą na froncie (lub na jego słabej części).
8. Rozwiązanie leksykograficzne i rozwiązanie sumy ważonej z **dodatnimi** wagami są Pareto-optymalne (zob. temat 2).

### Odmiany pojęcia

| Pojęcie | Warunek |
| :--- | :--- |
| **optymalny w sensie Pareto (silnie sprawny)** | nie istnieje wariant nie gorszy na wszystkich i lepszy na co najmniej jednym kryterium |
| **słabo Pareto-optymalny** | nie istnieje wariant **ściśle lepszy na wszystkich** kryteriach jednocześnie (zbiór słabo sprawny zawiera zbiór sprawny) |
| **punkt idealny** | wartości najlepsze dla każdego kryterium z osobna – zwykle poza zbiorem osiągalnym |

## Przykład

Dwa kryteria maksymalizowane, sześć wariantów:

| Wariant | $f_1$ | $f_2$ | Dominowany przez |
| :--- | :-: | :-: | :--- |
| A | 2 | 9 | – |
| B | 4 | 8 | – |
| C | 6 | 6 | – |
| D | 5 | 5 | C (6 ≥ 5, 6 ≥ 5) |
| E | 8 | 3 | – |
| F | 7 | 2 | E (8 ≥ 7, 3 ≥ 2) |

**Zbiór Pareto: $\{A,B,C,E\}$**. Warianty D i F są zdominowane, więc je odrzucamy. Wśród A, B, C, E nie ma pary, w której jeden dominowałby drugi – np. A ma najlepsze $f_2$, ale najgorsze $f_1$; E odwrotnie.

W wykresie $(f_1,f_2)$ wariant należy do frontu Pareto, jeśli w „kwadrancie” w prawo-górę od niego nie ma żadnego innego punktu.

## Wyznaczanie zbioru Pareto

**Metoda bezpośrednia (zbiór skończony)**: porównać każdą parę wariantów, usunąć zdominowane – złożoność $O(n^2s)$ ($n$ – liczba wariantów, $s$ – kryteriów). Szybsze są algorytmy sortujące/dziel i zwyciężaj (np. dla 2 kryteriów: sortowanie malejące wg $f_1$ i skanowanie z kontrolą maksimum $f_2$ – $O(n\log n)$).

**Zbiory ciągłe**:

- **suma ważona** $\sum w_kf_k$ z $w_k>0$ – generuje punkty frontu (tylko jego część wypukłą, tzw. rozwiązania wspierane),
- **metoda ε-ograniczeń** (kryterium główne + progi na pozostałych) – może znaleźć także punkty niewypukłej części frontu,
- **metoda punktu idealnego** (minimalizacja odległości, np. Czebyszewa),
- **algorytmy ewolucyjne** (NSGA-II, SPEA2, MOEA/D) – aproksymują cały front.

**Diagram Hassego** (graf skierowany relacji „jest lepszy od") pokazuje strukturę dominacji; warianty Pareto to **wierzchołki nieopatrzone żadną strzałką dominującą** („szczyty").

## Wybór jednego wariantu ze zbioru Pareto

Sama optymalność Pareto nie wskazuje jednego wariantu. Stosuje się:

- sumę ważoną (po normalizacji, temat 1) i wagi (AHP, temat 3),
- metodę leksykograficzną (temat 2) lub kryterium główne,
- minimalizację odległości od punktu idealnego,
- metody relacji przewyższania (ELECTRE, PROMETHEE),
- metody interaktywne.

## Podsumowanie

- Wariant jest **Pareto-optymalny**, gdy żaden inny wariant go nie dominuje: nie da się poprawić jednego kryterium bez pogorszenia innego.
- Dominacja: nie gorszy na wszystkich kryteriach i lepszy na co najmniej jednym.
- Warianty Pareto są wzajemnie nieporównywalne; reprezentują **kompromisy** między kryteriami. Zbiór może zawierać jeden wariant (kryteria zgodne) albo wszystkie (kryteria przeciwstawne).
- Pierwszy krok analizy wielokryterialnej: **odrzucić warianty zdominowane**, potem wybrać jeden z pozostałych według preferencji decydenta.
