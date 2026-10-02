# Znaczenie monitorowania sieci lokalnej w wykrywaniu i analizie incydentów bezpieczeństwa

> Opracowanie oparte na kursie Cisco *Cyber Threat Management* (Moduł 2: SIEM, SOAR; Moduł 4: profilowanie sieci i serwerów, wykrywanie anomalii) oraz wykładzie o ryzyku; uzupełnienia – ***(uzupełnienie)***.

## Po co monitorować sieć lokalną

Zabezpieczenia prewencyjne (zapory, 802.1X, segmentacja) **nie zatrzymają wszystkich ataków**. Monitorowanie zapewnia **widoczność** (kto, co, kiedy, skąd), umożliwia:

1. **wczesne wykrycie** naruszeń i ich oznak (skanowanie, ruch boczny, eksfiltracja) – skrócenie czasu przebywania napastnika w sieci,
2. **analizę i rekonstrukcję incydentu** (analiza kryminalistyczna – *forensics*, określenie zakresu, źródła i skutków),
3. **szybką reakcję** (izolacja hosta, blokada, powiadomienie),
4. **wykrycie nieautoryzowanych urządzeń i zmian** (rogue devices, shadow IT),
5. **zgodność i audyt** (dowody, raporty dla RODO, ISO 27001, NIS2),
6. **doskonalenie zabezpieczeń** (wnioski, strojenie reguł),
7. **diagnostykę i planowanie pojemności** (efekt uboczny).

Zasada z kursu: *aby wykryć poważne incydenty, trzeba zrozumieć, scharakteryzować i przeanalizować informacje o normalnym funkcjonowaniu sieci.*

## Profilowanie sieci i serwerów – baza wykrywania (kurs Cisco, Moduł 4)

**Profilowanie sieci** tworzy **statystyczną linię bazową (baseline)**; **niewyjaśnione odchylenia** od niej mogą wskazywać na naruszenie.

Zasady:

- w baseline uwzględnia się **wszystkie normalne operacje** sieci i aktualizuje ją (**okno przesuwne** usuwa nieaktualne dane); analiza przez **dłuższy czas** (zmienność dobowa/sezonowa),
- np. zwiększone użycie łączy WAN w nietypowych godzinach → możliwa **eksfiltracja danych**.

### Elementy profilu sieciowego

| Element | Opis |
| :--- | :--- |
| **czas trwania sesji** | od nawiązania przepływu do jego zakończenia |
| **całkowita przepustowość** | ilość danych między źródłem a celem w okresie |
| **używane porty** | procesy TCP/UDP dostępne do odbioru danych (malware używa nietypowych portów) |
| **przestrzeń adresowa zasobów krytycznych** | adresy IP/lokalizacje ważnych systemów i danych |
| **ruch typów/kierunków** | normalne wzorce wejścia/wyjścia |
| **ruch host–host** | wzrost ruchu między klientami (zamiast klient–serwer) → **rozprzestrzenianie się malware bocznie** |
| **zachowanie użytkowników** | z AAA, logów serwerów, systemu profilowania (**Cisco ISE**) |

### Profil serwera

**Podstawa bezpieczeństwa dla danego serwera**: akceptowane parametry sieci, użytkowników i aplikacji. Elementy: **porty nasłuchujące** (demony TCP/UDP), **zalogowani użytkownicy i konta**, **konta usługowe**, **środowisko oprogramowania** (zadania, procesy, aplikacje).

## Wykrywanie anomalii (kurs Cisco, Moduł 4)

- **Analiza zachowania sieci (NBA – Network Behavior Analysis):** analiza dużych ilości różnorodnych danych (cechy przepływu, pakietów, telemetria) technikami **statystycznymi i uczeniem maszynowym**; porównanie normalnej wydajności z bieżącą; znaczące odchylenia = potencjalne **IOC**.
- Przykład: wykrycie **robaka** po zachowaniu skanującym zainfekowanych hostów.
- **Wykrywanie oparte na regułach** – analiza zdekodowanych pakietów względem wcześniej zdefiniowanych wzorców (sygnatury).
- Przykład progu: co X minut próbkowanie 1/Y przepływów przez Z sekund; **jeśli liczba przepływów > N → alarm** (potencjalna utrata danych).

## Co wykrywa monitoring w LAN – typowe symptomy

| Symptom | Możliwy incydent |
| :--- | :--- |
| skanowanie portów/hostów (wiele SYN, ICMP sweep) | rozpoznanie, robak |
| wiele nieudanych logowań | **brute force**, password spraying |
| skok ruchu / pakietów na sekundę | **DoS/DDoS**, burza rozgłoszeniowa |
| dziwne ARP, zmiana MAC bramy, nowy serwer DHCP | **ARP spoofing, rogue DHCP** |
| nowe nieznane urządzenie w sieci | **rogue device**, shadow IT |
| duży ruch wychodzący w nietypowych porach | **eksfiltracja** |
| komunikacja z podejrzanymi domenami/IP (IOC) | C2, malware |
| ruch między stacjami (SMB/RDP) | **ruch boczny** |
| zmiany konfiguracji urządzeń, plików | naruszenie integralności |

