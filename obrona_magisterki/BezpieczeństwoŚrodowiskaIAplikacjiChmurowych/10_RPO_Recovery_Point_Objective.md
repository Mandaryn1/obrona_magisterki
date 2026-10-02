# RPO (Recovery Point Objective)

## Definicja

**RPO (Recovery Point Objective – docelowy punkt odtworzenia)** to **maksymalny akceptowalny czas utraty danych** liczony wstecz od momentu awarii, czyli **jak dużo danych (mierzone w czasie) organizacja może stracić** w wyniku incydentu.

- RPO = **5 minut** → po awarii można stracić co najwyżej dane z ostatnich 5 minut.
- RPO = **24 godziny** → dopuszczalna utrata danych z ostatniej doby (wystarcza codzienny backup).
- RPO = **0** → brak dopuszczalnej utraty danych (wymaga replikacji synchronicznej).

Wykład (slajd 26): **istotne jest zrozumienie RPO i RTO dla różnych systemów** w kontekście backupu i odzyskiwania awaryjnego (*disaster recovery*) zapewniających ciągłość biznesową.

## Wykres

```
 czas ──────────────────────────────────────────────────────────▶
        ▲ ostatnia kopia      ▲ awaria              ▲ system znów działa
        │◀───── RPO ─────────▶│◀──────── RTO ───────▶│
        (maks. utrata danych)     (maks. przestój)
```

RPO patrzy **wstecz** od awarii (utrata danych), RTO **w przód** (przestój) – zob. temat 11.

## Od czego zależy RPO

Wynika z **analizy wpływu na biznes (BIA – Business Impact Analysis)**: im większa wartość i zmienność danych, tym niższe RPO.

| System | Typowe RPO | Uzasadnienie |
| :--- | :--- | :--- |
| transakcje bankowe, płatności | ≈ 0 | każda utracona transakcja to strata i ryzyko prawne |
| sklep internetowy (zamówienia) | minuty | utrata zamówień = utrata przychodu |
| system CRM, e-mail | godziny | częściowa odtwarzalność |
| środowisko testowe, archiwum | doba–tydzień | niski koszt utraty |

## Jak osiąga się dane RPO

RPO bezpośrednio określa **częstotliwość tworzenia kopii/replikacji**:

| Mechanizm | Osiągalne RPO | Uwagi |
| :--- | :--- | :--- |
| **backup dzienny** (np. `pg_dump` z CronJob o 2:00 – przykład z wykładu, slajd 315) | **do 24 godzin** | tanie; utrata danych od ostatniego backupu |
| **backup godzinowy / snapshoty** | do 1 h | snapshoty dysków, AWS Backup |
| **kopie przyrostowe + archiwizacja logów transakcji (WAL)** – *point-in-time recovery (PITR)* | sekundy–minuty | odtworzenie do dowolnej chwili |
| **replikacja asynchroniczna** (między strefami/regionami) | sekundy | małe opóźnienie replikacji = potencjalna utrata |
| **replikacja synchroniczna** (multi-AZ) | **≈ 0** | każda transakcja potwierdzona w kilku miejscach; wyższy koszt i opóźnienia |

Im niższe RPO, tym **wyższy koszt** (infrastruktura, transfer, złożoność).

## Przykład obliczeniowy

Baza danych zamówień: backup pełny raz dziennie o 2:00 (CronJob w Kubernetes). Awaria o 17:30.

- Ostatni backup z 2:00 → utrata danych z **15,5 godziny**,
- RPO tego rozwiązania = **24 h** (gorszy przypadek: awaria tuż przed kolejnym backupem),
- aby zapewnić **RPO = 15 min**, trzeba np. archiwizować WAL co minutę (PITR) lub użyć replikacji, a backup pełny tylko uzupełnia.

## RPO w kopiach zapasowych i DR w chmurze (wykład)

- backup w chmurze może być **automatyczny i geograficznie rozproszony**, dane backupu **szyfrowane** i chronione przed dostępem,
- procedury odzyskiwania **regularnie testowane**, udokumentowane i przeglądane,
- narzędzia: `pg_dump`, **Velero** (backup klastrów Kubernetes), **AWS Backup** (snapshoty RDS), **Barman** (PostgreSQL), przechowywanie w S3; **backup powinien być zaszyfrowany**, pamiętać o kosztach i czasie przechowywania,
- *(uzupełnienie)* zasada **3-2-1** (3 kopie, 2 nośniki, 1 poza lokalizacją), **kopie niemodyfikowalne (immutable)** jako ochrona przed ransomware, testy odtwarzania.

## Strategie DR a RPO/RTO *(uzupełnienie)*

| Strategia | RPO | RTO | Koszt |
| :--- | :--- | :--- | :--- |
| **Backup & restore** | godziny | godziny–doby | **niski** |
| **Pilot light** (minimalna kopia środowiska) | minuty | dziesiątki minut–godziny | niski–średni |
| **Warm standby** (pomniejszone środowisko działa) | sekundy–minuty | minuty | średni–wysoki |
| **Multi-site active-active** | ≈ 0 | ≈ 0 | **wysoki** |

## Dobre praktyki

- ustalić RPO per system na podstawie BIA, uzgodnić z biznesem, zapisać w SLA,
- dobrać częstotliwość i metodę kopii/replikacji do RPO,
- **monitorować** opóźnienie replikacji i powodzenie backupów (alerty),
- **testować odtwarzanie** (czy dane faktycznie są odtwarzalne – kopia, której nie sprawdzono, to nie kopia),
- szyfrować i chronić kopie (także przed ransomware).

## Podsumowanie

- **RPO** = maksymalny dopuszczalny **czas utraty danych** (jak „stare" mogą być odtworzone dane).
- Określa **częstotliwość kopii/replikacji**; niższe RPO = wyższy koszt.
- Wartość ustala **BIA**; komplementarne do **RTO** (czas przywrócenia działania).

---
[⬅️ Poprzedni temat](9_Szyfrowanie_danych_w_spoczynku_i_w_tranzycie.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](11_RTO_Recovery_Time_Objective.md)