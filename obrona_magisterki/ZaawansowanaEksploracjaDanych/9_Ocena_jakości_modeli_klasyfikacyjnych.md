# Ocena jakości modeli klasyfikacyjnych

## Cel oceny (ewaluacji)

Sprawdzenie, czy model spełnia założenia projektowe, **poprawnie uogólnia** wzorce na nowe dane i jak można poprawić jego skuteczność. Polega na porównaniu przewidywań modelu z rzeczywistymi wartościami na **zbiorze testowym** (niewidzianym podczas uczenia).

Kryteria oceny modeli eksploracji danych: **łatwość interpretacji** (modele deskrypcyjne), **trafność i wiarygodność predykcji** (predykcyjne), **wydajność i skalowalność**, **przydatność**.

## Rodzaje klasyfikacji

| Rodzaj | Opis | Przykład |
| :--- | :--- | :--- |
| **dwuklasowa (binarna)** | dwie wzajemnie dopełniające się klasy; zwykle „pozytywna” = zjawisko pożądane/poszukiwane | choroba tak/nie, spam/nie-spam |
| **wieloklasowa** | $\ge3$ klas, każdy obiekt do jednej | rozpoznawanie gatunków, cyfr, twarzy |
| **wieloetykietowa** | obiekt może należeć do dowolnej liczby klas | obraz zawiera psa i/lub kota |
| **jednoklasowa** | wykrywanie obiektów jednej klasy, reszta to odstające | wykrywanie oszustw, anomalii |

## Macierz pomyłek (confusion matrix)

Dla klasyfikacji binarnej:

| | **Przewidziane: pozytywne** | **Przewidziane: negatywne** |
| :--- | :-: | :-: |
| **Rzeczywiste: pozytywne** | **TP** (true positive) | **FN** (false negative) |
| **Rzeczywiste: negatywne** | **FP** (false positive) | **TN** (true negative) |

- **TP** – poprawnie zaklasyfikowane jako pozytywne,
- **TN** – poprawnie zaklasyfikowane jako negatywne,
- **FP** – błędnie jako pozytywne (*błąd I rodzaju, fałszywy alarm*),
- **FN** – błędnie jako negatywne (*błąd II rodzaju, przeoczenie*).

## Miary jakości (klasyfikacja binarna)

| Miara | Wzór | Znaczenie |
| :--- | :--- | :--- |
| **Dokładność (accuracy, trafność)** | $\dfrac{TP+TN}{TP+TN+FP+FN}$ | odsetek poprawnych klasyfikacji |
| **Błąd klasyfikacji** | $1-\text{accuracy}=\dfrac{FP+FN}{TP+TN+FP+FN}$ | odsetek błędów |
| **Precyzja (precision, PPV)** | $\dfrac{TP}{TP+FP}$ | jaka część zaklasyfikowanych jako pozytywne jest naprawdę pozytywna |
| **Czułość (recall, sensitivity, TPR)** | $\dfrac{TP}{TP+FN}$ | jaka część rzeczywistych pozytywnych została wykryta |
| **Specyficzność (specificity, TNR)** | $\dfrac{TN}{TN+FP}$ | jaka część rzeczywistych negatywnych została rozpoznana |
| **Precyzja negatywna (NPV)** | $\dfrac{TN}{TN+FN}$ | trafność przewidywań negatywnych |
| **F1-score** | $\dfrac{2\cdot P\cdot R}{P+R}$ | **średnia harmoniczna** precyzji i czułości |
| **FPR** | $\dfrac{FP}{FP+TN}=1-\text{specyficzność}$ | odsetek fałszywych alarmów |

Uogólnienie: $F_\beta=(1+\beta^2)\dfrac{P\cdot R}{\beta^2P+R}$ ($\beta>1$ – większy nacisk na czułość, $\beta<1$ – na precyzję).

Uwagi:

- **Accuracy jest myląca dla klas niezrównoważonych** (np. 99% zdrowych: model „zawsze zdrowy” ma 99% dokładności, a czułość 0). Wtedy stosuje się precyzję, czułość, F1, AUC.
- **Wysoka precyzja nie oznacza dobrego modelu** – może być przy niskiej czułości (model pomija wiele przypadków). Występuje **kompromis precyzja–czułość** zależny od progu decyzyjnego.
- Co ważniejsze – zależy od zastosowania: w diagnostyce chorób zwykle **czułość** (nie przeoczyć chorego), w filtrowaniu spamu **precyzja** (nie wyrzucić ważnej poczty).

### Przykład liczbowy

$TP=40,\ FN=10,\ FP=5,\ TN=45$ ($N=100$):

- accuracy $=\frac{85}{100}=0{,}85$, błąd $=0{,}15$,
- precyzja $=\frac{40}{45}=0{,}889$, czułość $=\frac{40}{50}=0{,}80$, specyficzność $=\frac{45}{50}=0{,}90$,
- $F_1=\frac{2\cdot0{,}889\cdot0{,}8}{0{,}889+0{,}8}\approx0{,}842$.