## Źródła danych do monitoringu

przechwytywanie pakietów (**SPAN/TAP**), **przepływy NetFlow/sFlow/IPFIX**, logi urządzeń (przełączniki, zapory, AP, DHCP, DNS, proxy), logi systemów i usług (Event Log, syslog, `auth.log`), **IDS/IPS**, **kontrola dostępu (802.1X/RADIUS/ISE)**, skanery, EDR, honeypoty, threat intelligence (zob. temat 4 i 9).

## SIEM i SOAR (kurs Cisco, Moduł 2)

**SIEM** – technologia do **raportowania w czasie rzeczywistym i długoterminowej analizy zdarzeń** bezpieczeństwa; powstał z połączenia **SIM** (zarządzanie informacją) i **SEM** (zarządzanie zdarzeniami). Wykorzystuje **kolektory logów** z urządzeń bezpieczeństwa, sieciowych, serwerów i aplikacji.

| Funkcja SIEM | Opis |
| :--- | :--- |
| **Korelacja** | analiza logów z różnych systemów, przyspieszenie wykrycia |
| **Agregacja** | redukcja wolumenu przez łączenie duplikatów zdarzeń |
| **Analiza kryminalistyczna** | przeszukiwanie logów i zapisów zdarzeń z całej organizacji |
| **Zatrzymanie (retencja)/raportowanie** | skorelowane dane w monitoringu na żywo i podsumowaniach długoterminowych |

Cele monitoringu SIEM: **identyfikacja zagrożeń wewnętrznych i zewnętrznych**, monitorowanie aktywności i wykorzystania zasobów, **raporty zgodności** dla audytów, **wsparcie reagowania na incydenty**. Po wykryciu SIEM może zarejestrować dodatkowe informacje, wygenerować alert i **zlecić innym kontrolom zatrzymanie działania**; zaawansowane – **UEBA** (analiza zachowań użytkowników i jednostek). SIEM bywa **kosztowny** i opłaca się dla milionów zdarzeń dziennie.

SIEM dostarcza kontekst źródła aktywności: **użytkownik** (nazwa, status uwierzytelnienia, lokalizacja, grupa, kwarantanna), **urządzenie** (model, system, MAC, sposób połączenia), **postawa (posture)** (zgodność z polityką, wersja antywirusa, poprawki, zgodność MDM). Pomaga odpowiedzieć: kto, czy ważny, czy uprawniony, jakie ma inne dostępy, czy to kwestia zgodności.

**SOAR** – zbiera dane o zagrożeniach z wielu źródeł i **reaguje na zdarzenia niskiego poziomu bez człowieka**; trzy funkcje: **zarządzanie zagrożeniami i podatnościami**, **reagowanie na incydenty**, **automatyzacja operacji**; integracja z SIEM.

## Proces od monitoringu do reakcji

```
 zbieranie danych → baseline i korelacja (SIEM) → alert → ocena (triage) → analiza
   → ograniczenie (izolacja, blokada) → usunięcie przyczyny → odtworzenie → wnioski (post-mortem)
```

Wykład o ryzyku: monitorowanie obejmuje analizę logów, **alertów bezpieczeństwa**, wskaźników wydajności systemów ochrony i trendów aktywności użytkowników i sieci; ciągła ocena wykrywa słabości zanim zostaną wykorzystane.

## Analiza incydentu – co daje monitoring

- **oś czasu** zdarzeń (kiedy zaczął się atak, jak się rozwijał),
- **zakres** (które hosty, konta, dane),
- **wektor wejścia** (phishing, podatność, brute force),
- **IOC** do wyszukiwania w innych miejscach i do reguł IDS,
- **dowody** do postępowania (logi, pcap, obrazy), **wnioski** do poprawy.

## Wyzwania monitoringu *(uzupełnienie)*

szyfrowanie ruchu (TLS) ogranicza widoczność, ogromny wolumen danych, **fałszywe alarmy i zmęczenie alertami**, rozproszenie źródeł, konieczność **synchronizacji czasu (NTP)**, koszty i kompetencje, prywatność i prawo (RODO, regulamin monitoringu), ochrona samych logów.

## Podsumowanie

- Monitoring daje **widoczność**, wczesne wykrywanie, możliwość **analizy kryminalistycznej**, szybką reakcję i dowody zgodności.
- Podstawa wykrywania: **profil (baseline) normalnego ruchu i serwerów** + **wykrywanie anomalii (NBA, reguły)**.
- Narzędzia zbiorcze: **SIEM** (korelacja, agregacja, forensics, retencja) i **SOAR** (automatyzacja reakcji).

---
[⬅️ Poprzedni temat](2_Podatności_Ethernetu_i_protokołów_lokalnych_sieci_komputerowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Narzędzia_do_monitorowania_i_analizy_ruchu_w_lokalnych_sieciach_komputerowych.md)