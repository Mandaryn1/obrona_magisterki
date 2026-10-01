# Pojemność pasożytnicza – czym jest i co powoduje

## Definicja

**Pojemność pasożytnicza** (pasożytnicza, rozproszona, pojemność montażowa; ang. *parasitic / stray capacitance*) to **niezamierzona pojemność elektryczna**, która powstaje **pomiędzy dowolnymi dwoma elementami przewodzącymi** (ścieżkami, wyprowadzeniami, przewodami, obudową, masą), **rozdzielonymi dielektrykiem** (powietrzem, laminatem PCB, izolacją), **gdy między nimi istnieje różnica potencjałów**. Nie jest to element wstawiony przez projektanta – wynika z fizycznej konstrukcji układu i jest **nieunikniona**.

Kondensator = dwie okładziny + dielektryk. Każde dwa przewodniki są „okładzinami" kondensatora:

$$C=\varepsilon_0\varepsilon_r\frac{A}{d}\qquad(\varepsilon_0=8{,}854\cdot10^{-12}\ \text{F/m})$$

Pojemność rośnie z **powierzchnią $A$** i przenikalnością $\varepsilon_r$, maleje z **odległością $d$**. Dla przykładu: dwie płaskie powierzchnie 1 cm² w odległości 1,6 mm w laminacie FR-4 ($\varepsilon_r\approx4{,}4$) mają około 2,4 pF – wartość „mała", ale w układach szybkich i o wysokiej impedancji wystarcza, by zakłócić działanie.

## Gdzie powstaje

- **między ścieżkami** na PCB biegnącymi równolegle blisko siebie (**sprzężenie pojemnościowe**),
- **między ścieżką a płaszczyzną masy/zasilania** (laminat jako dielektryk), między warstwami wielowarstwowej płytki,
- **między wyprowadzeniami** układu scalonego, w obudowie, w złączach, kablach i przewodach,
- **wejścia i wyjścia układów** – pojemność wejściowa bramek CMOS i MOSFET-ów (np. $C_{GS}$, $C_{GD}$ – pojemność Millera), pojemności złączowe diod i tranzystorów,
- **uzwojenia cewek i transformatorów** (pojemność międzyzwojowa; powoduje rezonans własny cewki),
- **między elementami a obudową**, ciałem człowieka i otoczeniem,
- **linie transmisyjne i magistrale** (pojemność przypadająca na jednostkę długości przewodu).

## Skutki i problemy

| Skutek | Mechanizm | Przykład |
| :--- | :--- | :--- |
| **Opóźnienie i ograniczenie szybkości sygnałów** | pojemność tworzy z rezystancją źródła filtr RC; krawędzie sygnału są „rozmywane" | $R=10\,\text{k}\Omega$, $C=20\,\text{pF}$: $\tau=200$ ns, czas narastania 10–90% $\approx2{,}2\tau=440$ ns – ogranicza prędkość transmisji |
| **Ograniczenie prędkości magistral** | pojemność przewodów i wejść układów trzeba ładować przez rezystory podciągające | w I²C maks. pojemność magistrali ok. 400 pF; $R_{p,\max}\approx\dfrac{t_r}{0{,}8473\,C_b}$ (np. Fast mode: $t_r=300$ ns, $C_b=100$ pF → $R_p\lesssim3{,}5\ \text{k}\Omega$) |
| **Przesłuchy (crosstalk)** | sygnał z jednej ścieżki „przecieka" pojemnościowo do sąsiedniej | zakłócenia w sąsiednich liniach przy szybkich zboczach |
| **Zniekształcenia sygnału, przeregulowania, dzwonienie** | pojemność + indukcyjność pasożytnicza tworzą obwody rezonansowe | oscylacje po zboczu sygnału na liniach szybkich |
| **Większy pobór mocy** | każda zmiana napięcia wymaga przeładowania pojemności: $P_{dyn}=C\,V^2f$ | $C=10$ pF, $V=3{,}3$ V, $f=10$ MHz → ok. 1,1 mW na jedną linię |
| **Zmiana parametrów filtrów, wzmacniaczy, oscylatorów** | pojemności „dodają się" do projektowanych; w wzmacniaczach ograniczają pasmo i mogą powodować niestabilność | zmiana częstotliwości rezonansowej, spadek wzmocnienia przy wysokich częstotliwościach |
| **Zakłócenia sieciowe i EMI** | sprzężenia pojemnościowe przenoszą zakłócenia z zasilaczy impulsowych i silników | zakłócenia w torze pomiarowym czujników |
| **Błędy pomiarowe w czujnikach** | pojemność kabli i wejść obciąża czujniki o dużej impedancji (np. piezo, pojemnościowe, pH) i dodaje się do mierzonej pojemności | w czujnikach pojemnościowych (przyciski dotykowe) pojemność pasożytnicza ogranicza czułość |
| **Prądy upływu/łączenie masy (ground bounce)** | szybkie przełączanie przez pojemności wprowadza prądy impulsowe w masie | niestabilność odczytu ADC |