## Krzywa ROC i AUC

Klasyfikator probabilistyczny zwraca prawdopodobieństwo; klasa zależy od **progu odcięcia** (domyślnie 0,5).

- **Krzywa ROC** (*Receiver Operating Characteristic*): wykres **czułości (TPR)** względem **FPR** $(=1-\text{specyficzność})$ dla wszystkich możliwych progów,
- punkt $(0,1)$ – klasyfikator idealny; przekątna – losowy,
- **AUC** (*Area Under Curve*) – pole pod krzywą ROC: $0{,}5$ – losowy, $1$ – idealny; interpretacja: prawdopodobieństwo, że losowy obiekt pozytywny dostanie wyższy wynik niż losowy negatywny. Nie zależy od progu, mniej wrażliwa na niezrównoważenie klas (dla dużej nierównowagi lepsza bywa krzywa precision–recall).

## Klasyfikacja wieloklasowa

**Macierz pomyłek $K\times K$**: wiersze – klasa prawdziwa, kolumny – przewidywana. Dla klasy $i$:

- $TP_i$ – komórka na przekątnej $(i,i)$,
- $FP_i$ – suma **kolumny** $i$ z pominięciem $TP_i$,
- $FN_i$ – suma **wiersza** $i$ z pominięciem $TP_i$,
- $TN_i$ – reszta macierzy.

Miary dla każdej klasy liczy się „jedna kontra reszta”, a następnie **uśrednia**:

| Uśrednianie | Opis |
| :--- | :--- |
| **macro** | średnia arytmetyczna miar klas (każda klasa tak samo ważna) |
| **weighted** (ważone) | średnia ważona liczebnością klas (**średnia ważona czułości, precyzji, F1**) |
| **micro** | sumowanie TP, FP, FN po klasach, a potem jedna miara globalna |

Dokładność wieloklasowa $=\dfrac{\sum_i TP_i}{N}$.

## Inne miary

- **Log loss** (entropia krzyżowa): ocenia jakość prawdopodobieństw (kara za pewne, ale błędne predykcje); jest funkcją kosztu regresji logistycznej,
- **Współczynnik kappa Cohena** $\kappa$: zgodność ponad przypadkową; **MCC** (Matthews) – zrównoważona miara także dla klas niezrównoważonych,
- **Accuracy zrównoważona** $=\frac{\text{czułość}+\text{specyficzność}}{2}$,
- macierz kosztów błędów (różne koszty FP i FN).

## Metody ewaluacji (protokoły)

| Metoda | Opis |
| :--- | :--- |
| **Hold-out (walidacja na odłożonych danych)** | podział na zbiór treningowy i walidacyjny/testowy, typowo **80% / 20%**; ocena na danych niewidzianych w uczeniu |
| **k-krotna walidacja krzyżowa (k-fold CV)** | zbiór na $k$ równych części; $k$ razy uczenie na $k-1$ i test na pozostałej; **średnia** wyników; zwykle $k=10$; lepsze wykorzystanie danych, ograniczenie przeuczenia; większe $k$ = większa dokładność, ale i koszt |
| **Stratyfikowana CV** | zachowuje proporcje klas w foldach (klasy niezrównoważone) |
| **Leave-one-out** | $k=n$; kosztowna, dla małych zbiorów |
| **Bootstrap** | losowanie ze zwracaniem |

Walidacja krzyżowa **nie zastępuje** podziału na zbiór treningowy i testowy, tylko go uzupełnia (ostateczny, niezależny zbiór testowy). **Przeuczenie (overfitting)**: model świetny na danych uczących, słaby na nowych – wykrywamy go porównując wyniki na zbiorze uczącym i testowym.

*Uwaga: reguła powrotu do średniej – nietypowe zjawiska z czasem zmierzają do średniej, więc model uczony na danych historycznych może gorzej działać na nowych.*

## Narzędzia (Python)

```python
from sklearn.metrics import confusion_matrix, classification_report, accuracy_score
from sklearn.metrics import roc_auc_score

y_pred = clf.predict(X_test)
print(confusion_matrix(y_test, y_pred))
print(classification_report(y_test, y_pred))     # precision, recall, F1, support
print(accuracy_score(y_test, y_pred))
print(roc_auc_score(y_test, clf.predict_proba(X_test)[:, 1]))   # AUC (binarna)
```

## Podsumowanie

- Podstawą jest **macierz pomyłek** (TP, FP, FN, TN) liczona na zbiorze testowym.
- Główne miary: accuracy, **precyzja**, **czułość**, specyficzność, **F1**, krzywa **ROC** i **AUC**.
- Przy klasach niezrównoważonych accuracy wprowadza w błąd – używamy precyzji/czułości/F1/AUC.
- Wieloklasowo: macierz $K\times K$, miary „jedna kontra reszta” i uśrednianie (macro/weighted/micro).
- Wiarygodna ocena: hold-out 80/20 i **k-krotna walidacja krzyżowa** (k=10), kontrola przeuczenia.
