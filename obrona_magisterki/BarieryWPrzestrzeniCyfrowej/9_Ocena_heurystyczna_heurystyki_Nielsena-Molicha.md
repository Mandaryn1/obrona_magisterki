# Ocena heurystyczna – heurystyki Nielsena-Molicha

## Ocena heurystyczna

**Ocena heurystyczna** (*heuristic evaluation*) – metoda **ekspercka (bez udziału użytkowników)**: **eksperci oceniają oprogramowanie, wykorzystując standardowy zestaw zasad dobrego interfejsu – heurystyk**.

**Heurystyka** – zbiór **optymalnych (quasi-optymalnych) zasad** wynikających z:

- **doświadczenia**,
- **zdrowego rozsądku**,
- **dobrych praktyk**,
- **fizycznych właściwości człowieka**.

Heurystyka uwzględnia **kontekst użycia** oprogramowania. Ocena wskazuje **odchylenia od zasad**, czyli potencjalne problemy użyteczności.

### Etapy oceny heurystycznej

1. **Planowanie** – wybór heurystyki i kontekstu użycia, **zaznajomienie ekspertów**, opracowanie **scenariuszy**.
2. **Realizacja badań** – eksperci **niezależnie od siebie** wykonują zadania i odnotowują **wszystkie odchylenia** od zasad heurystyki; **oceniają istotność** problemów.
3. **Analiza wyników** – **scalenie list** problemów opracowanych przez ekspertów, oszacowanie stopy wykrytych problemów.
4. **Raport** (problemy, ich waga, rekomendacje poprawy).

### Efektywność

- **Jeden ekspert wykrywa ok. 35%** problemów – dlatego potrzebnych jest kilku niezależnych ekspertów (*uzupełnienie*: zwykle **3–5**; każdy kolejny dodaje coraz mniej).
- Badanie na **wczesnych etapach projektowania** daje informację zwrotną i **zmniejsza liczbę problemów przed testami z użytkownikami**, a więc **obniża koszty** tych testów.

*(uzupełnienie)* **Skala dotkliwości problemów** (Nielsen, 0–4): 0 – nie jest to problem; 1 – kosmetyczny; 2 – mały; 3 – duży (priorytet wysoki); 4 – katastrofalny (obowiązkowo naprawić). W materiałach lekcyjnych stosuje się też skalę 1–5 (lista LUT) i klasyfikację: **krytyczne / istotne / małoistotne**.

## Heurystyki Nielsena-Molicha

Opracowane na podstawie badań statystycznych dotyczących **prawidłowej interakcji człowiek–maszyna** o możliwie najszerszym spektrum zastosowań. **Lista 10 zaleceń**, których spełnienie jest oceniane przez ekspertów (*Molich i Nielsen, 1990; Nielsen, 1994*).

| Nr | Heurystyka | Treść |
| :-: | :--- | :--- |
| **1** | **Widoczny status systemu** | system **zawsze informuje** użytkownika o swoim stanie za pomocą stosownych, zrozumiałych elementów i **odpowiednio szybko**, bez zbędnych opóźnień |
| **2** | **Zgodność pomiędzy systemem a rzeczywistością** | język i **terminologia** zrozumiałe dla użytkownika; informacje w **logicznym, naturalnym porządku**; zrozumiałe konwencje multimedialne (metafory graficzne) |
| **3** | **Kontrola i swoboda działań użytkownika** | prosta możliwość **powrotu** do poprzedniego położenia (nawigacja, błędny wybór); „ucieczka" nie wymaga długiego dialogu, jasno oznaczona i łatwo dostępna |
| **4** | **Zachowanie jednakowych konwencji w obrębie serwisu** (spójność) | te same słowa, symbole i elementy graficzne w całym oprogramowaniu; bez nietypowych elementów graficznych/behawioralnych; najlepiej **konwencje platformy** |
| **5** | **Zapobieganie błędom** | dialog zaprojektowany tak, by **zapobiegać błędom**; twórcy powinni **chronić** użytkownika i aplikację przed popełnieniem błędów, a nie tylko je obsługiwać |
| **6** | **Rozpoznawanie, a nie zapamiętywanie** | użytkownik **nie musi pamiętać** informacji przy przechodzeniu między częściami aplikacji; potrzebne dane i instrukcje **widoczne na ekranie** – nie obciążać pamięci krótkotrwałej |
| **7** | **Elastyczność i efektywność** | doświadczeni użytkownicy mają **przyspieszony dostęp** do funkcji (skróty); możliwość wyboru najbardziej odpowiedniego sposobu wykonania zadania spośród wielu |
| **8** | **Estetyka i minimalizm interfejsu** | brak elementów **zbędnych** w danym momencie, utrudniających zrozumienie; interfejs zgodny z powszechnymi kanonami estetyki |
| **9** | **Właściwa obsługa błędów** | komunikaty **proste i zwięzłe**, wskazujące **typ problemu** i **sposób rozwiązania**; **bez kodów błędów** |
| **10** | **Pomoc i dokumentacja** | interfejs **samowyjaśniający się** (używalny bez dokumentacji), a jednocześnie z pomocą i dokumentacją w zakresie niezbędnych zadań; dostęp **prosty i intuicyjny**, niezajmujący więcej czasu niż to konieczne |

