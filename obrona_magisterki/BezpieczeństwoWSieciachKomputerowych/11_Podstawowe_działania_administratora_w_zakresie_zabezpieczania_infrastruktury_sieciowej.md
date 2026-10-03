# Podstawowe działania administratora w zakresie zabezpieczania infrastruktury sieciowej

Administrator zabezpiecza infrastrukturę sieciową w sposób ciągły i warstwowy. Podstawowe działania to:

- **Inwentaryzacja i klasyfikacja zasobów:** wiedza, co jest w sieci (urządzenia, systemy, usługi, dane) i jak ważne to jest. Bez tego nie da się niczego chronić.
- **Utwardzanie (hardening):** wyłączenie zbędnych usług i portów, zmiana domyślnych haseł, bezpieczna konfiguracja według **CIS Benchmarks**, zarządzanie urządzeniami przez SSH, nie Telnet.
- **Aktualizacje i poprawki (patch management):** regularne łatanie systemów, aplikacji i firmware, z priorytetem dla luk krytycznych.
- **Ochrona perymetru i segmentacja:** **zapory**, IPS, **DMZ**, VLAN-y, **VPN** dla dostępu zdalnego, filtrowanie ruchu wejściowego i wyjściowego, domyślna odmowa.
- **Kontrola dostępu i tożsamości:** silne uwierzytelnianie i **MFA**, **zasada najmniejszych uprawnień**, role (RBAC), 802.1X/NAC, zabezpieczenie przed brute force (blokady, fail2ban), osobne konta administracyjne.
- **Szyfrowanie:** TLS, IPsec/VPN, WPA3, szyfrowanie dysków i danych wrażliwych.
- **Monitoring i logowanie:** zbieranie logów, **IDS/IPS**, **SIEM**, NetFlow, alerty, analiza ruchu.
- **Kopie zapasowe i odtwarzanie:** reguła **3-2-1** (trzy kopie, dwa nośniki, jedna poza lokalizacją), testy odtwarzania i plan ciągłości działania.
- **Reagowanie na incydenty:** procedury, kontakty, izolacja zagrożonych systemów, analiza powłamaniowa.
- **Ochrona punktów końcowych:** antywirus/EDR, zapora hostowa, szyfrowanie dysków.
- **Ochrona fizyczna:** kontrola dostępu do serwerowni i urządzeń.
- **Testy i audyty:** skanowanie podatności, testy penetracyjne, przegląd konfiguracji i reguł zapór.
- **Polityki i ludzie:** polityki bezpieczeństwa, dokumentacja, szkolenia użytkowników (phishing), zgodność z normami (ISO 27001, RODO, NIS2).

**Zasady nadrzędne:** **obrona w głąb**, najmniejsze uprawnienia, domyślna odmowa, ciągłe doskonalenie (monitorowanie, ocena ryzyka, poprawki).

## Lista kontrolna administratora

| Obszar | Pytanie kontrolne |
| :--- | :--- |
| Inwentaryzacja | czy znam wszystkie urządzenia i usługi? |
| Konfiguracja | czy usunięto zbędne usługi i domyślne hasła? |
| Aktualizacje | czy łatki krytyczne wdrożono w terminie? |
| Dostęp | czy MFA, minimalne uprawnienia, zarządzanie przez SSH/AAA? |
| Sieć | czy działa segmentacja i zapory default deny? |
| Monitoring | czy logi trafiają do SIEM i są analizowane? |
| Kopie | czy kopie są offline i testowane? |
| Incydenty | czy istnieje i jest ćwiczony plan reakcji? |
| Ludzie | czy użytkownicy są szkoleni? |

## Podsumowanie

- Administrator: **inwentaryzuje, utwardza, łata, segmentuje, kontroluje dostęp, szyfruje, monitoruje, wykonuje kopie, reaguje na incydenty i szkoli** – w ramach ciągłego cyklu PDCA.
- Podstawa: **najmniejsze uprawnienia, domyślna odmowa, minimalna powierzchnia ataku, obrona w głąb** oraz audyt i doskonalenie.

---
[⬅️ Poprzedni temat](10_Segmentacja_sieci_minimalizacja_uprawnień_inspekcja_ruchu_i_kontrola_dostępu.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](12_Praktyczne_znaczenie_Wireshark_nmap_i_systemowych_narzędzi_diagnostycznych.md)