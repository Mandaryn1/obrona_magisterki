# Czym jest RTO (Recovery Time Objective)?

**RTO (Recovery Time Objective)** to **maksymalny dopuszczalny czas niedostępności** usługi lub systemu po awarii, czyli ile czasu może minąć od awarii do pełnego przywrócenia działania. Wyrażamy go w jednostkach czasu.

**Przykład:** jeśli RTO wynosi 4 godziny, a awaria nastąpiła o 10:00, to system musi działać ponownie najpóźniej o 14:00.

**Od czego zależy RTO:** od **szybkości odtwarzania**. Obejmuje wykrycie awarii, decyzję, przywrócenie danych i uruchomienie usług. Skracają go:

- **automatyzacja** (automatyczne przełączanie, infrastruktura jako kod),
- **redundancja** (zapasowe zasoby w innej strefie lub regionie, tryb active-active lub warm/hot standby),
- **wcześniej przygotowane i przetestowane procedury** odtwarzania,
- szybkie łącza i gotowe obrazy systemów.

Im krótsze RTO, tym droższe rozwiązanie.

**Znaczenie w chmurze:** RTO wraz z RPO wyznacza strategię **disaster recovery**. Wybiera się ją według krytyczności usługi, od taniej, ale wolnej (backup and restore), przez pilot light i warm standby, po drogą, ale prawie natychmiastową (multi-site active-active).

**Różnica względem RPO:** RTO dotyczy **czasu niedostępności** (jak długo czekamy na przywrócenie), a RPO **utraty danych** (ile danych możemy stracić).

## Podsumowanie

- **RTO** = maksymalny dopuszczalny **czas niedostępności** po awarii (od awarii do przywrócenia działania).
- **RPO** – ile danych można stracić; **RTO** – jak długo można nie działać.
- Zależy od **BIA/SLA**; osiąga się je przez redundancję, automatyzację, plany i **testy DR**; krótsze RTO = większy koszt.

---
[⬅️ Poprzedni temat](10_RPO_Recovery_Point_Objective.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](12_VPC_Virtual_Private_Cloud_w_bezpieczeństwie_chmury.md)