### Przykłady naruszeń *(uzupełnienie)*

| Heurystyka | Naruszenie | Poprawa |
| :--- | :--- | :--- |
| 1 | brak wskaźnika postępu po kliknięciu „Wyślij" | pasek postępu, komunikat „Wysyłanie…" |
| 3 | brak „Anuluj"/„Cofnij" | przycisk wstecz, „Cofnij usunięcie" |
| 4 | raz „Zapisz", raz „Zatwierdź" dla tej samej akcji | jednolite nazewnictwo |
| 5 | pole „Data" przyjmuje dowolny tekst | selektor daty, maska, walidacja na bieżąco |
| 9 | „Błąd 0x80070005" | „Nie masz uprawnień do zapisu w tym folderze. Wybierz inną lokalizację." |

## Listy kontrolne a heurystyki

Heurystyki są **ogólne**; **listy kontrolne** (LUT, WCAG, inspekcja standardów) są **bardziej szczegółowe** i formalne (pytania tak/nie lub skala) – zob. temat 7. Ocena heurystyczna daje więcej problemów „nieoczywistych", lista kontrolna – bardziej powtarzalna.

## Zalety i wady

| Zalety | Wady |
| :--- | :--- |
| szybka i **tania**, bez rekrutacji użytkowników | **subiektywizm** ekspertów; wynik zależy od ich kompetencji |
| możliwa na **wczesnym etapie** (szkice, prototypy) | jeden ekspert wykrywa tylko ok. 35% problemów – potrzebnych kilku |
| wskazuje **konkretne naruszenia zasad** i sposób poprawy | **fałszywe alarmy** (problemy, których użytkownicy nie doświadczają) |
| zmniejsza koszt późniejszych testów | nie wykrywa problemów wynikających z kontekstu użycia i zachowań, które znają tylko użytkownicy |
| powtarzalna, dobrze opisana procedura | nie zastępuje testów z użytkownikami; ogólnikowa |

## Podsumowanie

- Ocena heurystyczna = **eksperci niezależnie** sprawdzają interfejs względem zestawu zasad (heurystyk), notują odchylenia, oceniają ich istotność, scalają listy i tworzą raport.
- **10 heurystyk Nielsena-Molicha:** status systemu, zgodność z rzeczywistością, kontrola i swoboda, spójność, zapobieganie błędom, rozpoznawanie zamiast zapamiętywania, elastyczność i efektywność, estetyka i minimalizm, obsługa błędów, pomoc i dokumentacja.
- Jeden ekspert wykrywa ok. 35% problemów; metoda tania i wczesna, ale subiektywna – uzupełnia testy z użytkownikami.

---
[⬅️ Poprzedni temat](8_Metodyka_SUS.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Okulografia_idea_istota_urządzenia_eksperyment_rezultaty.md)