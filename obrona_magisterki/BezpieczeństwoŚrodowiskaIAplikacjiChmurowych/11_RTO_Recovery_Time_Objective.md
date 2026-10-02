# RTO (Recovery Time Objective)

## Definicja

**RTO (Recovery Time Objective – docelowy czas odtworzenia)** to **maksymalny akceptowalny czas, w którym usługa/system może być niedostępny** po awarii lub incydencie – czas od wystąpienia awarii do **przywrócenia działania** na uzgodnionym poziomie.

- RTO = **4 godziny** → system musi być uruchomiony ponownie w ciągu 4 h od awarii.
- RTO = **15 minut** → wymaga automatycznego przełączenia (failover).
- RTO ≈ **0** → ciągła dostępność (wiele aktywnych lokalizacji).

Wykład (slajd 26): **istotne jest zrozumienie RPO i RTO dla różnych systemów** w ramach backupu i *disaster recovery* (ciągłość biznesowa).

## RTO a RPO

| | **RPO** | **RTO** |
| :--- | :--- | :--- |
| Dotyczy | **utraty danych** | **przestoju (czasu niedostępności)** |
| Mierzone | wstecz od awarii | w przód od awarii |
| Pytanie | *ile danych możemy stracić?* | *jak długo możemy nie działać?* |
| Wpływa na | **częstotliwość backupów/replikacji** | **architekturę odtwarzania, automatyzację, redundancję** |
| Jednostka | czas (minuty, godziny) | czas (minuty, godziny) |

```
 ostatnia kopia          awaria                       usługa przywrócona
      │◀────── RPO ──────▶│◀────────── RTO ───────────▶│
      │   utrata danych   │      przestój usługi       │
```

## Co składa się na czas odtworzenia

RTO obejmuje cały proces, nie tylko „kliknięcie przywróć":

1. **wykrycie awarii** (monitoring, alerty),
2. **decyzja** i eskalacja (kto ogłasza DR),
3. **przywrócenie** infrastruktury i danych (uruchomienie środowiska, odtworzenie backupu, przełączenie ruchu/DNS),
4. **weryfikacja** i testy,
5. **uruchomienie dla użytkowników**.

*(uzupełnienie)* Powiązane: **MTD/MAO** (Maximum Tolerable Downtime) – maksymalny czas, jaki biznes może wytrzymać; **WRT** (Work Recovery Time) – czas przywrócenia pracy po technicznym odtworzeniu; zależność: **RTO + WRT ≤ MTD**.

## Od czego zależy RTO

Wynika z **BIA** (Business Impact Analysis): koszt godziny przestoju, straty wizerunkowe, wymagania prawne i umowne (**SLA**).

| System | Przykładowe RTO |
| :--- | :--- |
| płatności, bankowość, sterowanie krytyczne | sekundy–minuty |
| sklep internetowy, API produktowe | minuty–1 godzina |
| wewnętrzny system księgowy | 4–8 godzin |
| archiwum, środowisko testowe | doby |

## Jak skracać RTO

| Strategia (DR w chmurze) | RTO | Opis |
| :--- | :--- | :--- |
| **Backup & restore** | godziny–doby | odtworzenie z kopii do nowego środowiska (najtańsze) |
| **Pilot light** | dziesiątki minut–godziny | minimalna, uśpiona kopia kluczowych komponentów |
| **Warm standby** | minuty | pomniejszone, działające środowisko; skalowane po awarii |
| **Active–active (multi-site/multi-region)** | ≈ 0 | pełna redundancja, ruch rozdzielany; awaria jednej lokalizacji niezauważalna |

Techniki obniżania RTO:

- **automatyzacja odtwarzania** – Infrastructure as Code (Terraform), obrazy kontenerów, **orkiestracja** (Kubernetes: `replicas: 3`, probes, automatyczny restart), *runbooki* (np. DRR – *disaster recovery runbook* z wykładu, slajd 242),
- **redundancja i wysoka dostępność** – wiele stref dostępności (multi-AZ), **load balancing**, automatyczny **failover**,
- szybkie przywracanie (snapshoty, repliki gotowe do awansu),
- **monitoring i alerty** skracające czas wykrycia,
- **regularne testy DR** (ćwiczenia, *chaos engineering* – slajdy 242, 237),
- gotowa dokumentacja i przeszkolony zespół.

Niższe RTO = **wyższy koszt** (redundancja, automatyzacja).

## Przykład

Serwis katalogu produktów (Spring Boot + PostgreSQL) w Kubernetes. Wymagania: **RPO = 5 min, RTO = 30 min**.

- RPO 5 min → archiwizacja WAL/replikacja co kilka minut,
- RTO 30 min → gotowy manifest/IaC, automatyczne przywrócenie klastra (Velero), warm standby w drugim regionie, automatyczny failover DNS, runbook testowany co kwartał.

## Zależności z bezpieczeństwem

RTO/RPO to część **dostępności** (A w CIA) i reagowania na incydenty: atak **ransomware**, **DDoS** lub błędna konfiguracja mogą wymusić DR. Kopie zapasowe powinny być **szyfrowane i niezmienne**, a plan DR **testowany**.

## Podsumowanie

- **RTO** = maksymalny dopuszczalny **czas niedostępności** po awarii (od awarii do przywrócenia działania).
- **RPO** – ile danych można stracić; **RTO** – jak długo można nie działać.
- Zależy od **BIA/SLA**; osiąga się je przez redundancję, automatyzację, plany i **testy DR**; krótsze RTO = większy koszt.

---
[⬅️ Poprzedni temat](10_RPO_Recovery_Point_Objective.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](12_VPC_Virtual_Private_Cloud_w_bezpieczeństwie_chmury.md)