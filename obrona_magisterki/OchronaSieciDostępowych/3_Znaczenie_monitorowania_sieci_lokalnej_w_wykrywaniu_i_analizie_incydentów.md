# Znaczenie monitorowania sieci lokalnej w wykrywaniu i analizie incydentów bezpieczeństwa

**Monitorowanie sieci** to ciągłe zbieranie i analiza danych o ruchu i zdarzeniach. Jest podstawą wykrywania incydentów, bo **nie da się zareagować na to, czego się nie widzi**. Żadne zabezpieczenia nie dają stuprocentowej ochrony, więc trzeba szybko wykryć atak, który je ominął.

**Znaczenie:**

- **Widoczność:** wiemy, jakie urządzenia, usługi i połączenia są w sieci. Pozwala to wykryć **nieautoryzowane urządzenia** (rogue devices).
- **Wczesne wykrywanie:** krótszy czas od włamania do wykrycia oznacza mniejsze szkody (ważne przy APT, gdzie atakujący bywa w sieci miesiącami).
- **Analiza incydentu i forensics:** zapisane logi i dane o ruchu pozwalają odtworzyć przebieg ataku (co, kiedy, skąd i jak daleko się rozprzestrzenił).
- **Szybka reakcja:** alerty uruchamiają procedury reagowania, a **SOAR** może automatycznie blokować zagrożenie.
- **Zgodność z regulacjami:** RODO, NIS2 i ISO 27001 wymagają logowania i obsługi incydentów.
- **Doskonalenie zabezpieczeń:** analiza incydentów pokazuje słabe punkty i pozwala poprawić zasady i konfigurację.

**Profilowanie sieci (baseline):** monitoring zaczyna się od opisu **normalnego zachowania**: czas trwania sesji, przepustowość, używane porty, adresy, pary host–host i zachowanie użytkowników. Odchylenia od niego sygnalizują **anomalie** (nagły wzrost ruchu, nietypowe godziny, nowe połączenia, skanowanie, beaconing do serwera C2). Wykrywanie anomalii wspiera analiza zachowania sieci (NBA) i ML.

**Źródła i narzędzia:** logi, NetFlow, IDS/IPS, przechwytywanie pakietów (SPAN/TAP), **SIEM** (korelacja i agregacja zdarzeń z wielu źródeł, długie przechowywanie logów) i **SOAR** (automatyzacja).

**Wyzwania:** ruch szyfrowany, duża ilość danych, fałszywe alarmy i zmęczenie alertami, koszty i prywatność.

**Wniosek:** monitoring skraca czas wykrycia i reakcji oraz ogranicza skutki incydentu. Skutecznie działa tylko wtedy, gdy są jasno określone normalny ruch, alerty i procedury reagowania.

### Analiza incydentu – co daje monitoring

- **oś czasu** zdarzeń (kiedy zaczął się atak, jak się rozwijał),
- **zakres** (które hosty, konta, dane),
- **wektor wejścia** (phishing, podatność, brute force),
- **IOC** do wyszukiwania w innych miejscach i do reguł IDS,
- **dowody** do postępowania (logi, pcap, obrazy), **wnioski** do poprawy.

## Podsumowanie

- Monitoring daje **widoczność**, wczesne wykrywanie, możliwość **analizy kryminalistycznej**, szybką reakcję i dowody zgodności.
- Podstawa wykrywania: **profil (baseline) normalnego ruchu i serwerów** + **wykrywanie anomalii (NBA, reguły)**.
- Narzędzia zbiorcze: **SIEM** (korelacja, agregacja, forensics, retencja) i **SOAR** (automatyzacja reakcji).

---
[⬅️ Poprzedni temat](2_Podatności_Ethernetu_i_protokołów_lokalnych_sieci_komputerowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Narzędzia_do_monitorowania_i_analizy_ruchu_w_lokalnych_sieciach_komputerowych.md)