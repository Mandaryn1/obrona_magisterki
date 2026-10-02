# Bezpieczeństwo sieci teleinformatycznych – wprowadzenie

> **Co zawierała paczka z materiałami.** Plik nazywał się *„bezpieczeństwo systemów operacyjnych i usług"* i zawiera 5 wykładów: **W1 – Windows**, **W2 – Linux**, **W3 – Wstęp do etycznego hackingu**, **W4 – Planowanie i określanie zakresu testów penetracyjnych**, **W5 – Gromadzenie informacji i skanowanie podatności** (kurs Cisco Ethical Hacker / PenTest+).
>
> - **W3, W4, W5** pasują do tego przedmiotu: tematy **1** (rozpoznanie, dostęp, socjotechnika) i **10** (etapy i metody testowania bezpieczeństwa sieci).
> - **W1 (Windows) i W2 (Linux)** dotyczą systemów operacyjnych, więc zostawiam je dla przedmiotu *Bezpieczeństwo systemów operacyjnych i usług*. W2 to w całości skany obrazów (bez warstwy tekstowej) – do tamtego przedmiotu przejrzę go wzrokowo.
> - Pozostałe tematy oparłem na **wykładzie o zaporach i IDS/IPS** z poprzedniej paczki *Mechanizmy bezpieczeństwa komputerowego* (tematy 6, 7, 8, 9), na jej slajdach o **ISO 27001, SZBI i NIST CSF** (temat 2), o **AAA/SSO/Kerberos** (tematy 4, 5) i o **AI w bezpieczeństwie IoT** (temat 11) oraz na własnej wiedzy.

| Zagadnienie | Pokrycie materiałami |
| :-: | :--- |
| 1 Ataki: rozpoznanie, dostęp, socjotechnika | **dobre** – W5 (rozpoznanie pasywne i aktywne), W3 (socjotechnika), poprzednie wykłady (phishing, APT) |
| 2 ISO 27001, NIS2, inne normy | **częściowe** – slajdy o ISO 27001/27002/SZBI/NIST CSF, W4 (PCI DSS, RODO); NIS2 – własna wiedza + sprawdzone w sieci |
| 3 Styk z sieciami zewnętrznymi | **słabe** – zapory, NAT, VPN, segmentacja (poprzednie wykłady) |
| 4 Kontrola dostępu do sieci, NAC | **słabe** – AAA, 802.1X w IoT (poprzednie wykłady) |
| 5 RADIUS, TACACS+, ISE | **słabe** – AAA, Kerberos, SSO; reszta z własnej wiedzy |
| 6 Rodzaje firewalli i polityki | **dobre** – wykład zapór |
| 7 DPI, IDS i IPS | **bardzo dobre** – wykład zapór/IDS/IPS |
| 8 Adaptacyjne urządzenia zabezpieczające | **częściowe** – Cisco ASA, NGFW, UTM z wykładu zapór |
| 9 VPN | **częściowe** – slajd o Firewall + NAT + VPN; reszta z własnej wiedzy |
| 10 Etapy i metody testowania | **bardzo dobre** – W3, W4, W5 + kurs Cisco z przedmiotu *Ochrona sieci dostępowych* |
| 11 AI i ML w ochronie sieci | **częściowe** – wykłady IoT i ryzyko, NBA z kursu Cisco |

Oznaczenia: „**wykład**" – z Twoich materiałów, ***(uzupełnienie)*** – z mojej wiedzy.

## Specyfika „sieci teleinformatycznej" w kontekście bezpieczeństwa

Sieć teleinformatyczna to infrastruktura łącząca systemy informatyczne i teleinformatyczne (LAN/WAN, łącza operatorskie, VPN, chmura), w której zabezpiecza się **granice** (styk z sieciami zewnętrznymi), **dostęp** (kto i co wchodzi do sieci), **ruch** (inspekcja, szyfrowanie) oraz **proces** (standardy, testy, monitoring).

```
 Internet / partnerzy / chmura
        │  (styk – temat 3)       ┌──────────────── zasady i zgodność (temat 2) ────────────────┐
   [router brzegowy / anty-DDoS]  │ ISO 27001, NIS2, PCI DSS, RODO, NIST CSF                    │
        │                         └──────────────────────────────────────────────────────────────┘
   [NGFW / ASA / IPS (7, 8)] ──── DPI, IDS/IPS, firewall (6) ──── VPN (9) ──── DMZ
        │
   [NAC 802.1X (4) + RADIUS/TACACS+/ISE (5)] ── segmenty LAN ── serwery ── użytkownicy
        │
   SIEM + ML (11) ◀── logi ◀── testy bezpieczeństwa i pentesty (10) ◀── atakujący (1)
```

## Słowniczek

| Skrót | Znaczenie |
| :--- | :--- |
| **NAC** | Network Access Control |
| **AAA** | Authentication, Authorization, Accounting |
| **RADIUS / TACACS+** | protokoły AAA |
| **ISE** | Cisco Identity Services Engine |
| **ASA / FTD** | Cisco Adaptive Security Appliance / Firepower Threat Defense |
| **NGFW / UTM / WAF** | zapora nowej generacji / zintegrowane zarządzanie zagrożeniami / zapora aplikacji WWW |
| **DPI** | Deep Packet Inspection |
| **IDS / IPS** | system wykrywania / zapobiegania włamaniom |
| **VPN / IPsec / IKE** | wirtualna sieć prywatna / zestaw protokołów / wymiana kluczy |
| **ISMS (SZBI)** | System Zarządzania Bezpieczeństwem Informacji |
| **NIS2** | dyrektywa UE 2022/2555 o cyberbezpieczeństwie |
| **KSC** | Krajowy System Cyberbezpieczeństwa (polska ustawa) |
| **PTES / OSSTMM / ISSAF** | metodyki testów penetracyjnych |
| **OSINT** | wywiad z otwartych źródeł |
| **ML / UEBA / NDR** | uczenie maszynowe / analiza zachowań / wykrywanie zagrożeń w sieci |
