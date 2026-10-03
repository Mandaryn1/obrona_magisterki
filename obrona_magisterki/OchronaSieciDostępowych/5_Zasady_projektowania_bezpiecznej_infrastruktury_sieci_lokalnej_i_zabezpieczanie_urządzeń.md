# Zasady projektowania bezpiecznej infrastruktury sieci lokalnej. Metody zabezpieczania urządzeń końcowych i pośredniczących w sieci dostępowej

**Zasady projektowania bezpiecznej sieci LAN:**

- **Security by design:** bezpieczeństwo uwzględnia się od początku projektu, a nie dodaje się po fakcie.
- **Obrona w głąb:** wiele niezależnych warstw zabezpieczeń.
- **Najmniejsze uprawnienia i domyślna odmowa:** dostęp tylko do tego, co niezbędne.
- **Zero Trust:** nie ufamy nikomu domyślnie, także wewnątrz sieci; ciągła weryfikacja.
- **Redundancja i prostota:** brak pojedynczych punktów awarii, a projekt przejrzysty i łatwy do zarządzania.
- **Architektura hierarchiczna:** warstwy dostępu, dystrybucji i rdzenia.
- **Segmentacja na strefy:** użytkownicy, serwery, DMZ, zarządzanie, goście, IoT/OT, z kontrolą ruchu między strefami zaporą.

**Zabezpieczanie urządzeń pośredniczących (przełączniki, routery, punkty dostępowe):**

- **Dostęp administracyjny:** SSH zamiast Telnetu, uwierzytelnianie centralne **AAA** (RADIUS/TACACS+), silne hasła, osobna sieć zarządzania.
- **Utwardzanie:** wyłączenie zbędnych usług i nieużywanych portów, zmiana domyślnych haseł, aktualizacja firmware, logowanie.
- **Zabezpieczenia L2:** **802.1X**, **port security**, **DHCP snooping**, **DAI**, **BPDU Guard**, wyłączenie DTP, zmiana VLAN-u natywnego.
- **Kontrola ruchu:** listy ACL, uRPF, ograniczenie ICMP, uwierzytelnianie protokołów routingu.
- **Sieć bezprzewodowa:** WPA3 lub WPA2/WPA3-Enterprise (802.1X), wyłączenie WPS, segmentacja SSID.

**Zabezpieczanie urządzeń końcowych:**

- **Aktualizacje i zarządzanie poprawkami** (agentowe, szczególnie dla urządzeń mobilnych).
- **Antywirus/EDR**, zapora hostowa, **szyfrowanie dysków** (BitLocker, LUKS).
- **Najmniejsze uprawnienia:** konta standardowe, brak pracy na koncie administratora, **MFA**.
- **Zarządzanie konfiguracją** i aktywami: wykrywanie urządzeń, wzorce konfiguracji (np. Ansible, Puppet), naprawa niezgodności, **CIS Controls**.
- **MDM** dla urządzeń mobilnych (zdalne wymazanie, wymuszanie szyfrowania), kontrola **BYOD**.
- **Zarządzanie IoT:** osobny segment, zmiana haseł, aktualizacje.
- **Kontrola dostępu do sieci (NAC):** ocena zgodności urządzenia (poprawki, AV) przed dopuszczeniem.

**Uzupełnienie:** monitoring i logi, kopie zapasowe, szkolenia użytkowników, ochrona fizyczna, regularne audyty i testy.

**Wniosek:** bezpieczna sieć dostępowa łączy dobry projekt (segmentacja, hierarchia, redundancja) z zabezpieczeniem każdego urządzenia i stałym zarządzaniem konfiguracją, aktualizacjami i monitoringiem.

### Lista kontrolna – bezpieczna sieć dostępowa

| Obszar | Kontrola |
| :--- | :--- |
| Projekt | segmentacja (VLAN/strefy), default deny, osobna sieć zarządzania, redundancja |
| Dostęp do sieci | 802.1X/NAC, port security, nieużywane porty wyłączone |
| L2 | DHCP snooping, DAI, BPDU Guard, brak DTP, native VLAN nieużywany |
| Urządzenia sieciowe | SSH/HTTPS, AAA, brak domyślnych haseł, aktualny firmware, logowanie |
| Hosty | hardening, EDR, szyfrowanie dysku, patch management, MFA |
| Zarządzanie | inwentarz, baseline konfiguracji, kontrola zmian |
| Monitoring | SIEM, NetFlow, IDS, alerty o nowych urządzeniach |
| Ludzie/organizacja | polityki, szkolenia, plan reakcji |

## Podsumowanie

- Zasady: **security by design, obrona w głąb, least privilege, default deny, segmentacja, Zero Trust, redundancja, widoczność, zarządzalność**.
- Urządzenia pośredniczące: **AAA/SSH, utwardzanie, 802.1X, port security, DHCP snooping, DAI, BPDU Guard, aktualizacje, zarządzanie konfiguracją**.
- Urządzenia końcowe: **inwentaryzacja aktywów, bazowe konfiguracje, patch management (agentowy/bezagentowy/pasywny), AV/EDR, szyfrowanie, MFA, NAC z oceną postury, MDM**.
- Spójne **polityki i CIS Controls** zapewniają kompletność.

---
[⬅️ Poprzedni temat](4_Narzędzia_do_monitorowania_i_analizy_ruchu_w_lokalnych_sieciach_komputerowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_Zagrożenia_komunikacji_bezprzewodowej_i_sposoby_jej_zabezpieczania.md)