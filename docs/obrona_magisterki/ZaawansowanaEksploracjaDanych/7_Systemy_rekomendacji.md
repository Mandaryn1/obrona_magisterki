# Systemy rekomendacji. Wymień rodzaje systemów rekomendacji i omów przykładowy

> **💬 Gotowa wypowiedź ustna:**
> *"Systemy rekomendacji służą do personalizacji oferty i podpowiadania produktów na podstawie danych historycznych. Dzielimy je na metody oparte na treści, analizujące atrybuty produktów, oraz metody kolaboratywne, opierające się na zachowaniach całej społeczności.
> 
> W filtrowaniu kolaboratywnym system nie musi znać cech samego produktu. Wariacja User-to-User szuka użytkowników o podobnych ocenach i poleca produkty wybierane przez tych sąsiadów. Z kolei wariant Item-to-Item analizuje, które produkty są powtarzalnie oceniane razem przez tych samych ludzi. Główną zaletą jest elastyczność i niezależność od treści, a podstawowym wyzwaniem jest problem zimnego startu dla nowych użytkowników i produktów."*

## 1. Definicja i cel systemów rekomendacji

* **Definicja:** Systemy rekomendacji to algorytmy predykcyjne, które wybierają z bardzo dużego zbioru dostępnych opcji te obiekty (produkty, filmy, artykuły, reklamy), którymi dany użytkownik może być najbardziej zainteresowany.
* **Różnica względem wyszukiwarek:** Wyszukiwarki wymagają precyzyjnego zapytania od użytkownika. System rekomendacyjny działa wtedy, gdy użytkownik nie precyzuje kryteriów lub przegląda ofertę, a rekomendacje bazują na danych historycznych.

---

## 2. Rodzaje systemów rekomendacji

* **Filtrowanie oparte na treści (Content-Based Filtering):** Rekomenduje obiekty o cechach podobnych do tych, które użytkownik wybierał lub wysoko oceniał w przeszłości (wymaga znajomości atrybutów produktów).
* **Filtrowanie kolaboratywne / kolektywne (Collaborative Filtering):** Bazuje na zachowaniach i ocenach całej społeczności; szuka użytkowników o podobnych gustach i poleca to, co podobało się osobom z tej samej grupy.
* **Oparte na odkrywaniu asocjacji (analiza koszykowa):** Analizuje bazy transakcji i szuka wzorców współwystępowania obiektów (np. algorytm Apriori wyznaczający regułę: *"użytkownicy kupujący chleb kupują też mleko"*).
* **Oparte na demografii lub podsumowaniach statystycznych:** Sugeruje produkty najpopularniejsze, o najwyższej średniej ocen lub dopasowane do ogólnego profilu demograficznego (np. wiek, płeć).
* **Systemy hybrydowe:** Łączą kilka powyższych metod (np. kolaboratywną z opartą na treści), aby niwelować ich indywidualne wady.

---

## 3. Omówienie wybranego rodzaju: Filtrowanie Kolaboratywne (Collaborative Filtering)

* **Zasada działania:**  
  System nie musi znać ani analizować cech samego produktu (np. gatunku filmu czy opisu towaru). Analizuje jedynie **macierz ocen / interakcji** (użytkownik \\(\times\\) produkt) i szuka ukrytych podobieństw w zachowaniach ludzi.

* **Dwa podstawowe warianty:**
  * **User-to-User (orientacja na użytkownika):** Znajduje "sąsiadów" – osoby, które oceniały produkty w sposób najbardziej zbliżony do danego użytkownika, i poleca mu te przedmioty, które posiedli/ocenili sąsiedzi, a których ten użytkownik jeszcze nie zna.
  * **Item-to-Item (orientacja na przedmioty, np. opatentowane przez Amazon):** Wyznacza podobieństwo pomiędzy przedmiotami na podstawie tego, jak podobnie są oceniane przez wszystkich użytkowników. Jeśli produkty A i B są często kupowane przez tych samych ludzi, użytkownikowi kupującemu A proponuje się B.

* **Główne zalety:**
  * Niezależność od treści – działa tak samo dobrze dla książek, muzyki, produktów e-commerce czy filmów.
  * Potrafi zaproponować zupełnie nowe, zaskakujące kategorie produktów, którymi użytkownik może się zainteresować (*serendipity*).

* **Główne wady / wyzwania:**
  * **Problem "zimnego startu" (Cold Start):** Trudność w zarekomendowaniu czegokolwiek nowemu użytkownikowi (brak historii) lub polecenia nowego produktu, który nie ma jeszcze żadnych ocen.
  * **Rzadkość macierzy (Sparsity):** Większość użytkowników ocenia tylko znikomy promil produktów z oferty sklepu.

## Podsumowanie

- System rekomendacji przewiduje, co spodoba się użytkownikowi, nawet przy niepełnym zapytaniu.
- Rodzaje: wyszukiwanie, kategorie/klasyfikacja, grupowanie, filtrowanie cech przedmiotu (content-based), **filtrowanie kolaboratywne** (memory-/model-based, zimny start), asocjacje, wiedza/demografia, sesyjne, grafowe, **hybrydowe**.
- **Item-to-item CF (Amazon)**: macierz użytkownik–przedmiot → podobieństwo kosinusowe przedmiotów → polecanie podobnych do posiadanych.
- **Reguły asocjacyjne**: wsparcie, ufność, lift; Apriori wykorzystuje monotoniczność wsparcia (zbiór częsty ⇒ podzbiory częste).
- Historia: Tapestry (1992), GroupLens (1994), Amazon (1998), Netflix Prize (2006–09), Spotify, TikTok.

---
[⬅️ Poprzedni temat](6_Metody_analizy_skupień.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Metody_porównania_modeli_uczenia_maszynowego.md)