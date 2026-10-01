# Technologie wspomagające osoby z niepełnosprawnościami

## Pojęcia

Ograniczenia użytkowników przeciwdziała się za pomocą **technologii wspomagających lub adaptacyjnych**:

- **Technologia wspomagająca (assistive technology)** – „każdy obiekt lub system, który **zwiększa lub utrzymuje zdolności** osób niepełnosprawnych". Wspomaga i ułatwia interakcję człowiek–komputer, **nie zastępując** wprowadzania danych przez użytkownika.
- **Technologia adaptacyjna (adaptive technology)** – „każdy obiekt lub system, który jest **specjalnie zaprojektowany** w celu zwiększenia lub utrzymania możliwości osób niepełnosprawnych" (urządzenia i oprogramowanie specjalnie dostosowane).

W praktyce granica jest płynna; w dostępności cyfrowej mówi się o **technologiach asystujących**, które działają na styku użytkownika i systemu (np. czytnik ekranu czyta interfejs aplikacji). Aby technologie wspomagające działały, interfejs musi być **poprawnie zbudowany** (semantyczny HTML, tekst alternatywny, atrybuty ARIA – zob. temat 5).

## Ograniczenia motoryczne

**Technologie wspomagające** – ułatwiają wprowadzanie danych:

- **skróty klawiszowe**,
- **gesty** myszy, rąk lub innych części ciała,
- **tekst predykcyjny** (zwykle słownikowy) – zmniejsza liczbę wciśnięć klawiszy,
- **rozpoznawanie mowy** (dyktowanie, sterowanie głosem).

**Technologie adaptacyjne** *(uzupełnienie – przykłady)*:

- alternatywne klawiatury (ekranowe, uproszczone, jednoręczne, z dużymi klawiszami, osłony klawiatury), **trackballe**, specjalne **myszy** i joysticki,
- **przełączniki** (switch access) z **skanowaniem** elementów ekranu,
- sterowanie **głową** (wskaźniki, kamery), **wzrokiem** (eyetracker jako urządzenie wejściowe – temat 10), „dmuchanie i zasysanie" (sip-and-puff),
- sterowanie **głosem** (np. Voice Control, Dragon).

## Zaburzenia widzenia

Dwie kategorie użytkowników: **niewidomi** oraz **widzący, ale z wadami wzroku**.

**Technologie wspomagające:**

- **duże czcionki** (powiększanie tekstu w systemie i przeglądarce),
- **motywy i ikony o wysokim kontraście** (tryb wysokiego kontrastu, tryb ciemny),
- **oprogramowanie do powiększania ekranu** (lupy ekranowe),
- **czytniki ekranu** – programy odczytujące zawartość ekranu głosem syntetycznym lub wysyłające ją na linijkę brajlowską.

**Technologie adaptacyjne:**

- **monitory (linijki) brajlowskie** i **klawiatury brajlowskie**,
- *(uzupełnienie)* drukarki brajlowskie, notatniki brajlowskie, urządzenia do odczytu tekstu (OCR + synteza mowy), powiększalniki wideo (CCTV).

*(uzupełnienie)* **Popularne czytniki ekranu:** NVDA i JAWS (Windows), VoiceOver (macOS, iOS), TalkBack (Android), Narrator (Windows), Orca (Linux). Czytniki korzystają z **drzewa dostępności** (*accessibility tree*) tworzonego z kodu strony – stąd znaczenie poprawnej semantyki i ARIA.

## Zaburzenia słuchu *(uzupełnienie)*

- **napisy** (zamknięte/otwarte), **transkrypcje**, **napisy rozszerzone**, tłumaczenie na **polski język migowy (PJM)**,
- wizualne i wibracyjne sygnały alarmowe zamiast samych dźwięków,
- aparaty słuchowe, implanty, pętle indukcyjne, komunikatory tekstowe, automatyczne rozpoznawanie mowy do napisów na żywo.

## Ograniczenia poznawcze *(uzupełnienie)*

- **syntezatory mowy** (czytanie tekstu na głos), narzędzia wspierające czytanie (podkreślanie linii, dostosowanie odstępów i czcionki),
- narzędzia do **upraszczania tekstu**, tryb czytania, **łatwy język (ETR – easy to read)**,
- przypomnienia, planery, kalendarze, podpowiedzi kontekstowe, korekta pisowni.

## Podsumowanie według rodzaju ograniczenia

| Ograniczenie | Technologie wspomagające | Technologie adaptacyjne |
| :--- | :--- | :--- |
| **motoryczne** | skróty klawiszowe, gesty, tekst predykcyjny, rozpoznawanie mowy | alternatywne klawiatury/myszy, trackballe, przełączniki, sterowanie głową/wzrokiem |
| **wzrokowe** | duże czcionki, wysoki kontrast, lupy ekranowe, **czytniki ekranu** | monitory i klawiatury **brajlowskie** |
| **słuchowe** | napisy, transkrypcje, PJM | aparaty słuchowe, pętle indukcyjne |
| **poznawcze** | syntezator mowy, tryb czytania, uproszczony język | specjalistyczne oprogramowanie edukacyjne |

## Rola projektanta

Technologie wspomagające **nie zastąpią** dobrego projektu. Interfejs musi być:

- **zgodny ze standardami** (HTML, **WCAG**, **WAI-ARIA**), aby czytnik ekranu poprawnie rozpoznał strukturę, role i stany elementów,
- w pełni obsługiwany **klawiaturą**,
- niezależny od jednego zmysłu (informacja w kilku formach: tekst, dźwięk, kontrast, kształt),
- testowany z użyciem technologii wspomagających (i z udziałem osób z niepełnosprawnościami).

## Powiązania i podsumowanie

- Technologie wspomagające/adaptacyjne **kompensują ograniczenia człowieka** (temat 3), ale ich użycie zależy od jakości projektu (temat 5).
- Wspomagające = ułatwiają i **nie zastępują** interakcji (skróty, predykcja, powiększanie, czytnik); adaptacyjne = **specjalnie zaprojektowane** urządzenia/oprogramowanie (brajl, przełączniki).
- Główne technologie: **czytniki ekranu, lupy, wysoki kontrast, duże czcionki, rozpoznawanie mowy, tekst predykcyjny, monitory brajlowskie**.
