# Wnioskowanie statystyczne. ANOVA, MANOVA – postawienie zagadnienia, przykłady zastosowań

> **💬 Gotowa wypowiedź ustna:**
> *"ANOVA służy do jednoczesnego porównywania średnich w trzech lub więcej grupach. Używamy jej zamiast serii testów t-Studenta, aby zapobiec kumulacji błędu pierwszego rodzaju. Działa tak, że porównuje zmienność między średnimi grup a zmiennością wewnątrz grup. Jeśli różnice między grupami są znacznie większe niż szum wewnątrz nich, odrzucamy hipotezę o równości średnich. Do założeń należą rozkład normalny w grupach i jednorodność wariancji. Ponieważ ANOVA informuje tylko, że różnica istnieje, po jej wykonaniu stosujemy testy post-hoc, np. test Tukeya, aby ustalić, które konkretnie grupy się różnią.
> 
> MANOVA jest rozszerzeniem tej metody na sytuacje, w których badamy jednocześnie wiele zmiennych zależnych. Zamiast porównywać pojedyncze średnie, porównuje całe wektory średnich, uwzględniając współzależności między cechami, np. przy użyciu Lambdy Wilksa."*

## 1. ANOVA (Jednowymiarowa analiza wariancji) – postawienie zagadnienia

* **Cel metody:** Służy do weryfikacji hipotezy o jednoczesnej równości wartości średnich badanej cechy ilościowej w więcej niż dwóch (\\(k > 2\\)) populacjach.
* **Formułowanie hipotez:**
  * **Hipoteza zerowa (($H_0$)):** ($mu_1$ = $mu_2$ = $\dots$ = $mu_k$) (wszystkie średnie w grupach są równe, brak wpływu badanego czynnika).
  * **Hipoteza alternatywna (($H_1$)):** co najmniej dwie wartości średnie różnią się między sobą.
* **Dlaczego ANOVA, a nie seria testów t-Studenta?** Wielokrotne wykonywanie testu t-Studenta dla par grup powoduje drastyczny wzrost prawdopodobieństwa popełnienia błędu I rodzaju (skumulowany poziom istotności). ANOVA pozwala przeanalizować wszystkie grupy jednocześnie.
* **Podstawowe założenia:**
  * Zmienna zależna jest mierzone na skali ilościowej.
  * Populacje/próby są od siebie niezależne.
  * Badana zmienna ma rozkład normalny w każdej z grup.
  * Wariancje w porównywanych grupach są jednorodne (homoscedastyczność).
* **Testy post-hoc:** Sama ANOVA informuje jedynie o istnieniu istotnej różnicy, ale nie wskazuje, które konkretnie grupy się różnią. Do zidentyfikowania różnic między poszczególnymi parami średnich stosuje się testy porównań wielokrotnych (np. test Tukeya, Bonferroniego czy Scheffégo).

---

## 2. MANOVA (Wielowymiarowa analiza wariancji) – postawienie zagadnienia

* **Cel metody:** Jest rozszerzeniem analizy ANOVA na sytuacje, w których badamy jednocześnie **wiele ciągłych zmiennych zależnych** w odniesieniu do jednej lub wielu zmiennych niezależnych (czynników).
* **Formułowanie hipotez:** Testuje hipotezę zerową o równości **wektorów wartości średnich** dla połączonych zmiennych zależnych w porównywanych grupach.
* **Założenia:** Wielowymiarowy rozkład normalny w populacjach oraz jednorodność macierzy kowariancji.
* **Statystyki testowe:** Do weryfikacji hipotez wielowymiarowych wykorzystuje się m.in. Lambdę Wilksa, Ślad Pillai'a, Ślad Hotellinga oraz Pierwiastek Roya.

---

## 3. Przykłady zastosowań

* **Przykład ANOVA jednoczynnikowej:** Sprawdzenie, czy trzy odmiany irysów (*Setosa*, *Versicolor*, *Virginica*) różnią się istotnie średnią długością działki kielicha.
* **Przykład ANOVA dwuczynnikowej:** Badanie wpływu dwóch czynników naraz – poziomu wykształcenia oraz płci – a także ich wzajemnej interakcji na poziom zainteresowania polityką.
* **Przykład MANOVA:** Jednoczesna ocena, czy trzy odmiany irysów różnią się pod względem kompleksowego zestawu czterech cech morfologicznych jednocześnie (długości i szerokości działki oraz długości i szerokości płatka).

## Podsumowanie

- Wnioskowanie = estymacja + testowanie hipotez; kluczowe: $H_0$, $\alpha$, $p$-wartość, błędy I i II rodzaju.
- Testy parametryczne wymagają normalności (i jednorodności wariancji); gdy nie są spełnione – nieparametryczne (rangi).
- **ANOVA**: $H_0:\ \mu_1=\dots=\mu_k$; rozkład $SS_{total}=SS_{between}+SS_{within}$; $F=MS_b/MS_w$; po istotnym $F$ – post-hoc (Tukey).
- Warianty: jednoczynnikowa, powtarzane pomiary, dwuczynnikowa (bez/z powtórzeniami, interakcja); nieparametryczne: Kruskal-Wallis, Friedman.
- **MANOVA**: wiele zmiennych zależnych naraz, testy Wilksa/Pillaia/Hotellinga/Roya; wymaga wielowymiarowej normalności i jednorodności macierzy kowariancji.

---
[⬅️ Poprzedni temat](2_Metody_estymacji_gęstości_rozkładu_prawdopodobieństwa.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Modele_regresji_Regresja_wielokrotna.md)