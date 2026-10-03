# Rola sztucznej inteligencji i uczenia maszynowego w ochronie sieci teleinformatycznych.

Po co **AI/ML w ochronie sieci:** sygnatury nie wykrywają nowych ataków (zero-day), ilość danych jest ogromna, ruch jest w większości szyfrowany, a ataki rozprzestrzeniają się szybko. AI/ML pomaga **wykrywać anomalie, ograniczać fałszywe alarmy i automatyzować reakcję**, a także odciąża analityków SOC.

**Rodzaje uczenia:**

- **nadzorowane:** uczenie na oznaczonych danych (atak/normalny), np. klasyfikacja ruchu, malware, phishingu (Random Forest, SVM, sieci neuronowe),
- **nienadzorowane:** wykrywanie odstępstw bez etykiet, np. **autoenkodery i Isolation Forest**, kluczowe dla nieznanych ataków,
- **głębokie uczenie (DNN, LSTM):** analiza ruchu i sekwencji,
- **uczenie federacyjne** (trening bez wymiany surowych danych),
- **LLM** jako asystenci SOC (streszczanie alertów, generowanie zapytań i reguł).

**Zastosowania:**

- **wykrywanie anomalii i analiza zachowania sieci (NBA/NDR):** model uczy się normalnego ruchu (baseline), a odchylenia wskazują na atak (np. nagle skanująca żarówka IoT),
- **IDS/IPS i NGFW:** metoda hybrydowa łącząca sygnatury z ML, identyfikacja aplikacji, analiza **ruchu szyfrowanego** po metadanych (np. JA3),
- **SIEM, UEBA i SOAR:** korelacja zdarzeń, ocena ryzyka użytkowników i urządzeń, **priorytetyzacja alertów** (walka ze zmęczeniem alertami) i **automatyczna reakcja** (izolacja hosta, blokada ruchu), co skraca czas z godzin do minut,
- **malware, phishing, DGA, tunelowanie DNS, beaconing C2:** klasyfikatory behawioralne,
- **uwierzytelnianie adaptacyjne i Zero Trust:** ocena ryzyka logowania, wykrywanie przejętych kont,
- **DDoS i botnety:** wykrywanie na przepływach (NetFlow),
- **zarządzanie podatnościami:** priorytetyzacja (np. EPSS), symulacje ścieżek ataku, cyfrowe bliźniaki,
- **threat hunting i threat intelligence**, bezpieczeństwo IoT i brzegu sieci (lekkie modele na urządzeniach).

**Ograniczenia i ryzyka:**

- **fałszywe alarmy** (anomalia ≠ atak) i **dryf danych** (model się starzeje),
- brak **wyjaśnialności** (czarna skrzynka),
- **ataki na same modele:** zatruwanie danych treningowych, ataki adwersarialne (omijanie detekcji), kradzież modelu,
- **AI po stronie atakujących:** deepfake'i w socjotechnice, malware adaptacyjne, automatyczny phishing,
- koszty, kompetencje, jakość danych,
- **prywatność i prawo:** RODO (profilowanie, monitoring), unijny AI Act,
- zbytnia automatyzacja może blokować legalny ruch.

**Dobre praktyki:**

- podejście **hybrydowe** (sygnatury, reguły, ML),
- dostrajanie na własnym ruchu i monitorowanie dryfu,
- **człowiek w pętli** przy decyzjach krytycznych, stopniowe zwiększanie autonomii SOAR,
- ochrona danych treningowych i testy odporności modeli,
- mierzenie skuteczności (FPR, MTTD, MTTR).

**Wniosek:** AI/ML to **uzupełnienie**, a nie zamiennik tradycyjnych zabezpieczeń. Zwiększa skuteczność wykrywania i szybkość reakcji, ale wymaga dobrych danych, nadzoru człowieka i ochrony przed atakami na same modele.

## Podsumowanie

- **AI/ML** uzupełniają tradycyjne środki: **wykrywanie anomalii i zero-day** (nienadzorowane: autoenkodery, isolation forest; NBA/NDR), **redukcja fałszywych alarmów i alert fatigue**, **automatyczna reakcja (SOAR)**, UEBA i adaptacyjne uwierzytelnianie, wykrywanie malware/phishingu/DGA/tunelowania, priorytetyzacja podatności (EPSS), threat hunting i predykcja, ochrona IoT (edge ML), asystenci LLM w SOC.
- **Ograniczenia:** fałszywe alarmy i dryf danych, brak wyjaśnialności, **zatruwanie i ataki adwersarialne**, AI po stronie atakujących (deepfake'i, malware adaptacyjne), prywatność i regulacje (AI Act), potrzeba nadzoru człowieka.
- Zasada: **hybrydowe podejście, człowiek w pętli, dobre dane, ciągłe dostrajanie i testy odporności**.

---
[⬅️ Poprzedni temat](10_Etapy_i_metody_testowania_bezpieczeństwa_sieci_teleinformatycznych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../BezpieczeństwoSystemówOperacyjnychIUsług/BezpieczeństwoSystemówOperacyjnychIUsług_tytul.md)