# Ocena jakości modeli klasyfikacyjnych

> **💬 Gotowa wypowiedź ustna:**
> *"Jakość modeli klasyfikacyjnych oceniamy na podstawie macierzy pomyłek, która zestawi decyzje modelu ze stanem faktycznym, dzieląc wyniki na TP, TN, FP i FN.
> 
> Na jej podstawie obliczamy metryki: Dokładność mierzy ogólny procent poprawnych odpowiedzi, ale bywa myląca przy nierównowadze klas. Precyzja mówi, ile zaklasyfikowanych przypadków pozytywnych było prawdziwych, a Czułość – jaki procent wszystkich rzeczywistych przypadków wyłapaliśmy. Średnią harmoniczną obu jest miara F1-score. Ogólną jakość niezależnie od progu odcięcia oceniamy zaś krzywą ROC i polem pod nią, czyli wskaźnikiem AUC."*

## 1. Macierz pomyłek (Confusion Matrix)

* **Podstawa ewaluacji:** Jest to tabela zestawiąca wartości rzeczywiste z wartościami przewidzianymi przez model klasyfikacyjny. Dla klasyfikacji binarnej składa się z 4 pól:
  * **TP (True Positive):** obserwacje pozytywne, poprawnie zaklasyfikowane jako pozytywne.
  * **TN (True Negative):** obserwacje negatywne, poprawnie zaklasyfikowane jako negatywne.
  * **FP (False Positive):** błąd I rodzaju – obserwacje w rzeczywistości negatywne, błędnie uznane za pozytywne.
  * **FN (False Negative):** błąd II rodzaju – obserwacje w rzeczywistości pozytywne, błędnie uznane za negatywne.

---

## 2. Podstawowe metryki jakości klasyfikacji

* **Dokładność (Accuracy):**  
  $[\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}]$  
  Mierzy udział wszystkich poprawnych predykcji w całkowitej liczbie obserwacji. Może być zwodnicza w przypadku zbiorów o silnej nierównowadze klas (np. gdy 99% danych to klasa negatywna).
* **Precyzja (Precision):**  
  $[\text{Precision} = \frac{TP}{TP + FP}]$  
  Określa, jaki procent obserwacji zaklasyfikowanych jako pozytywne rzeczywiście należy do klasy pozytywnej. Istotna, gdy koszt błędu FP jest wysoki (np. filtr spamu).
* **Czułość (Recall / Sensitivity):**  
  $[\text{Recall} = \frac{TP}{TP + FN}]$  
  Mierzy zdolność modelu do wykrywania rzeczywistych obiektów pozytywnych. Kluczowa w zastosowaniach medycznych i krytycznych, gdzie pomyłka FN jest niebezpieczna (np. niewykrycie choroby).
* **Swoistość / Specyficzność (Specificity):**  
  $[\text{Specificity} = \frac{TN}{TN + FP}]$  
  Mierzy zdolność modelu do poprawnej identyfikacji przypadków negatywnych.
* **F1-Score:**  
  $[\text{F1} = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}]$  
  Średnia harmoniczna precyzji i czułości. Równoważy obie te miary i stanowi najlepszy pojedynczy wskaźnik ogólnej jakości klasyfikacji przy niezbalansowanych danych.

---

## 3. Ocena modeli na podstawie prawdopodobieństw (ROC i AUC)

* **Krzywa ROC (Receiver Operating Characteristic):** Wykres przedstawiający relację między czułością (TPR) a wskaźnikiem fałszywych alarmów (FPR = 1 - Specyficzność) dla różnych progów odcięcia prawdopodobieństwa.
* **Wskaźnik AUC (Area Under Curve):** Pole powierzchni pod krzywą ROC:
  * $(AUC = 0.5)$ – model działa losowo (brak zdolności predykcyjnej).
  * $(AUC = 1.0)$ – klasyfikator idealny.
  * Zaletą AUC jest niezależność od wybranego punktu odcięcia (progu).

---

## 4. Techniki podziału danych do ewaluacji

* **Prosty podział (Train/Test Split):** Podział danych na zbiór treningowy i testowy (np. 80/20).
* **$(K)$-krotna walidacja krzyżowa (K-fold Cross-Validation):** Podział zbioru danych na $(k)$ równych części. Model uczony jest $(k)$-krotnie na $(k-1)$ częściach i oceniany na pozostałym 1 podzbiorze. Wynikiem końcowym jest średnia z $(k)$ prób, co zapobiega nadmiernemu dopasowaniu (overfittingowi) i zapewnia stabilną ocenę.

## Podsumowanie

- Podstawą jest **macierz pomyłek** (TP, FP, FN, TN) liczona na zbiorze testowym.
- Główne miary: accuracy, **precyzja**, **czułość**, specyficzność, **F1**, krzywa **ROC** i **AUC**.
- Przy klasach niezrównoważonych accuracy wprowadza w błąd – używamy precyzji/czułości/F1/AUC.
- Wieloklasowo: macierz $K\times K$, miary „jedna kontra reszta” i uśrednianie (macro/weighted/micro).
- Wiarygodna ocena: hold-out 80/20 i **k-krotna walidacja krzyżowa** (k=10), kontrola przeuczenia.

---
[⬅️ Poprzedni temat](8_Metody_porównania_modeli_uczenia_maszynowego.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Ocena_jakości_modeli_regresyjnych_i_prognozujących.md)