# Metody porównania modeli uczenia maszynowego. Omów na przykładzie

> **💬 Gotowa wypowiedź ustna:**
> *"Stosujemy testy statystyczne do porównywania modeli ML, aby sprawdzić, czy różnica w ich wynikach jest statystycznie istotna, czy wynika jedynie z losowego podziału danych. Zwykły test t-Studenta nie nadaje się do walidacji krzyżowej, ponieważ nakładanie się zbiorów w kolejnych krokach łamie założenie o niezależności próby i prowadzi do fałszywych wniosków.
> 
> Dla dwóch klasyfikatorów na jednym zbiorze testowym stosuje się test McNemara. Jest to test nieparametryczny, który analizuje wyłącznie te przypadki, w których modele dały odmienne odpowiedzi. Buduje się tabelę niezgodności i oblicza statystykę $\chi^2$. Jeśli wyliczona wartość przekracza wartość krytyczną, odrzucamy hipotezę o jednakowej dokładności modeli i stwierdzamy istotną różnicę w ich działaniu."*

## 1. Cel stosowania testów statystycznych do porównywania modeli

* **Problem prostej oceny:** Porównanie wyłącznie wartości wskaźników (np. dokładności czy błędu klasyfikacji) dla pojedynczego podziału na zbiór treningowy i testowy nie wystarcza, ponieważ różnice mogą wynikać z przypadkowego losowania danych.
* **Potrzeba weryfikacji hipotez:** Statystyczne testy istotności pozwalają ocenić, czy obserwowana różnica w wynikach modeli jest statystycznie istotna, czy wynika jedynie ze zmienności losowej.
* **Ważne ograniczenie klasycznego testu $(t)$-Studenta w ML:** W walidacji krzyżowej ($(k)$-fold CV) zbiory treningowe i testowe w kolejnych iteracjach nakładają się na siebie, co narusza założenie o niezależności obserwacji. Powoduje to niedoszacowanie wariancji i sztuczne zawyżenie błędu I rodzaju (często wykrywa się "istotną" różnicę tam, gdzie jej nie ma).

---

## 2. Przegląd znanych metod

* **Dla dwóch modeli na jednym zbiorze danych:**
  * **Test McNemara:** nieparametryczny test oparty na statystyce $(\chi^2)$ do porównywania wyników klasyfikacji dwóch modeli na tym samym zbiorze testowym.
  * **Skorygowany sparowany test $(t)$-Studenta (Nadeau i Bengio):** modyfikacja testu $(t)$, w której koryguje się estymację wariancji różnic ze względu na nakładanie się zbiorów w walidacji krzyżowej.
* **Dla dwóch modeli na wielu zbiorach danych:**
  * **Test rangowanych znaków Wilcoxona (Wilcoxon signed-rank test):** nieparametryczna alternatywa dla testu $(t)$ do porównywania dwóch klasyfikatorów na wielu zbiorach danych.
  * **Test znaków (Sign test):** prosty test nieparametryczny zliczający, ile razy dany model wygrał z drugim.
* **Dla wielu modeli ($(m > 2)$) na wielu zbiorach danych ($(n > 2)$):**
  * **Test Friedmana (z modyfikacją Imana i Davenporta):** nieparametryczny test bazujący na rangach modeli.
  * **Test post-hoc Nemenyi:** stosowany po odrzuceniu hipotezy zerowej w teście Friedmana, pozwalający wyznaczyć, które konkretnie modele różnią się między sobą.

---

## 3. Omówienie wybranej metody na przykładzie: Test McNemara

* **Zasada działania:**Test McNemara służy do porównywania dwóch klasyfikatorów binarnych ocenianych na tym samym zbiorze testowym. Analizuje wyłącznie te przypadki, w których modele dały odmienne odpowiedzi (jedna poprawna, druga błędna).
* **Sformułowanie hipotez i tabela kontyngencji:**Wyniki klasyfikacji zestawiamy w tabeli $(2 \times 2)$:

  * $(n_{11})$ – oba modele sklasyfikowały przykład poprawnie.
  * $(n_{00})$ – oba modele popełniły błąd.
  * $(n_{10})$ – Model 1 poprawny, Model 2 błędny.
  * $(n_{01})$ – Model 1 błędny, Model 2 poprawny.

  **Hipoteza zerowa ($(H_0)$):** $(n_{10} = n_{01})$ (oba modele popełniają błędy z taką samą częstotliwością, brak istotnej różnicy między modelami).
* **Statystyka testowa (z poprawką na ciągłość):**
  $[\chi^2 = \frac{(|n_{01} - n_{10}| - 1)^2}{n_{01} + n_{10}}$]
  Statystyka ta ma rozkład $(\chi^2)$ z $(1)$ stopniem swobody ($(df = 1)$).
* **Przykład zastosowania:**Porównujemy dwa klasyfikatory: Liniową Analizę Dyskryminacyjną (LDC) oraz Kwadratową Analizę Dyskryminacyjną (QDC) w zadaniu rozpoznawania odmian kwiatów Iris (*Versicolor* vs *Virginica*).

  1. Po przeprowadzeniu klasyfikacji dla zbioru testowego otrzymujemy tabelę odmiennych rozstrzygnięć:
     * Liczba przypadków, gdy QDC był poprawny, a LDC błędny: $(n_{01} = 6)$.
     * Liczba przypadków, gdy LDC był poprawny, a QDC błędny: $(n_{10} = 0)$.
  2. Obliczamy wartość statystyki testowej:
     $[\chi^2 = \frac{(|6 - 0| - 1)^2}{6 + 0} = \frac{25}{6} \approx 4.17$]
  3. Odczytujemy wartość krytyczną z tablic rozkładu $(\chi^2)$ dla $(\alpha = 0.05)$ i $(df = 1)$, która wynosi $(3.841)$.
  4. **Wnioskowanie:** Ponieważ $(4.17 > 3.841)$, odrzucamy hipotezę zerową $(H_0)$ na korzyść $(H_1)$. Stwierdzamy, że klasyfikatory LDC i QDC różnią się w sposób statystycznie istotny.

---

## Podsumowanie

- Samo „A ma wyższą dokładność” nie wystarcza – trzeba sprawdzić **istotność** różnicy.
- 2 modele, 1 zbiór: **McNemar** (zgodność odpowiedzi) lub sparowany $t$ **z korektą Nadeau-Bengio** (zwykły test $t$ jest niepoprawny przez zależność foldów).
- 2 modele, wiele zbiorów: **Wilcoxon**, test znaków.
- $>2$ modeli, wiele zbiorów: **Friedman** (+ Iman-Davenport), post-hoc **Nemenyi**.
- Poza testami: porównanie miar jakości w walidacji krzyżowej, kosztu, interpretowalności i złożoności.

---

[⬅️ Poprzedni temat](7_Systemy_rekomendacji.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](9_Ocena_jakości_modeli_klasyfikacyjnych.md)
