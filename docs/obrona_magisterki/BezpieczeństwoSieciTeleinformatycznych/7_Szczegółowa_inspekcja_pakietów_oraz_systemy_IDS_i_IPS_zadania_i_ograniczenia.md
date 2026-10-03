# Szczegółowa inspekcja pakietów w wykrywaniu i blokowaniu zagrożeń. Zadania oraz ograniczenia systemów IDS i IPS w ochronie sieci teleinformatycznych

**Szczegółowa inspekcja pakietów (DPI, Deep Packet Inspection)** analizuje ruch **głębiej niż nagłówki L3–L4**: ładunek (payload) i protokół aplikacyjny, śledzi sesje i składa strumienie. Dzięki temu rozpoznaje aplikacje niezależnie od portu i wykrywa ataki ukryte w treści.

**Co wykrywa i blokuje:** SQL Injection, XSS, command injection, path traversal, exploity znanych luk (EternalBlue, Shellshock, Heartbleed), komunikację C2 botnetów, malware w pobieranych plikach, wyciek danych (DLP).

**Techniki:** dekodowanie protokołów, **składanie fragmentów i normalizacja** (przeciw ukrywaniu ataków), dopasowanie sygnatur i wzorców, analiza heurystyczna i behawioralna, identyfikacja aplikacji, inspekcja TLS (rozszyfrowanie na proxy) oraz analiza metadanych TLS (np. JA3). Kosztem jest wydajność, dlatego stosuje się akcelerację sprzętową (ASIC/FPGA).

**IDS (Intrusion Detection System)** to system **pasywny**: analizuje **kopię** ruchu (SPAN/TAP), wykrywa podejrzane zdarzenia i **generuje alerty**. Nie wpływa na ruch. Rodzaje: **NIDS** (sieciowy, np. Snort, Suricata) i **HIDS** (hostowy: logi, integralność plików, procesy, np. OSSEC, Wazuh).

**IPS (Intrusion Prevention System)** działa **inline** (w ścieżce ruchu) i oprócz wykrycia **blokuje** zagrożenie w czasie rzeczywistym (odrzucenie pakietów, blokada adresu, reset sesji). Rodzaje: **NIPS** i **HIPS**. Najczęściej jest elementem NGFW.

**Metody wykrywania:**

- **sygnaturowa:** wysoka precyzja dla znanych ataków, mało fałszywych alarmów, ale **bezradna wobec zero-day** i wymaga aktualizacji,
- **anomalii:** porównanie z profilem normalnego ruchu (ML), wykrywa nowe ataki, ale daje więcej fałszywych alarmów,
- **hybrydowa:** łączy oba podejścia.

**Zadania:** wykrywanie skanowania, brute force, exploitów, DDoS, C2; ciągły monitoring 24/7; alertowanie i integracja z **SIEM/SOC**; **rejestrowanie dowodów** do analizy powłamaniowej; aktywna blokada (IPS); wsparcie zgodności (RODO, PCI DSS).

**Ograniczenia:**

- **ruch szyfrowany** (ponad 90% to TLS) ogranicza wgląd w treść, a inspekcja TLS wymaga zaufanych certyfikatów, kosztuje wydajność i budzi kwestie prywatności,
- **fałszywe alarmy** i **zmęczenie alertami** (potrzebne dostrajanie, SOAR),
- **zero-day** niewidoczne dla sygnatur,
- **wydajność i skalowalność** przy wysokich przepustowościach,
- **IPS inline to potencjalny punkt awarii** (stąd HA i tryb bypass) oraz ryzyko blokady legalnego ruchu,
- możliwość **omijania** (fragmentacja, obfuskacja, tunelowanie),
- widzi tylko monitorowane segmenty i zależy od jakości sygnatur,
- wymaga kompetencji i nie zastępuje innych warstw.

**IDS a IPS:** IDS wykrywa i alarmuje (bezpieczny dla ruchu, ale reakcja jest ręczna), IPS wykrywa i **reaguje automatycznie**, ale fałszywy alarm może przerwać legalny ruch.

| Cecha | **IDS** | **IPS** |
| :--- | :--- | :--- |
| Tryb | pasywny (kopia ruchu, SPAN/TAP) | **inline** (w ścieżce ruchu) |
| Reakcja | **alert** | **blokada** + alert |
| Wpływ na ruch | brak | **opóźnienie, SPOF**, ryzyko fałszywej blokady |
| Fałszywy alarm | tylko zbędny alert | **zablokowanie legalnego ruchu** |
| Zastosowanie | monitoring, forensics, zgodność | aktywna ochrona |
| Wdrożenie | łatwiejsze | wymaga HA/bypass, testów |

**Dobre praktyki:** ciągłe dostrajanie reguł i aktualizacje sygnatur, tryb detekcji przed blokowaniem, whitelisting, redundancja IPS, integracja z SIEM i threat intelligence, testy skuteczności.

## Podsumowanie

- **DPI** analizuje ładunek i protokół aplikacyjny (L7): wykrywa SQLi, XSS, exploity, C2, malware; wyzwania: ruch szyfrowany (inspekcja TLS), wydajność, prywatność.
- **IDS** (pasywny, NIDS/HIDS, sygnatury/anomalie/hybryda) – alerty i logi; **IPS** (inline, NIPS/HIPS) – blokada w czasie rzeczywistym; „IDS wykrywa – IPS reaguje".
- **Ograniczenia:** fałszywe alarmy i alert fatigue, zero-day, szyfrowanie, wydajność i skalowalność, SPOF IPS, fragmentacja polityk; środki: dostrajanie, SOAR, inspekcja TLS, bypass, HA, integracja z SIEM i threat intelligence.

---
[⬅️ Poprzedni temat](6_Rodzaje_firewalli_oraz_zasady_tworzenia_polityk_i_reguł_bezpieczeństwa.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Adaptacyjne_urządzenia_zabezpieczające_i_ich_rola_w_sieciach_korporacyjnych.md)