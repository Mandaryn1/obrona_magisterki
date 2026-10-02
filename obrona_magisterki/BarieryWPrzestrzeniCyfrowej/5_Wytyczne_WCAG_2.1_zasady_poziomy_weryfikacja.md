# Wytyczne dostępności treści internetowych WCAG 2.1 – zasady, poziomy, weryfikacja

**WCAG 2.1 (Web Content Accessibility Guidelines)** to międzynarodowy standard dostępności treści internetowych, opracowany przez **W3C (grupa WAI)** i opublikowany w 2018 r. Jest podstawą wymagań prawnych dla stron i aplikacji (w UE m.in. norma EN 301 549). Zawiera **13 wytycznych** i **78 kryteriów sukcesu**.

**Cztery zasady (POUR):**

1. **Postrzegalność (Perceivable):** informacje i elementy interfejsu muszą być podane w sposób, który użytkownik może odebrać. Przykłady: tekst alternatywny dla grafik, napisy do filmów, odpowiedni kontrast, brak informacji przekazywanej tylko kolorem.
2. **Funkcjonalność (Operable):** interfejs musi dać się obsługiwać. Przykłady: pełna obsługa klawiaturą, wystarczający czas na działanie, brak migotań grożących napadami, czytelna nawigacja.
3. **Zrozumiałość (Understandable):** treść i obsługa są zrozumiałe. Przykłady: określony język strony, przewidywalne działanie, pomoc przy błędach w formularzach.
4. **Solidność (Robust):** treść działa z różnymi przeglądarkami, urządzeniami i technologiami wspomagającymi dzięki poprawnemu, semantycznemu kodowi.

**Trzy poziomy zgodności:**

- **A (podstawowy):** minimalne wymagania (30 kryteriów).
- **AA (średni):** standard wymagany przez przepisy dla sektora publicznego (dodatkowe 20 kryteriów), np. kontrast tekstu 4,5:1.
- **AAA (najwyższy):** najwyższy poziom, trudny do spełnienia w całości (28 kryteriów), np. kontrast 7:1.

Zgodność na danym poziomie oznacza spełnienie wszystkich kryteriów tego i niższych poziomów. WCAG 2.1 uzupełniła wersję 2.0 m.in. o wymagania dla urządzeń mobilnych, osób słabowidzących i z ograniczeniami poznawczymi. Nowsza wersja **2.2** (2023) dodaje kolejne kryteria.

**Weryfikacja zgodności:**

- **Testy automatyczne:** WAVE, axe, Lighthouse, walidatory HTML/CSS, sprawdzanie kontrastu. Wykrywają jednak tylko część problemów.
- **Testy ręczne:** obsługa wyłącznie klawiaturą, powiększenie strony, ocena struktury nagłówków, sensowności tekstów alternatywnych i formularzy.
- **Testy z technologiami wspomagającymi:** czytniki ekranu (NVDA, JAWS), lupy.
- **Testy z użytkownikami** z niepełnosprawnościami.
- **Audyt dostępności** i **deklaracja dostępności** (w Polsce wymagana od podmiotów publicznych, z informacją o stopniu zgodności i ewentualnych wyłączeniach).

## Podsumowanie

- WCAG 2.1 (W3C): **4 zasady POUR** (postrzegalność, funkcjonalność, zrozumiałość, kompatybilność) → **13 wytycznych** → **kryteria sukcesu** (mierzalne).
- **Poziomy zgodności:** **A** (musi), **AA** (powinna – wymóg prawny), **AAA** (może).
- **ARIA** uzupełnia HTML o role, stany i właściwości dla dynamicznych aplikacji.
- Weryfikacja: narzędzia automatyczne (**WAVE**, W3C Validator) + listy kontrolne + testy z czytnikiem ekranu i z użytkownikami.

---
[⬅️ Poprzedni temat](4_Technologie_wspomagające_osoby_z_niepełnosprawnościami.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_Metody_oceny_jakości_interfejsu_klasyfikacja_typy_metod.md)