# Monitorowanie ruchu sieciowego w wykrywaniu nieprawidłowości i incydentów

> Opracowanie oparte na wykładach: *Zapory i IDS* (slajdy 10–19, 27–29, 33), *Mechanizmy bezpieczeństwa* (slajdy 49–50), W1 (obrona w głąb); uzupełnienia oznaczone ***(uzupełnienie)***.

## Po co monitorować

Nawet najlepsze mechanizmy prewencji nie zatrzymają wszystkich ataków. **Monitoring** pozwala **wykryć** naruszenia (a często ich wczesne etapy: rozpoznanie, ruch boczny), skrócić czas reakcji, wesprzeć **analizę powłamaniową** i spełnić wymagania zgodności (RODO, PCI DSS – wykład, slajd 13). Wykład: *kluczem jest szybkie wykrycie, odpowiednia reakcja i wyciągnięcie wniosków* (MBK1, slajd 50).

## Źródła danych

| Źródło | Co daje | Zalety | Wady |
| :--- | :--- | :--- | :--- |
| **Przechwytywanie pakietów** (SPAN/mirror, **TAP**) | pełna zawartość ruchu | najbogatsza analiza (Wireshark, IDS) | ogromny wolumen, szyfrowanie |
| **Przepływy** (NetFlow, sFlow, IPFIX) | metadane: kto z kim, ile, kiedy | skalowalne, działa też dla ruchu szyfrowanego | brak treści |
| **Logi urządzeń** (zapory, routery, przełączniki, proxy, DNS, DHCP, VPN, WAF) | zdarzenia, decyzje zapory | tanie, bogate w kontekst | wymagają agregacji i synchronizacji czasu (NTP) |
| **Logi systemów** (Windows Event Log, `auth.log`, auditd) | logowania, zmiany uprawnień | wgląd w hosta | rozproszone |
| **IDS/IPS i NSM (Zeek)** | alerty i metadane protokołów (`conn.log`, `dns.log`) | detekcja gotowa | szum |
| **SNMP / telemetria** | obciążenie, błędy interfejsów | stan urządzeń | ograniczona |
| **EDR / honeypoty / threat intelligence** | zdarzenia hostowe, IoC | wczesne wykrycie | |

Dobre praktyki gromadzenia logów (wykład, MBK1): logi zawierają *kto, co, kiedy, skąd, wynik*; chronione przed modyfikacją, **archiwizowane**, **agregowane w SIEM**.

## Podstawa: profil normalnego ruchu (baseline)

Aby rozpoznać nieprawidłowość, trzeba znać **normę**: typowe protokoły, wolumeny, godziny, pary host–host, kierunki. Odchylenia od baseline są podstawą **wykrywania anomalii** (wykład, slajd 12).

## Typowe nieprawidłowości i co o nich świadczy

| Obserwacja | Możliwa przyczyna / atak | Gdzie widać |
| :--- | :--- | :--- |
| **Skanowanie portów** (wiele SYN do różnych portów/hostów) | rozpoznanie (nmap, masscan) | IDS, zapora, NetFlow |
| **Wiele nieudanych logowań** z jednego IP (4625, `auth.log`) | **brute force**, password spraying | logi systemów, IPS, SIEM |
| **Skok wolumenu / pakietów/s** | **DDoS**, amplifikacja | NetFlow, IDS |
| **Regularne połączenia do tej samej domeny/IP** (*beaconing*) | komunikacja **C2** malware | Zeek, DNS, proxy |
| **Duże zapytania/odpowiedzi DNS, dziwne domeny** | **tunelowanie DNS**, DGA | logi DNS |
| **Duży ruch wychodzący w nietypowych godzinach** | **eksfiltracja danych** | NetFlow, proxy, DLP |
| **Ruch wewnętrzny między hostami, które nie komunikowały się wcześniej** (SMB, RDP) | **ruch boczny** (lateral movement) | NetFlow, zapory wewnętrzne, EDR |
| **Zduplikowane/zmienione MAC–IP, wiele ARP** | ARP spoofing | przełączniki, arpwatch |
| **Nowy serwer DHCP/DNS** | rogue DHCP/DNS | Wireshark, DHCP snooping |
| **Nietypowe porty/protokoły, SSH na 443** | omijanie zapór, tunelowanie | IDS, Zeek |
| **Połączenia do znanych złośliwych adresów** (IoC) | zainfekowany host | zapora, threat intelligence |
| **Nowe/nieznane urządzenia w sieci** | rogue device, shadow IT | NAC, skanowanie |

