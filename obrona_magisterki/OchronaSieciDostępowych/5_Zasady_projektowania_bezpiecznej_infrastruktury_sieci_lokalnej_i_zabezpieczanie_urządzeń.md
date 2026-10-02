# Zasady projektowania bezpiecznej infrastruktury sieci lokalnej. Metody zabezpieczania urządzeń końcowych i pośredniczących w sieci dostępowej

> Opracowanie głównie z własnej wiedzy ***(uzupełnienie)***; z materiałów wykorzystano Moduł 4 kursu Cisco (zarządzanie aktywami, konfiguracją, poprawkami, MDM), Moduł 1 (CIS Critical Security Controls, polityki) i wykład IoT/ryzyko.

## Część A. Zasady projektowania bezpiecznej infrastruktury LAN

### Zasady ogólne

| Zasada | Znaczenie |
| :--- | :--- |
| **Security by design** | bezpieczeństwo uwzględniane **od etapu projektu**, nie dodawane po fakcie (kurs Cisco: bezpieczeństwo operacyjne zaczyna się od planowania i wdrażania; zespół analizuje projekty, identyfikuje zagrożenia i słabe punkty) |
| **Obrona w głąb** | wiele niezależnych warstw (temat 7) |
| **Najmniejsze uprawnienia i need-to-know** | dostęp tylko do niezbędnego |
| **Domyślna odmowa** (*default deny*) | wszystko zabronione, co nie jest jawnie dozwolone |
| **Segmentacja i separacja stref** | ograniczenie ruchu bocznego |
| **Zero Trust** | nie ufać nikomu domyślnie; weryfikować urządzenie i użytkownika |
| **Redundancja i odporność** | brak pojedynczych punktów awarii (SPOF) |
| **Prostota i minimalizacja powierzchni ataku** | tylko niezbędne usługi, porty, protokoły |
| **Widoczność** | monitoring i logowanie od początku |
| **Zarządzalność** | inwentarz, standardowe konfiguracje, automatyzacja |
| **Zgodność z politykami i normami** | ISO 27001, CIS Controls, RODO, NIS2 |

### Architektura

**Model hierarchiczny** (dostęp – dystrybucja – rdzeń): warstwa **dostępu** (przełączniki dla stacji i AP; główne środki: 802.1X, port security, DHCP snooping, DAI), **dystrybucji** (routing między VLAN, ACL, polityki), **rdzeń** (szybki transport, redundancja). Alternatywnie architektura *collapsed core* w mniejszych sieciach.

**Strefy bezpieczeństwa i segmentacja:**

```
 Internet ─▶ [NGFW/IPS] ─▶ DMZ (WWW, poczta, proxy)
                │
        ┌───────┴────────────────────────────────────────────────┐
   sieć użytkowników   serwery/dane   zarządzanie (OOB)   goście   IoT/OT   telefonia VoIP
     (VLAN 10)         (VLAN 20)       (VLAN 99)          (VLAN 50) (VLAN 60)  (VLAN 70)
 komunikacja między strefami tylko przez zaporę/ACL, domyślna odmowa
```

- **VLAN-y** per rola/funkcja/poziom zaufania (użytkownicy, serwery, goście, IoT/OT, zarządzanie, VoIP), **osobna sieć zarządzania (out-of-band)**,
- **DMZ** dla usług publicznych, **mikrosegmentacja** dla wrażliwych systemów,
- **izolacja gości i urządzeń niezaufanych** (BYOD, IoT – wykład IoT: oddzielne VLAN-y/sieć gościnna),
- zalecane **adresacja i dokumentacja** (mapa topologii, przepływy),
- **redundancja** łączy i urządzeń (STP/RSTP, EtherChannel, HSRP/VRRP, zasilanie awaryjne),
- **ochrona fizyczna** (zamknięte szafy, kontrola dostępu, wyłączone nieużywane gniazda).

### Proces projektowania

1. analiza wymagań i **klasyfikacja zasobów** oraz **analiza ryzyka**,
2. projekt logiczny (strefy, VLAN, adresacja, polityki między strefami),
3. wybór mechanizmów (NAC, zapory, IDS/IPS, SIEM),
4. **standardowe konfiguracje (szablony)**, dokumentacja,
5. testy bezpieczeństwa przed uruchomieniem (**ST&E** – kurs Cisco),
6. eksploatacja: monitoring, aktualizacje, przeglądy, audyty.

## Część B. Zabezpieczanie urządzeń pośredniczących (przełączniki, routery, AP)

| Obszar | Środki |
| :--- | :--- |
| **Dostęp administracyjny** | konta imienne, **AAA** (TACACS+/RADIUS), **SSH zamiast Telnet**, brak HTTP – tylko HTTPS, ograniczenie ACL do sieci zarządzania, timeouty sesji, MFA dla administratorów, brak domyślnych kont i haseł, banery |
| **Utwardzanie** | wyłączenie niepotrzebnych usług (CDP/LLDP na portach brzegowych, HTTP, Telnet, SNMPv1/2c, TFTP), aktualizacja firmware, silne hasła (szyfrowane), kopia konfiguracji, wyłączenie nieużywanych portów |
| **Ochrona portów dostępowych** | **802.1X/NAC**, **port security** (limit MAC, sticky), **DHCP snooping**, **DAI**, **IP Source Guard**, **BPDU Guard/Root Guard**, storm control, porty `access` bez DTP, nieużywany **native VLAN**, nieużywane porty w VLAN „parking" i wyłączone |
| **Routing i trójka L3** | uwierzytelnianie protokołów routingu, uRPF, ACL, wyłączenie source routing/redirectów/directed broadcast |
| **Monitoring i logowanie** | syslog do SIEM, SNMPv3, NTP, NetFlow |
| **Zarządzanie konfiguracją** (kurs Cisco, Moduł 4) | inwentaryzacja i kontrola konfiguracji, **bazowe konfiguracje (baseline)**, kontrola zmian; wg NIST: ustanawianie i utrzymywanie **integralności** systemów przez kontrolę inicjalizacji, zmian i monitorowania konfiguracji (SP 800-128); narzędzia do automatyzacji: **Puppet, Chef, Ansible, SaltStack** |
| **Aktualizacje i poprawki** | identyfikacja, pozyskanie, dystrybucja, instalacja, weryfikacja (kurs Cisco: patch management) |
| **Wi-Fi** | WPA3/WPA2-Enterprise, zarządzanie AP, WIDS/WIPS (temat 6) |
| **Fizyczne** | zamki, brak dostępu do konsoli |

