# Metodyka SUS (System Usability Scale)

**SUS (System Usability Scale)** to krótki, standardowy **kwestionariusz do subiektywnej oceny użyteczności** systemu. Opracował go John Brooke w 1986 r. Jest prosty, tani, szybki i niezależny od technologii (strony WWW, aplikacje, sprzęt). Daje **jedną liczbę od 0 do 100**, więc umożliwia porównanie interfejsów lub wersji. Należy do metod z udziałem użytkowników (pomiar satysfakcji po sesji testowej).

**Budowa:** 10 stwierdzeń, na które użytkownik odpowiada w **skali Likerta 1–5** (od „zdecydowanie się nie zgadzam" do „zdecydowanie się zgadzam"). Pytania są **naprzemiennie pozytywne i negatywne**, np.:

- „Chciałbym często korzystać z tego systemu" (nieparzyste, pozytywne),
- „System wydał mi się niepotrzebnie skomplikowany" (parzyste, negatywne).

**Obliczanie wyniku:**

- dla pytań **nieparzystych** (1, 3, 5, 7, 9): punkty = odpowiedź − 1,
- dla pytań **parzystych** (2, 4, 6, 8, 10): punkty = 5 − odpowiedź,
- sumujemy punkty z wszystkich pytań (0–40) i **mnożymy przez 2,5**, co daje wynik 0–100.

**Interpretacja:**

- **średni wynik na świecie to ok. 68 pkt**, więc poniżej tej wartości użyteczność uznaje się za niższą od przeciętnej,
- powyżej ok. 80 pkt oznacza dobry lub bardzo dobry wynik,
- poniżej ok. 50 pkt oznacza poważne problemy.

Wynik **nie jest procentem** i nie mówi, co dokładnie jest źle.

**Zalety:** szybkość (kilka minut), niski koszt, wiarygodność nawet przy małej próbie (ok. 8–12 osób), szerokie stosowanie i dostępne wartości odniesienia. **Wady:** nie wskazuje konkretnych problemów (trzeba uzupełnić testami lub wywiadem), jest subiektywny i daje tylko ogólną ocenę.

W praktyce SUS stosuje się **po teście z użytkownikami** razem z metrykami wydajnościowymi (czas, błędy) w celu pomiaru satysfakcji.

## Podsumowanie

- SUS = 10 pytań (Likert 1–5), na przemian pozytywne (nieparzyste) i negatywne (parzyste).
- Wzór: $SUS=\big(\sum_{nieparz.}(S_i-1)+\sum_{parz.}(5-S_i)\big)\cdot2{,}5$ → wynik **0–100**.
- Średnia dla 500 systemów = **68**; dwa czynniki: użyteczność (8 pozycji) i możliwość nauczenia (Q4, Q10).
- Szybka metoda oceny satysfakcji po wykonaniu scenariusza; uzupełnia metryki wydajnościowe i testy.

---
[⬅️ Poprzedni temat](7_Techniki_oceny_jakości_interfejsów_z_udziałem_i_bez_udziału_użytkowników.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](9_Ocena_heurystyczna_heurystyki_Nielsena-Molicha.md)