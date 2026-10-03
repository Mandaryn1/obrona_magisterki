# Proces reagowania na incydenty oraz podstawy analizy powłamaniowej

## 1. Cykl życia reagowania na incydenty (NIST SP 800-61 / ISO 27035)

Proces reagowania na incydenty bezpieczeństwa to ustrukturyzowana metoda obsługi naruszeń, podzielona na cztery kluczowe fazy:

1. **Przygotowanie (Preparation):**
   * Tworzenie procedur, powoływanie zespołów reagowania (CSIRT/SOC) oraz wdrożenie narzędzi do monitorowania i zbierania logów.
2. **Detekcja i Analiza (Detection & Analysis):**
   * Wykrywanie nieprawidłowości na podstawie alertów z systemów SIEM, IDS, zintegrowanych menedżerów logów oraz zgłoszeń użytkowników.
   * Identyfikacja Wskaźników Kompromitacji (**IoC** — *Indicators of Compromise*), takich jak nieznane procesy, podejrzane połączenia sieciowe czy zmodyfikowane pliki systemowe.
3. **Zawieranie, Usuwanie i Przywracanie (Containment, Eradication & Recovery):**
   * **Zawieranie:** Izolacja zainfekowanych hostów od sieci LAN w celu powstrzymania rozprzestrzeniania się zagrożenia.
   * **Usuwanie:** Neutralizacja złośliwego oprogramowania, usuwanie kont utworzonych przez atakującego oraz podatności wykorzystanych do włamania.
   * **Przywracanie:** Bezpieczne odtwarzanie systemów i danych z czystych kopii zapasowych oraz stopniowe przywracanie usług do produkcji.
4. **Działania poincydentalne (Post-Incident Activity / Lessons Learned):**
   * Sporządzenie raportu końcowego, analiza wyciągniętych wniosków (*Lessons Learned*) oraz aktualizacja procedur i mechanizmów obronnych w celu uniknięcia ponownego włamania.

---

## 2. Podstawy analizy powłamaniowej (Digital Forensics / DFIR)

Analiza powłamaniowa polega na zabezpieczeniu, gromadzeniu i badaniu materiału dowodowego w formie cyfrowej w celu odtworzenia przebiegu ataku.

* **Zasada Zachowania Łańcucha Dowodowego (Chain of Custody):**
  * Ścisła dokumentacja każdego kroku (kto, kiedy i czym pozyskał dowód), tworzenie kopii binarnych dysków (*forensic image*) oraz weryfikacja ich spójności wartościami funkcji skrótu (MD5/SHA-256).
* **Kolejność ulotności danych (Order of Volatility):**
  * Zbieranie dowodów rozpoczyna się od zasobów najbardziej ulotnych:
    1. Rejestry procesora i pamięć podręczna.
    2. Zawartość pamięci RAM (pamięć ulotna).
    3. Stan połączeń sieciowych i tabela ARP.
    4. Dane na nośnikach pamięci masowej (dyski twarde).
    5. Archiwa i kopie zapasowe.

---

## 3. Kluczowe artefakty i narzędzia w analizie śledczej

* **Pamięć RAM i aktywne procesy:**
  * Pobieranie zrzutu pamięci operacyjnej (*RAM dump*) do analizy ulotnych artefaktów (np. wstrzykniętych skryptów czy kluczy szyfrujących).
  * Analiza aktywnych połączeń TCP i otwartych portów za pomocą polecenia `netstat -abno`, co pozwala powiązać adres IP i port z konkretnym identyfikatorem procesu (PID) oraz odnaleźć go w Menedżerze zadań.
* **Dzienniki zdarzeń systemowych (Event Logs):**
  * **Windows Event Viewer:** Analiza logów aplikacji, systemu oraz zabezpieczeń. Kluczowe jest badanie zdarzeń oznaczonych jako błędy lub krytyczne oraz specyficznych Identyfikatorów Zdarzeń (Event ID — np. zdarzenie logowania).
  * **Linux Syslog / Journald:** Analiza plików logów w `/var/log/` (np. `auth.log`, `secure`, `syslog`).
* **Analiza ruchu sieciowego:**
  * Rejestrowanie i inspekcja pakietów przechwyconych za pomocą narzędzi takich jak Wireshark, `tcpdump` czy `tshark` w celu identyfikacji komunikacji z serwerami Command & Control (C2).
* **Systemy SIEM (Security Information and Event Management):**
  * Centralizacja oraz korelasja wpisów dzienników i alertów pochodzących z urządzeń sieciowych, zapór ogniowych oraz hostów w czasie rzeczywistym.

---

## 4. Podsumowanie do wypowiedzi na obronie

> *"Reagowanie na incydenty opiera się na cyklu NIST obejmującym przygotowanie, detekcję i analizę, zawieranie i usuwanie skutków ataku oraz wyciąganie wniosków po incydencie. Analiza powłamaniowa (DFIR) wymaga zachowania łańcucha dowodowego i zbierania danych według kolejności ulotności – rozpoczynając od pamięci RAM i aktywnych połączeń sieciowych (badanych m.in. poleceniem netstat pod kątem PID), po czym przechodzi do analizy dzienników zdarzeń (Windows Event Log / Syslog) i analizy pakietów sieciowych w narzędziach Wireshark i tcpdump."*

## Podsumowanie

- **Proces IR:** **przygotowanie → wykrywanie i analiza → ograniczanie, usuwanie, odtworzenie → działania po incydencie**; obowiązki: rejestr incydentów, automatyczne powiadomienia, procedury, post-mortem (wykład MBK1).
- **Forensics:** zabezpieczenie dowodów (kopie, hashe, łańcuch dowodowy, kolejność ulotności), analiza **logów, pamięci, procesów, utrwalenia, systemu plików**, **oś czasu**, IOC, zakres szkód; mapowanie na ATT&CK.
- Wymogi prawne: **RODO 72 h**, NIS2 24 h/72 h; wnioski → udoskonalenie zabezpieczeń.

---
[⬅️ Poprzedni temat](10_Bezpieczeństwo_podstawowych_usług_sieci_lokalnej_DHCP_DNS_NAT_HTTP_FTP.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](12_Testy_penetracyjne_ocena_podatności_oraz_bazy_CVE_i_MITRE_ATTCK.md)