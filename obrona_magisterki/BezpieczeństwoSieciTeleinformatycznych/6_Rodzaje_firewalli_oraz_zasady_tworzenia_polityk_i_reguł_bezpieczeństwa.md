# Rodzaje firewalli oraz zasady tworzenia polityk i reguł bezpieczeństwa

**Zapora (firewall)** to bariera kontrolująca ruch sieciowy według reguł bezpieczeństwa. Oddziela strefy o różnym poziomie zaufania (Internet, DMZ, LAN, serwery) i egzekwuje politykę.

**Rodzaje:**

- **Filtracja pakietów (L3–L4):** analizuje nagłówek każdego pakietu osobno (adresy IP, porty, protokół, flagi). Jest szybka i prosta, ale **nie widzi treści** ani stanu połączenia (ACL na routerach).
- **Zapory stanowe (stateful):** prowadzą **tabelę stanów sesji**, automatycznie dopuszczają ruch powrotny i odrzucają pakiety niezgodne ze stanem. Przykłady: iptables z conntrack, Cisco ASA, Windows Defender Firewall.
- **Zapory obwodowe (circuit-level):** kontrolują ustanowienie sesji (np. SOCKS), nie analizują treści.
- **Aplikacyjne / proxy (L7):** głęboka inspekcja protokołów (HTTP, FTP, SMTP, DNS), często jako pośrednik. **WAF** jest ich specjalizacją: chroni aplikacje WWW przed SQL Injection i XSS (np. ModSecurity, F5, Cloudflare).
- **NGFW (nowej generacji):** zapora stanowa plus **IPS, kontrola aplikacji niezależnie od portu, antymalware, inspekcja TLS**, filtrowanie URL, świadomość użytkownika (Palo Alto, Fortinet, Cisco Firepower).
- **UTM (Unified Thread Management):** jedno urządzenie z zaporą, IPS, antywirusem, VPN, filtrowaniem treści i antyspamem. Dobre dla MŚP, ale to pojedynczy punkt awarii.
- **Hostowe** (Windows Firewall, iptables/nftables, UFW) i chmurowe (Security Groups).

**Polityka a reguły:** polityka bezpieczeństwa (dokument: kto, z jakiej strefy, do czego) jest źródłem technicznych reguł zapory. Reguła zawiera m.in. strefę lub interfejs źródłowy i docelowy, adresy, usługę lub aplikację, użytkownika, **akcję** (allow, deny, reject), profile (IPS, AV), logowanie.

**Zasady tworzenia reguł:**

1. **Domyślna odmowa**: wszystko zabronione, co nie jest jawnie dozwolone; na końcu reguła *deny all* z logowaniem.
2. **Najmniejsze uprawnienia:** wąskie źródło, cel i usługa, unikanie `any`.
3. **Kolejność reguł:** oceniane od góry (first match), szczegółowe przed ogólnymi, bez reguł przesłoniętych.
4. **Filtrowanie w obu kierunkach:** także ruchu **wychodzącego (egress)** przeciw C2 i eksfiltracji.
5. **Segmentacja** i **anti-spoofing** (blokada adresów prywatnych i bogon na interfejsie zewnętrznym).
6. **Obiekty i grupy** zamiast pojedynczych adresów, komentarze i właściciel reguły.
7. **Zarządzanie zmianami:** wniosek, zatwierdzenie, test, wdrożenie i możliwość wycofania.
8. **Regularny przegląd reguł:** usuwanie nieużywanych, tymczasowych i zbyt szerokich.
9. **Logowanie do SIEM** i monitoring.
10. **Utwardzenie samej zapory:** aktualizacje, zarządzanie z sieci zarządzania, MFA, kopie konfiguracji, HA.
11. Testy po zmianach i zgodność z normami (ISO 27001, PCI DSS).

**Typowe błędy:** reguła `any any permit`, brak reguły końcowej z logiem, stare i nieudokumentowane reguły, brak filtrowania wychodzącego, otwarte zarządzanie z Internetu, fragmentacja polityk na wielu urządzeniach (pomagają systemy centralne, np. Panorama, FortiManager).

**Wniosek:** zapora jest jedną z warstw obrony w głąb. Jej skuteczność zależy od jakości polityki i dyscypliny w zarządzaniu regułami.

## Podsumowanie

- **Rodzaje:** filtrujące pakiety (L3–4), **stanowe** (tabela stanów, dynamiczne reguły), obwodowe, **aplikacyjne/proxy i WAF** (L7: SQLi, XSS…), **NGFW** (IPS, kontrola aplikacji, antymalware, inspekcja TLS, tożsamość), **UTM**, hostowe; wykonania: Cisco ASA, Palo Alto, Fortinet, pfSense, iptables/nftables, Windows Firewall.
- **Polityka → reguły:** domyślna odmowa, najmniejsze uprawnienia, kolejność first-match, ingress i egress, anti-spoofing, obiekty i dokumentacja, zarządzanie zmianami, regularny przegląd i audyt, logowanie do SIEM, utwardzenie i HA, zgodność z normami.
- Wyzwania: fragmentacja polityk (zarządzanie cyklem życia, Panorama/FortiManager), wydajność i inspekcja TLS.

---
[⬅️ Poprzedni temat](5_Technologie_uwierzytelniania_i_autoryzacji_RADIUS_TACACS_plus_oraz_ISE.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Szczegółowa_inspekcja_pakietów_oraz_systemy_IDS_i_IPS_zadania_i_ograniczenia.md)