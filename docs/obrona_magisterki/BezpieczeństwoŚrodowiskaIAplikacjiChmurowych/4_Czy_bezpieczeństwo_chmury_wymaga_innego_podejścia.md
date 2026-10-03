# Czy i ewentualnie dlaczego (tak lub nie) do bezpieczeństwa chmury musimy podchodzić inaczej niż w przypadku standardowych metod bezpieczeństwa IT?

**Tak.** Podstawowe cele bezpieczeństwa (poufność, integralność, dostępność) i wiele zasad pozostają te same, ale chmura zmienia warunki, więc samo przeniesienie tradycyjnych metod nie wystarcza.

**Dlaczego trzeba podejść inaczej:**

- **Model współdzielonej odpowiedzialności.** Dostawca odpowiada za bezpieczeństwo samej chmury (infrastruktura, sprzęt, wirtualizacja), a klient za to, co w niej umieszcza (dane, konta, konfigurację, aplikacje). Zakres zależy od modelu: IaaS, PaaS lub SaaS. W tradycyjnym IT organizacja odpowiada za wszystko.
- **Brak kontroli nad infrastrukturą fizyczną.** Nie mamy wpływu na sprzęt, sieć i lokalizację danych. Trzeba polegać na umowach, certyfikatach i audytach dostawcy.
- **Współdzielenie zasobów (multi-tenancy).** Wielu klientów korzysta z tej samej infrastruktury, więc potrzebna jest silna izolacja, szyfrowanie i segmentacja.
- **Dynamiczność i skala.** Zasoby powstają i znikają automatycznie, więc ręczne zabezpieczanie nie nadąża. Potrzebna jest automatyzacja, **infrastruktura jako kod** i ciągły monitoring.
- **Rozmyty perymetr.** Nie ma jednej granicy sieci chroniącej wnętrze, bo dostęp jest przez Internet i API. Dlatego tożsamość staje się nowym perymetrem: **IAM, MFA i podejście Zero Trust** (nie ufamy nikomu domyślnie).
- **Duże znaczenie konfiguracji.** Większość incydentów wynika z błędów konfiguracji (np. publiczny magazyn danych), a nie z ataków na samą chmurę.
- **Nowa powierzchnia ataku.** API, interfejsy zarządzania, kontenery, funkcje serverless.
- **Zgodność i prawo.** Lokalizacja danych, RODO, uzależnienie od dostawcy (vendor lock-in).

**Wniosek:** tradycyjne mechanizmy (szyfrowanie, kontrola dostępu, kopie zapasowe, monitoring) nadal obowiązują, ale trzeba je dostosować do modelu współdzielonej odpowiedzialności, automatyzacji i bezpieczeństwa opartego na tożsamości.

## Podsumowanie

- **Tak** – chmura wymaga innego podejścia: współdzielona odpowiedzialność, współdzielona infrastruktura, dynamiczne zasoby, rozmyty perymetr, zarządzanie przez API, ograniczona widoczność, nowe zagrożenia.
- Zasady podstawowe (CIA, najmniejsze uprawnienia, obrona w głąb) pozostają, lecz **ich realizacja jest inna** (IAM, automatyzacja, Zero Trust, szyfrowanie, DevSecOps).
- Bezpieczeństwo trzeba projektować **od początku** i obejmować **wszystkie warstwy** (holistycznie).

---
[⬅️ Poprzedni temat](3_Modele_chmur_komputerowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Model_chmury_w_którym_dostawca_odpowiada_za_infrastrukturę_i_platformę.md)