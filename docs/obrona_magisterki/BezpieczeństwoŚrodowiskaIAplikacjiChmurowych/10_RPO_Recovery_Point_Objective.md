# Czym jest RPO (Recovery Point Objective)?

**RPO (Recovery Point Objective)** to **maksymalny dopuszczalny czas utraty danych** liczony wstecz od momentu awarii. Mówi, jak stary może być ostatni punkt, do którego odtworzymy dane, i ile danych organizacja jest w stanie stracić bez poważnych skutków. Wyrażamy go w jednostkach czasu.

**Przykład:** jeśli RPO wynosi 1 godzinę, a awaria nastąpiła o 15:00, to dane muszą dać się odtworzyć co najmniej do 14:00. Można więc stracić najwyżej godzinę pracy.

**Od czego zależy RPO:** od **częstotliwości tworzenia kopii zapasowych lub replikacji**. Kopie raz na dobę oznaczają RPO do 24 godzin, replikacja ciągła oznacza RPO bliskie zera. Im mniejsze RPO, tym droższe rozwiązanie (więcej łączy, miejsca i zasobów).

**Znaczenie w chmurze:** RPO jest jednym z kluczowych parametrów planowania odtwarzania po awarii (**disaster recovery**) i ustala się je zależnie od krytyczności danych, np. dla transakcji bankowych bliskie 0, dla archiwum godziny lub dni. Realizują go m.in. **automatyczne kopie zapasowe, snapshoty i replikacja** (między strefami i regionami).

**Nie należy mylić z RTO:** RPO dotyczy **ile danych możemy stracić**, a RTO **jak długo system może być niedostępny**.

## Podsumowanie

- **RPO** = maksymalny dopuszczalny **czas utraty danych** (jak „stare" mogą być odtworzone dane).
- Określa **częstotliwość kopii/replikacji**; niższe RPO = wyższy koszt.
- Wartość ustala **BIA**; komplementarne do **RTO** (czas przywrócenia działania).

---
[⬅️ Poprzedni temat](9_Szyfrowanie_danych_w_spoczynku_i_w_tranzycie.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](11_RTO_Recovery_Time_Objective.md)