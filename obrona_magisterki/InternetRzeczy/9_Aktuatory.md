# Aktuatory – czym są i do czego służą

## Definicja

**Aktuator (element wykonawczy, człon wykonawczy)** to urządzenie, które **zamienia sygnał sterujący (zwykle elektryczny) i energię zasilającą na działanie fizyczne** w otoczeniu: ruch (obrotowy lub liniowy), siłę, moment, ciepło, światło, dźwięk, przepływ płynu. Jest **odwrotnością czujnika**: czujnik zamienia zjawisko fizyczne na sygnał, aktuator – sygnał na zjawisko fizyczne.

W systemie sterowania (w tym IoT) tworzy **pętlę**: **czujnik → sterownik (mikrokontroler) → aktuator → obiekt/otoczenie → czujnik**.

## Do czego służą

- **realizacja decyzji układu sterującego** – wykonywanie poleceń (zamknij zawór, włącz silnik, odblokuj zamek),
- **utrzymywanie parametrów procesu** (regulacja temperatury, przepływu, położenia, prędkości – w pętli ze sprzężeniem zwrotnym),
- **automatyzacja** budynków, maszyn, pojazdów, rolnictwa,
- **sygnalizacja i interakcja z użytkownikiem** (alarm, wyświetlacz, głośnik, wibracje).

Przykłady w IoT: inteligentny zamek, zawór nawadniania, żaluzje z napędem, termostat grzejnika (zawór/grzałka), oświetlenie LED ściemniane PWM, dozowniki, roboty, drony, systemy HVAC.

## Elementy toru wykonawczego

1. **sygnał sterujący** z mikrokontrolera (GPIO, PWM, DAC, magistrala – małe moce),
2. **układ sterujący mocą (driver)**: tranzystor MOSFET/BJT, przekaźnik, SSR, **mostek H**, sterownik silnika krokowego, wzmacniacz,
3. **aktuator** (silnik, siłownik, elektrozawór…),
4. (opcjonalnie) **sprzężenie zwrotne**: enkoder, potencjometr, czujnik prądu/położenia → regulator PID.

Wyjścia mikrokontrolera mają małą wydajność prądową (kilka–kilkadziesiąt mA), dlatego **nie zasila się bezpośrednio** silników i przekaźników; stosuje się driver, **diodę zwrotną (flyback)** przy obciążeniach indukcyjnych i często **separację galwaniczną** (transoptor).

## Klasyfikacja

### Ze względu na rodzaj energii zasilającej

| Typ | Zasada | Cechy | Zastosowania |
| :--- | :--- | :--- | :--- |
| **Elektryczne** | energia elektryczna → ruch/siła | łatwe sterowanie, czyste, precyzyjne | silniki, elektrozawory, przekaźniki, solenoidy |
| **Pneumatyczne** | sprężone powietrze | szybkie, lekkie, bezpieczne w strefach wybuchowych; wymaga sprężarki | automatyka przemysłowa, chwytaki |
| **Hydrauliczne** | ciecz pod ciśnieniem | bardzo duże siły | maszyny budowlane, prasy |
| **Piezoelektryczne** | odkształcenie kryształu w polu elektrycznym | bardzo precyzyjne, szybkie, małe skoki | pozycjonowanie, wtryskiwacze, buzzery |
| **Termiczne** | rozszerzalność cieplna, stopy z pamięcią kształtu (SMA), bimetale | proste, wolne | zawory termiczne, aktuatory SMA |
| **Magnetostrykcyjne/elektroaktywne** (polimery EAP, MEMS) | zmiana kształtu pod wpływem pola | miniaturyzacja | mikrosystemy, robotyka miękka |

### Ze względu na rodzaj ruchu

- **obrotowe** – silniki DC, BLDC, **krokowe**, serwomechanizmy,
- **liniowe** – siłowniki, solenoidy, silniki liniowe, śruby kulowe,
- **dwustanowe** (on/off) – przekaźniki, styczniki, elektrozawory dwupołożeniowe,
- **ciągłe (proporcjonalne)** – serwozawory, silniki z regulacją prędkości.

### Najczęstsze aktuatory elektryczne w systemach wbudowanych

| Aktuator | Zasada, sterowanie |
| :--- | :--- |
| **Silnik prądu stałego (DC)** | prędkość ∝ napięcie → regulacja **PWM**; kierunek – **mostek H** |
| **Silnik bezszczotkowy (BLDC)** | komutacja elektroniczna (ESC), wysoka sprawność (drony, wentylatory) |
| **Silnik krokowy** | ruch w dyskretnych krokach (np. 200 kroków/obr.), sterowanie sekwencją impulsów (A4988, DRV8825); pozycjonowanie bez enkodera (drukarki 3D, CNC) |
| **Serwomechanizm** | silnik + przekładnia + czujnik położenia + regulator; kąt zadawany **PWM** (okres 20 ms, impuls 1–2 ms) |
| **Przekaźnik / SSR** | łączenie obwodów dużej mocy (230 V) sygnałem małej mocy |
| **Elektrozawór** | cewka otwiera/zamyka przepływ (woda, powietrze) |
| **Solenoid (elektromagnes)** | ruch liniowy, zamki elektromagnetyczne |
| **Grzałka / element Peltiera** | wytwarzanie/odbiór ciepła (regulacja PWM/SSR) |
| **Diody LED, wyświetlacze, buzzery, głośniki** | aktuatory świetlne i akustyczne |

## Parametry aktuatorów

siła/moment, prędkość, **skok** (zakres ruchu), **dokładność i powtarzalność** pozycjonowania, rozdzielczość, **czas reakcji**, sprawność, moc i napięcie zasilania, pobór prądu (także rozruchowy), **cykl pracy (duty cycle)**, trwałość (liczba cykli), hałas, wymiary i masa, odporność środowiskowa (IP).

## Sterowanie – otwarte i zamknięte

- **w pętli otwartej**: aktuator dostaje polecenie bez sprawdzania wyniku (np. silnik krokowy zakładający, że kroki nie giną),
- **w pętli zamkniętej**: czujnik (enkoder, czujnik położenia) mierzy efekt, **regulator (np. PID)** koryguje sterowanie → większa dokładność i odporność na zakłócenia (serwo).

## Aktuator a sensor

| | Sensor | Aktuator |
| :--- | :--- | :--- |
| Kierunek konwersji | zjawisko fizyczne → sygnał | sygnał → zjawisko fizyczne |
| Rola w systemie | „zmysły" | „mięśnie" |
| Moc | mała | często znaczna (wymaga drivera) |
| Przykłady | termistor, akcelerometr | silnik, zawór, przekaźnik |

W technologii **MEMS** i w robotyce element może pełnić obie role (np. element piezoelektryczny).

## Podsumowanie

- Aktuator zamienia **sygnał sterujący i energię** na **działanie fizyczne** (ruch, siła, ciepło, światło, dźwięk) – wykonuje decyzje układu sterowania.
- Klasyfikacje: według energii (elektryczne, pneumatyczne, hydrauliczne, piezo, termiczne), rodzaju ruchu (obrotowe, liniowe, dwustanowe, ciągłe).
- W IoT najczęściej: silniki DC/BLDC/krokowe, serwa, przekaźniki, elektrozawory, LED, buzzery – sterowane przez **drivery** (MOSFET, mostek H) i **PWM**.
- Wymagają dopasowania mocy, ochrony (dioda zwrotna, separacja) i często sprzężenia zwrotnego (PID).

---
[⬅️ Poprzedni temat](8_Cechy_sensora_inteligentnego.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Modulacja_szerokości_impulsów_PWM.md)