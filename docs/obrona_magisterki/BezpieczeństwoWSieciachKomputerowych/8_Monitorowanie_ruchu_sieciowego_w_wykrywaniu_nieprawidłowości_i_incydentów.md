# Monitorowanie ruchu sieciowego w wykrywaniu nieprawidłowości i incydentów

**Monitorowanie ruchu sieciowego** to ciągłe zbieranie i analiza danych o ruchu i zdarzeniach w sieci, żeby szybko wykryć nieprawidłowości i incydenty. Pozwala odpowiedzieć, co dzieje się w sieci i czy to jest normalne. Kluczowa jest **wczesna detekcja**: im szybciej wykryjemy atak, tym mniejsze szkody.

**Źródła danych:**

- **przechwytywanie pakietów** (SPAN/TAP, Wireshark/tcpdump),
- **NetFlow/sFlow** (metadane przepływów),
- **logi** zapór, serwerów, systemów, DNS i proxy,
- **IDS/IPS** i EDR,
- logi kontroli dostępu (np. 802.1X) oraz **honeypoty**.

**Punkt wyjścia: profil normalnego ruchu (baseline).** Najpierw określamy, jak wygląda zwykła sieć (typowe porty, protokoły, wolumeny, godziny, pary hostów). Odchylenia od niego wskazują na anomalie. Normalny ruch trzeba opisywać osobno dla sieci i dla serwerów.

**Symptomy nieprawidłowości:**

- **skanowanie** (wiele połączeń do różnych portów lub hostów),
- **brute force** (wiele nieudanych logowań),
- **nagły wzrost ruchu** (DDoS, eksfiltracja danych),
- **beaconing** (regularne połączenia do zewnętrznego serwera C2),
- **tunelowanie DNS** (nietypowo długie lub częste zapytania),
- **ruch boczny** między hostami wewnętrznymi,
- **ataki L2** (powtarzające się odpowiedzi ARP, wiele adresów MAC, fałszywy DHCP),
- nietypowe godziny, kierunki i protokoły.

**Metody analizy:** sygnaturowa (znane ataki), anomalii (odchylenia od baseline, także ML) i hybrydowa.

**Korelacja i reakcja:** logi i zdarzenia trafiają do **SIEM**, który **agreguje i koreluje** je, generuje alerty i wspiera analizę powłamaniową (przechowywanie logów). **SOAR** automatyzuje odpowiedź (blokada IP, izolacja hosta).

**Wyzwania:** ruch szyfrowany (TLS) ogranicza analizę treści (pomagają metadane), duża skala danych, **fałszywe alarmy i zmęczenie alertami**, wydajność, prywatność i prawo.

**Wniosek:** skuteczny monitoring łączy dobre źródła danych, baseline, automatyczne alerty i procedury reagowania na incydenty.

## Podsumowanie

- Monitoring ruchu opiera się na **pakietach (SPAN/TAP), przepływach (NetFlow/sFlow), logach i IDS/IPS**, porównywanych z **profilem normalnego ruchu**.
- Wskaźniki: skanowanie, brute force, skoki wolumenu, beaconing C2, tunelowanie DNS, eksfiltracja, ruch boczny, spoofing L2.
- Wykrycia trafiają do **SIEM/SOC/SOAR**; reakcja wg procedur (rejestr, powiadomienie, izolacja, post-mortem).
- Wyzwania: szyfrowanie, skala, fałszywe alarmy, SPOF, prawo.

---
[⬅️ Poprzedni temat](7_Narzędzia_do_identyfikacji_ataków_na_protokoły_i_usługi_sieciowe.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](9_Metody_zapobiegania_nieuprawnionemu_dostępowi_do_zasobów_sprzętowych_i_programowych.md)