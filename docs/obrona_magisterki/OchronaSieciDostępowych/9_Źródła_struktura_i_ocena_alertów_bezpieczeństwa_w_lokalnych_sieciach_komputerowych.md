# Źródła, struktura i ocena alertów bezpieczeństwa w lokalnych sieciach komputerowych

**Alert bezpieczeństwa** to powiadomienie o zdarzeniu, które może wskazywać na atak lub naruszenie polityki. Jego sens polega na tym, żeby analityk szybko ocenił, czy to realne zagrożenie, i zareagował.

**Źródła alertów:**

- **wewnętrzne:** IDS/IPS, zapory i NGFW, **SIEM** (korelacja zdarzeń), EDR/antywirus, **NAC/ISE** (nowe lub niezgodne urządzenia), przełączniki i WIDS (naruszenia port security, DAI, rogue AP), **NetFlow/NBA** (anomalie ruchu), skanery podatności, honeypoty, logi systemów i aplikacji,
- **zewnętrzne (threat intelligence):** serwisy i organizacje wymieniające informacje o zagrożeniach, m.in. **Cisco Talos**, SANS/Internet Storm Center, MITRE (CVE, ATT&CK), **FIRST**, CIS/MS-ISAC, Mandiant, DHS AIS.
- standardy wymiany informacji: **STIX/TAXII**, MISP, platformy TIP.

**Struktura alertu** (typowe pola):

- czas zdarzenia i źródło (sensor, system),
- **sygnatura/reguła** i opis zdarzenia, priorytet lub dotkliwość,
- **adres IP i port źródłowy i docelowy**,
- użytkownik i host,
- **payload** lub dowód (fragment ruchu, log),
- podjęta akcja (alert, zablokowano),
- odniesienia: **CVE**, technika **MITRE ATT&CK**.

**SIEM** wzbogaca alert o kontekst: użytkownika, urządzenie, jego krytyczność i postawę bezpieczeństwa.

**Ocena alertów** polega na ustaleniu, czy alert jest prawdziwy:

- **True Positive (TP):** prawdziwy alarm, rzeczywisty atak,
- **False Positive (FP):** fałszywy alarm, zdarzenie niegroźne,
- **False Negative (FN):** atak niewykryty (najgroźniejszy),
- **True Negative (TN):** brak ataku i brak alertu.

**Pytania pomocnicze przy ocenie:**

- kto jest źródłem i celem? czy zasób jest ważny?
- czy działanie było uprawnione lub zgodne z polityką?
- czy są inne alerty dotyczące tych zasobów?
- jakie jest urządzenie i jaka jego postawa?

**Ocena dotkliwości podatności:** system **CVSS 3.1** (skala 0–10, grupy Base/Temporal/Environmental, metryki m.in. AV, AC, PR, UI, S, C, I, A). Przedziały: poniżej 4 niski, poniżej 7 średni, do 8,9 wysoki, od 9 **krytyczny**. Baza **NVD/CVE**.

**Wyzwanie:** **zmęczenie alertami (alert fatigue)**, czyli tysiące alertów, z których wiele jest fałszywych. Ogranicza się je przez dostrajanie reguł, priorytetyzację, korelację w SIEM i automatyzację w **SOAR**.

### Priorytetyzacja alertów i podatności – wspólne zasady

Alert/podatność **pilna**, gdy: wysoki CVSS **i** krytyczny zasób **i** aktywny exploit (threat intelligence) **i** ekspozycja zewnętrzna; **niskie priorytety** – zadania planowe. Wykład ryzyko: ryzyko wysokie – natychmiastowe działania; średnie – monitorować i redukować wg planu; niskie – akceptować z nadzorem; alokacja zasobów uwzględnia krytyczność aktywów, koszt zabezpieczeń i prawdopodobieństwo.

## Podsumowanie

- **Źródła alertów:** IDS/IPS, zapory, SIEM (korelacja), EDR, NAC/ISE, urządzenia sieciowe i logi, skanery, honeypoty oraz **zewnętrzne threat intelligence** (SANS, MITRE, FIRST, CIS, Talos, FireEye, AIS) wymieniane przez **STIX/TAXII/MISP/TIP**.
- **Struktura alertu:** czas, sensor, sygnatura/reguła, kategoria i priorytet, źródło/cel (IP, porty), użytkownik/host, payload, akcja, odniesienia (CVE, ATT&CK); **SIEM wzbogaca** o kontekst użytkownika, urządzenia i postury.
- **Ocena:** weryfikacja TP/FP/FN, pytania kontekstowe (kto, jak ważny, uprawniony?), krytyczność aktywa, korelacja, **CVSS (0–10: Niski <4, Średni, Wysoki, Krytyczny ≥9; metryki Base/Temporal/Environmental)** + CVE/NVD; priorytetyzacja i walka z alert fatigue.

---
[⬅️ Poprzedni temat](8_Audyt_bezpieczeństwa_testy_penetracyjne_i_ocena_podatności.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Systemy_zarządzania_bezpieczeństwem_informacji_w_ochronie_sieci_lokalnych.md)