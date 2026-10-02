# Porównaj klasyczne szyfry podstawieniowe i przestawieniowe z nowoczesnymi algorytmami kryptograficznymi. Jak zmieniło się podejście do bezpieczeństwa szyfrów?

**Klasyczne szyfry** dzielą się na dwie grupy:

- **Podstawieniowe** zastępują każdy znak innym, np. szyfr Cezara (przesunięcie o stałą liczbę liter) czy Vigenère'a (przesunięcie zależne od słowa-klucza).
- **Przestawieniowe** nie zmieniają liter, tylko ich kolejność, np. szyfr kolumnowy.

Mają poważne słabości. Podstawieniowe zachowują **statystykę języka**, więc łamie się je **analizą częstości** liter. Vigenère'a łamie test Kasiskiego, który wyznacza długość klucza. Szyfry przestawieniowe zachowują rozkład liter, a przestrzeń kluczy jest zwykle mała i da się ją przeszukać. Ich bezpieczeństwo opierało się na **tajności metody** i intuicji twórcy.

**Nowoczesne algorytmy** (AES, ChaCha20, RSA, ECC) działają inaczej:

- Łączą oba pomysły wielokrotnie, w wielu rundach, zgodnie z zasadami Shannona: **dezorientacja** (zależność szyfrogramu od klucza jest bardzo skomplikowana) i **dyfuzja** (zmiana jednego bitu wpływa na cały szyfrogram).
- Pracują na **bitach i blokach**, a nie na literach, więc statystyka języka nie ma znaczenia.
- Używają **dużych kluczy** (np. 128–256 bitów), więc przeszukanie wszystkich jest niewykonalne.
- Są **publiczne i otwarcie analizowane**, zgodnie z **zasadą Kerckhoffsa**: bezpieczeństwo zależy wyłącznie od klucza.

**Jak zmieniło się podejście do bezpieczeństwa:**

- **Z tajności algorytmu na tajność klucza.** Algorytm jest jawny i latami testowany przez społeczność (konkursy NIST).
- **Z intuicji na formalne definicje i dowody.** Mówimy o odporności na modele ataku (IND-CPA, IND-CCA) i o redukcji do trudnych problemów matematycznych, np. faktoryzacji czy logarytmu dyskretnego.
- **Z samej poufności na szersze cele.** Chronimy też integralność i autentyczność (szyfrowanie uwierzytelnione AEAD, np. AES-GCM), a dodatkowo mamy podpisy i kryptografię klucza publicznego.

W skrócie: klasyczne szyfry były „sprytnymi sztuczkami", które łamie się na kartce papieru, a nowoczesne mają mierzalny poziom bezpieczeństwa wynikający z wielkości klucza i trudności matematycznej.

## Podsumowanie

- **Klasyczne:** podstawieniowe (Cezar, Vigenère, Playfair, Hill) i przestawieniowe (płotowy, kolumnowy); łamane analizą statystyczną, małe lub okresowe klucze, brak formalnego uzasadnienia.
- **Nowoczesne:** operacje na bitach, duże klucze, S-boksy i permutacje (dezorientacja i dyfuzja), kryptografia asymetryczna, skróty, podpisy.
- **Zmiana podejścia:** od tajności metody do **tajności klucza** (Kerckhoffs), od intuicji do **formalnych definicji i dowodów**, od COA do **CPA/CCA**, od ukrywania do **publicznej weryfikacji** i bezpieczeństwa mierzonego w bitach.

---
[⬅️ Poprzedni temat](4_Tajność_doskonała_i_szyfr_z_kluczem_jednorazowym.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_Tryby_pracy_szyfrów_blokowych.md)