W układach zasilanych bateryjnie ważna jest też **energia** tracona na ładowanie pojemności pasożytniczych przy każdym przełączeniu (energia $\tfrac12CV^2$ na cykl).

## Jak ograniczać pojemność pasożytniczą i jej skutki

- **krótsze i węższe ścieżki**, zmniejszenie powierzchni zachodzących na siebie warstw,
- **większe odstępy** między liniami wrażliwymi i szybkimi (reguła 3W), nierównoległe prowadzenie ścieżek,
- właściwe **płaszczyzny masy**, przekładki ekranujące, **ekrany** i przewody ekranowane, **pierścienie ochronne (guard ring)** wokół wysokoimpedancyjnych wejść,
- **sygnały różnicowe** (RS-485, CAN, USB) – odporne na sprzężenia,
- **mniejsze rezystancje** źródła i podciągania (niższe $RC$), bufory/wtórniki napięciowe, wzmacniacze linii,
- **rezystory szeregowe** tłumiące dzwonienie, terminacja linii,
- **kondensatory odsprzęgające** blisko wyprowadzeń zasilania układów,
- dobór podzespołów o **małych pojemnościach własnych**, obudowy o mniejszych pojemnościach (SMD zamiast przewlekanych),
- **kompensacja** w projekcie (uwzględnianie w modelu symulacyjnym, np. SPICE, dostrajanie kondensatorem),
- w czujnikach pojemnościowych: pomiar względem referencji, ekranowanie (driven shield).

## Pojemność pasożytnicza a indukcyjność pasożytnicza

Obok pojemności występuje **indukcyjność pasożytnicza** (ścieżki, wyprowadzenia, pętle prądowe), a razem tworzą **pasożytnicze obwody rezonansowe** (RLC). Realne elementy nie są idealne: kondensator ma szeregową indukcyjność (ESL), rezystor – pojemność równoległą, cewka – pojemność międzyzwojową. Powyżej częstotliwości rezonansu własnej „kondensator zachowuje się jak cewka" (i odwrotnie).

## Podsumowanie

- Pojemność pasożytnicza to **niezamierzona pojemność** między dowolnymi przewodzącymi elementami układu rozdzielonymi dielektrykiem; zależy od powierzchni, odległości i materiału ($C=\varepsilon A/d$).
- Powstaje na PCB, w kablach, w tranzystorach (np. pojemność Millera), cewkach i złączach.
- Powoduje **opóźnienia (RC) i ograniczenie prędkości** (np. magistrali I²C), **przesłuchy**, zniekształcenia, **straty energii** ($CV^2f$), błędy czujników i niestabilność wzmacniaczy.
- Ograniczamy ją odpowiednim **projektem PCB** (krótkie ścieżki, odstępy, masa, ekranowanie), **małymi rezystancjami** w torze sygnału, terminacją i odsprzęganiem.
