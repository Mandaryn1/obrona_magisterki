# Ochrona sieci dostępowych – wprowadzenie

> **Źródła i zakres pokrycia materiałami.** Dostarczone materiały to 4 moduły kursu Cisco **Cyber Threat Management** (zarządzanie i zgodność; testowanie bezpieczeństwa sieci; analiza zagrożeń; ocena podatności punktów końcowych). Dodatkowo wykorzystałem dwa wykłady z wcześniejszej paczki *Mechanizmy bezpieczeństwa komputerowego*: **Bezpieczeństwo IoT** (W10) i **Zarządzanie ryzykiem bezpieczeństwa informacji** (W11).
>
> | Zagadnienie | Pokrycie materiałami |
> | :-: | :--- |
> | 1 Zagrożenia w LAN | **brak** (własna wiedza; drobne odwołania do W1/zapór z poprzedniego przedmiotu) |
> | 2 Podatności Ethernetu i protokołów LAN | **brak** (własna wiedza) |
> | 3 Znaczenie monitorowania | **częściowe** – Moduł 2 (SIEM, SOAR), Moduł 4 (profilowanie sieci, wykrywanie anomalii) |
> | 4 Narzędzia monitorowania | **częściowe** – Moduł 2 (Nmap, SuperScan, sniffery, SIEM, narzędzia CLI), Moduł 4 (NetFlow, Wireshark) |
> | 5 Bezpieczna infrastruktura LAN, zabezpieczanie urządzeń | **słabe** – Moduł 4 (zarządzanie konfiguracją, aktywami, poprawkami, MDM) |
> | 6 Komunikacja bezprzewodowa | **brak** (własna wiedza; WPA3 – wykład IoT) |
> | 7 Ochrona wielowarstwowa | **słabe** (CIS Controls – Moduł 1; obrona w głąb – poprzedni przedmiot) |
> | 8 Audyt, testy penetracyjne, podatności | **bardzo dobre** – Moduły 2 i 4 |
> | 9 Alerty bezpieczeństwa (źródła, struktura, ocena) | **dobre** – Moduł 3 (threat intelligence, IOC, CVE), Moduł 4 (CVSS), Moduł 2 (SIEM) |
> | 10 Systemy zarządzania bezpieczeństwem informacji | **bardzo dobre** – Moduł 1 (ISO 27000, domeny, polityki), W11 (ryzyko, ISO 27001) |
> | 11 Systemy mobilne i IoT | **dobre dla IoT** (W10), **częściowe dla mobilnych** (MDM – Moduł 4) |
>
> Treści z materiałów oznaczam „**kurs Cisco**", „**wykład IoT**", „**wykład ryzyko**"; resztę ***(uzupełnienie)*** z własnej wiedzy. Wzmianki o prawie USA w Module 1 (FISMA, SOX, HIPAA…) pomijam – na obronie ważniejsze będą RODO i krajowe przepisy.

## Czym jest sieć dostępowa

**Sieć dostępowa (access network)** to warstwa sieci, w której **urządzenia końcowe** (komputery, telefony, drukarki, IoT, urządzenia mobilne) **uzyskują dostęp** do infrastruktury i usług – przez przełączniki dostępowe (Ethernet) i punkty dostępowe Wi-Fi. W modelu hierarchicznym Cisco: **warstwa dostępu** → dystrybucji → rdzenia. To tu najczęściej zaczynają się ataki (podpięcie obcego urządzenia, przejęty host, rogue AP), a ochrona dotyczy zarówno **sprzętu pośredniczącego** (przełączniki, AP, routery), jak i **urządzeń końcowych**.

## Mapa ochrony sieci dostępowej

```
 urządzenie końcowe ──▶ [port przełącznika / AP] ──▶ warstwa dystrybucji ──▶ rdzeń / Internet
   (hardening, EDR,       (802.1X, port security,       (VLAN, ACL, zapory)     (perymetr, IPS)
    MDM, patch)           DHCP snooping, DAI, WPA3)
              ▲                      ▲                          ▲
              └── monitoring (NetFlow, IDS, SIEM) ── zarządzanie podatnościami ── polityki/ISMS
```

## Słowniczek

| Skrót | Znaczenie |
| :--- | :--- |
| **LAN / WLAN** | sieć lokalna / bezprzewodowa sieć lokalna |
| **802.1X / NAC** | uwierzytelnianie portowe / kontrola dostępu do sieci |
| **VLAN, STP, ARP, DHCP** | podstawowe mechanizmy LAN |
| **SIEM / SOAR** | agregacja i korelacja logów / automatyzacja reakcji |
| **IOC / TTP** | wskaźniki włamania / taktyki i techniki atakującego |
| **CVE / CVSS / NVD** | identyfikator podatności / skala dotkliwości / baza podatności NIST |
| **ISMS (SZBI)** | System Zarządzania Bezpieczeństwem Informacji |
| **SOA** | Statement of Applicability – oświadczenie o stosowalności |
| **MDM / BYOD** | zarządzanie urządzeniami mobilnymi / własne urządzenia pracowników |
| **TIP / STIX / TAXII / MISP** | platforma i standardy wymiany informacji o zagrożeniach |
| **ST&E** | testy i ocena bezpieczeństwa (Security Test & Evaluation) |