(Wykład, slajd 17: IDS/IPS wykrywają m.in. brute force, skanowanie portów, exploity znanych luk, DDoS, SQL injection, komunikację C2.)

## Proces wykrywania i reagowania

```
 zbieranie danych → normalizacja → korelacja (SIEM) → alert → triage (SOC)
      → analiza → izolacja/blokada → usunięcie przyczyny → odtworzenie → wnioski
```

### SIEM, SOC, SOAR (wykład, slajdy 16, 28)

- **SIEM** – centralizacja logów i **korelacja zdarzeń** z wielu źródeł (np. logowania z IdP + ruch + zapora); alerty wg reguł.
- **SOC** – **monitoring 24/7**, priorytetyzacja incydentów (triage), weryfikacja fałszywych alarmów, wzbogacanie o threat intelligence, **reagowanie wg playbooków** (izolacja hosta, usunięcie malware), koordynacja z IT/prawnikami/PR, eskalacja do CISO, **ciągłe doskonalenie** (wnioski, strojenie reguł, threat hunting).
- **SOAR** – automatyzacja reakcji i redukcja *alert fatigue* (wykład, slajd 29).

### Obowiązki organizacji (MBK1, slajd 50)

1. **Rejestr prób naruszenia** – kto, kiedy, skąd, co próbował zrobić,
2. **automatyczne powiadamianie** administratorów,
3. **procedury reakcji** dla typów incydentów,
4. **post-mortem** – analiza przyczyn i działań naprawczych.

Przykład z wykładu: wielokrotne nieudane logowania z tego samego IP → system **automatycznie blokuje IP**, loguje zdarzenie, powiadamia administratora; po incydencie – analiza, czy atak celowany, aktualizacja polityk.

*(uzupełnienie)* Fazy reagowania wg **NIST SP 800-61**: przygotowanie → wykrywanie i analiza → ograniczanie, usuwanie, odtwarzanie → działania po incydencie.

## Wyzwania monitoringu

| Wyzwanie | Skutek | Środki zaradcze (wykład, slajdy 27, 29) |
| :--- | :--- | :--- |
| **Szyfrowanie** (>90% ruchu HTTPS, TLS 1.3) | brak wglądu w treść; C2 i eksfiltracja ukryte w TLS | inspekcja TLS (proxy MITM, certyfikat CA – uwaga na RODO/prywatność), analiza metadanych (**JA3**), EDR |
| **Skalowalność** (>10 Gb/s) | utrata pakietów, koszty | load balancing przed sensorami, FPGA/ASIC, flow zamiast pełnych pakietów |
| **Fałszywe alarmy / alert fatigue** | wypalenie analityków, przeoczenie realnego ataku | strojenie, whitelisting, SOAR, AI |
| **Wydajność vs bezpieczeństwo** (DPI, IPS inline = SPOF) | opóźnienia, awaria ruchu | bypass, selektywna inspekcja |
| **Zero-day i polimorfizm** | brak sygnatur | anomalie, sandboxing, behawioralna analiza |
| **Rozproszenie logów, brak synchronizacji czasu** | trudna korelacja | NTP, centralny SIEM |
| **Prywatność i prawo** | ryzyko naruszenia RODO | polityka monitorowania, minimalizacja danych |

## Metryki skuteczności *(uzupełnienie)*

**MTTD** (Mean Time To Detect), **MTTR** (to Respond/Recover), liczba fałszywych alarmów, pokrycie sieci czujnikami, odsetek zdarzeń zbadanych w SLA.

## Podsumowanie

- Monitoring ruchu opiera się na **pakietach (SPAN/TAP), przepływach (NetFlow/sFlow), logach i IDS/IPS**, porównywanych z **profilem normalnego ruchu**.
- Wskaźniki: skanowanie, brute force, skoki wolumenu, beaconing C2, tunelowanie DNS, eksfiltracja, ruch boczny, spoofing L2.
- Wykrycia trafiają do **SIEM/SOC/SOAR**; reakcja wg procedur (rejestr, powiadomienie, izolacja, post-mortem).
- Wyzwania: szyfrowanie, skala, fałszywe alarmy, SPOF, prawo.