Przykład (Cisco IOS) – patrz poprzedni przedmiot (tematy 5, 11) *(uzupełnienie)*.

## Część C. Zabezpieczanie urządzeń końcowych (stacje, serwery, drukarki, urządzenia mobilne, IoT)

### Zarządzanie aktywami (kurs Cisco, Moduł 4)

Organizacja musi wiedzieć, **jaki sprzęt uzyskuje dostęp do sieci, gdzie się znajduje** (fizycznie i logicznie) oraz **jakie oprogramowanie i dane** przechowuje. Zarządzanie aktywami **śledzi zasoby i identyfikuje nieautoryzowane urządzenia**. Proces wg NIST:

1. **zautomatyzowane wykrywanie i inwentaryzacja** rzeczywistego stanu,
2. sformułowanie **pożądanego stanu** (polityki, plany, procedury),
3. **identyfikacja niezgodnych** autoryzowanych zasobów,
4. **naprawa lub akceptacja** stanu urządzenia, iteracja definicji,
5. powtarzanie cykliczne lub na bieżąco.

### Konfiguracja i utwardzanie hostów

- **bazowe obrazy systemów** (złote obrazy) z podstawowym oprogramowaniem, **oprogramowaniem zabezpieczającym punkty końcowe** i politykami ograniczającymi dostęp użytkownika do konfiguracji podatnych na ataki (kurs Cisco); konfiguracje sprzętowe mogą określać **dozwolone interfejsy sieciowe i zewnętrzne nośniki**,
- **usunięcie zbędnych usług**, zasada najmniejszych uprawnień, brak lokalnych administratorów, **zapora hostowa**, biała lista aplikacji (AppLocker), SELinux/AppArmor (poprzedni przedmiot, W8),
- **silne hasła + MFA**, blokada konta (*lockout*), Fail2Ban,
- **szyfrowanie dysku** (BitLocker/LUKS z TPM),
- **antywirus/EDR**, kontrola urządzeń USB, filtrowanie poczty i WWW,
- **autentykacja do sieci:** certyfikat/802.1X.

### Zarządzanie poprawkami (kurs Cisco, Moduł 4)

- obejmuje **identyfikację, pozyskanie, dystrybucję, instalację i weryfikację** poprawek; „instalowanie łatek jest często najskuteczniejszym sposobem ograniczania luk",
- wymagane m.in. przez przepisy zgodności; zaniedbania → niepowodzenie audytu i kary,
- **zależy od zarządzania aktywami** (wiadomo, gdzie jest oprogramowanie),
- narzędzia: SolarWinds, LANDesk, **Microsoft SCCM** (dystrybucja do stacji i serwerów Windows),
- **techniki:** **oparte na agentach** (agent na hoście zgłasza stan i instaluje poprawki; preferowane dla urządzeń mobilnych), **skanowanie bezagentowe** (serwer skanuje sieć; działa w przeskanowanych segmentach – problematyczne dla mobilnych), **pasywne monitorowanie sieci** (identyfikacja po ruchu; skuteczne, gdy oprogramowanie ujawnia wersję w ruchu).

### Kontrola dostępu urządzeń do sieci (NAC)

**802.1X** (supplicant – authenticator – RADIUS), **ocena postury** (zgodność: aktualne łatki, AV, szyfrowanie), **kwarantanna**/VLAN naprawczy dla urządzeń niezgodnych, profilowanie urządzeń (ISE), **MAB** dla urządzeń bez supplicanta (drukarki, IoT) z ograniczonym VLAN.

### Urządzenia mobilne (MDM) – zob. temat 11, IoT – temat 11

## Część D. Elementy organizacyjne

- **polityki** bezpieczeństwa (dopuszczalne użytkowanie, hasła, dostęp zdalny, konserwacja sieci, obsługa incydentów, dane) – kurs Cisco, Moduł 1,
- **CIS Critical Security Controls** (kurs Cisco): **podstawowe** – inwentaryzacja sprzętu i oprogramowania, ciągłe zarządzanie podatnościami, kontrolowane uprawnienia administracyjne, bezpieczne konfiguracje, **analiza logów audytu**; **fundamentalne** – zabezpieczenia poczty i przeglądarek, **ochrona przed malware**, **ograniczenie i kontrola portów, protokołów i usług**, odzyskiwanie danych, **bezpieczne konfiguracje urządzeń sieciowych**, **ochrona granic**, ochrona danych, **kontrola dostępu wg wiedzy koniecznej**, **kontrola dostępu bezprzewodowego**, monitorowanie kont; **organizacyjne** – szkolenia, bezpieczeństwo aplikacji, **reagowanie na incydenty**, **testy penetracyjne i ćwiczenia red team**,
- szkolenia użytkowników, procedury onboardingu/offboardingu (konta, uprawnienia, sprzęt).

## Lista kontrolna – bezpieczna sieć dostępowa

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