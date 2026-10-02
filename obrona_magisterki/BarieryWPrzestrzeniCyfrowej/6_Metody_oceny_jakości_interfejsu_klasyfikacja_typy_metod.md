# Metody oceny jakości interfejsu – klasyfikacja, typy metod

**Po co oceniać interfejs:** żeby wykryć problemy użyteczności i dostępności, zmierzyć jakość, porównać warianty i uzasadnić zmiany. Ocenę powtarza się cyklicznie, na prototypie i na gotowym produkcie.

**Klasyfikacja metod:**

- **Automatyczne:** ocenę wykonują **narzędzia komputerowe**, np. walidatory kodu albo WAVE sprawdzający zgodność z WCAG. Są możliwe tam, gdzie istnieją wzorce poddające się algorytmizacji.
- **Manualne:** ocenę wykonuje **człowiek**. Dzielą się na:
  - **z udziałem użytkowników:** oceny pochodzą od grupy użytkowników,
  - **bez udziału użytkowników:** oceny pochodzą od ekspertów.

**Typy metod:**

- **Testowanie:** użytkownicy wykonują zaplanowane zadania według scenariuszy, a badacz obserwuje i mierzy (czas, błędy, sukces zadania). Etapy: plan badań, dobór uczestników, realizacja, opracowanie wyników.
- **Inspekcja:** eksperci przeglądają interfejs z użyciem list kontrolnych, kryteriów lub analizy heurystycznej (np. heurystyki Nielsena) i wykrywają potencjalne problemy.
- **Wywiad (ankieta):** informacje od użytkowników w formie wywiadów, ankiet i kwestionariuszy (np. SUS).
- **Modelowanie analityczne:** prognozowanie jakości na podstawie formalnych modeli interakcji, np. GOMS/KLM, prawo Fittsa. Jest pracochłonne i rzadko stosowane.
- **Symulacja:** komputerowe modele zachowań użytkownika. Wymaga specjalistycznego oprogramowania i jest rzadko stosowana.

**Metryki jakości:**

- **wydajnościowe:** sukces zadania, czas zadania, liczba błędów, łatwość nauki,
- **bazujące na problemach:** liczba i rodzaj wykrytych problemów,
- **bazujące na ocenach użytkowników:** oceny po zadaniu i po sesji (np. SUS),
- **behawioralne i fizjologiczne:** ruchy gałek ocznych (eyetracking), średnica źrenic, tętno.

Metryki odpowiadają trzem wymiarom użyteczności z ISO 9241-11: skuteczność, efektywność i satysfakcja. Do porównania interfejsów stosuje się też **globalne miary użyteczności** (np. SUS), często jako ważoną kombinację wielu wskaźników.

## Podsumowanie

- Metody: **automatyczne** (narzędzia) i **manualne** (z udziałem lub bez udziału użytkowników).
- **5 typów:** testowanie, inspekcja, wywiad, modelowanie analityczne, symulacja (ostatnie dwa rzadko stosowane).
- **4 grupy metryk:** wydajnościowe, bazujące na problemach, bazujące na ocenach użytkowników, behawioralne/fizjologiczne.
- Globalne miary (WUP, SUS, SUM) łączą wiele wskaźników; wymagają normalizacji i wag.

---
[⬅️ Poprzedni temat](5_Wytyczne_WCAG_2.1_zasady_poziomy_weryfikacja.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Techniki_oceny_jakości_interfejsów_z_udziałem_i_bez_udziału_użytkowników.md)