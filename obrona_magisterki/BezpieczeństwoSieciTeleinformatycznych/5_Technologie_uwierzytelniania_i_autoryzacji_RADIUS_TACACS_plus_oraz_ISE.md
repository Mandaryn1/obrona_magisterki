# Technologie uwierzytelniania i autoryzacji: RADIUS, TACACS+ oraz ISE

> Opracowanie głównie z własnej wiedzy ***(uzupełnienie)***; z wykładów: AAA, MFA, Kerberos/SSO, SAML/OAuth2/OIDC (*Mechanizmy bezpieczeństwa*, slajdy 7–9, 14–22, 33–39), **Cisco ISE** jako system profilowania i kontroli dostępu (kurs Cisco), IAM i RBAC/ABAC (wykład chmura).

## Architektura AAA

**AAA (Authentication, Authorization, Accounting)** – framework centralnego zarządzania dostępem. Urządzenie sieciowe (NAS – Network Access Server: przełącznik, AP, zapora, router, VPN) **nie przechowuje poświadczeń**, lecz pyta **serwer AAA**.

```
 użytkownik/urządzenie ──▶ NAS (klient AAA) ──(RADIUS / TACACS+)──▶ serwer AAA ──▶ katalog (AD/LDAP), PKI, MFA
```

Korzyści: **centralizacja** kont i polityk, **spójne uprawnienia**, natychmiastowe odebranie dostępu, **audyt** w jednym miejscu, brak lokalnych haseł na dziesiątkach urządzeń.

Dwa główne zastosowania:

1. **dostęp użytkowników do sieci** (Wi-Fi, 802.1X, VPN) → zwykle **RADIUS**,
2. **administracja urządzeniami sieciowymi** (SSH/konsola do routerów, przełączników, zapór) → zwykle **TACACS+**.

## RADIUS (Remote Authentication Dial-In User Service)

- **Standard IETF** (RFC 2865 – uwierzytelnianie i autoryzacja, RFC 2866 – accounting); opracowany pierwotnie dla dostępu dial-up.
- **Transport: UDP** – porty **1812 (uwierzytelnianie/autoryzacja)** i **1813 (accounting)** (starsze 1645/1646).
- **Łączy uwierzytelnianie i autoryzację** w jednej wymianie: klient wysyła **Access-Request**, serwer odpowiada **Access-Accept** (z atrybutami autoryzacji: VLAN, ACL, czas sesji), **Access-Reject** lub **Access-Challenge** (np. kolejny krok MFA/EAP). Accounting: **Accounting-Request** (Start/Interim/Stop).
- **Atrybuty (AVP)** – elastyczne pary atrybut–wartość (np. `Tunnel-Private-Group-ID` dla VLAN, `Filter-Id`, `Session-Timeout`; atrybuty vendor-specific, np. Cisco AV-pair).
- **Ochrona:** szyfrowane jest **tylko hasło użytkownika** (atrybut User-Password, z użyciem *shared secret* i MD5); reszta pakietu jawna → w praktyce **tunelowanie** (EAP w TLS), **Message-Authenticator**, silny *shared secret*, izolacja sieci zarządzania; **RadSec** (RADIUS over TLS, TCP 2083) szyfruje całość; **Diameter** to następca RADIUS (telekomunikacja).
- **Zastosowania:** **802.1X (przewodowo i Wi-Fi)**, **VPN** (uwierzytelnianie użytkowników zdalnych), hotspoty, eduroam, **EAP** (EAP-TLS, PEAP).
- **Serwery:** FreeRADIUS, Microsoft **NPS**, Cisco **ISE**, Aruba ClearPass, Radiator.
- **Wady:** słabsza ochrona transmisji, mniej szczegółowa autoryzacja poleceń na urządzeniach, UDP (brak niezawodności).

## TACACS+ (Terminal Access Controller Access-Control System Plus)

- Pierwotnie **protokół Cisco** (następca TACACS i XTACACS); później opisany jako **RFC 8907** (informacyjny).
- **Transport: TCP, port 49** – niezawodność, wykrywanie awarii.
- **Rozdziela** uwierzytelnianie, autoryzację i accounting (każdy może być obsługiwany osobno, np. uwierzytelnienie przez AD, autoryzacja przez TACACS+).
- **Szyfruje całą treść pakietu** (poza nagłówkiem) – więcej niż RADIUS.
- **Autoryzacja poleceń (command authorization)** – **drobnoziarnista kontrola**, które polecenia konfiguracyjne i na jakich urządzeniach może wydać administrator (np. `show` tak, `configure terminal` nie; poziomy uprawnień/privilege level); **accounting poleceń** – pełny ślad, **kto wpisał jakie polecenie**.
- **Zastosowania:** **zarządzanie urządzeniami (device administration)**: router, switch, firewall, load balancer; zgodność (PCI DSS, SOX) i audyt administracyjny.
- **Serwery:** Cisco ISE (osobna licencja/usługa *Device Administration*), Cisco ACS (wycofany), tac_plus, TACACS+ w ClearPass, Fortinet.
- **Wady:** wsparcie głównie sprzętu sieciowego (Cisco i inni), mniej uniwersalny niż RADIUS, historycznie własnościowy.

### Porównanie RADIUS i TACACS+

| Cecha | **RADIUS** | **TACACS+** |
| :--- | :--- | :--- |
| Standard | **IETF (RFC 2865/2866)**, otwarty | Cisco → RFC 8907 |
| Transport / port | **UDP** 1812/1813 | **TCP** 49 |
| AAA | uwierzytelnianie i autoryzacja **połączone**, accounting osobno | **rozdzielone** A, A, A |
| Szyfrowanie | **tylko hasło** | **cała treść pakietu** |
| Autoryzacja poleceń | ograniczona | **tak (per polecenie, poziomy uprawnień)** |
| Główne zastosowanie | **dostęp do sieci** (802.1X, Wi-Fi, VPN) | **administracja urządzeń** |
| Accounting | sesji (czas, dane) | sesji i **poleceń** |
| Wsparcie | bardzo szerokie (wielu producentów) | sprzęt sieciowy (Cisco i część innych) |
| Elastyczność atrybutów | duża (AVP, VSA) | mniejsza |
| Typowo | użytkownicy końcowi | **administratorzy** |

W praktyce stosuje się **oba**: RADIUS dla użytkowników (802.1X/VPN) i TACACS+ dla administratorów.

## Cisco ISE (Identity Services Engine)

**Cisco ISE** – **platforma kontroli dostępu do sieci i polityk tożsamości** (kurs Cisco: system profilowania użytkowników i urządzeń, AAA; może zasilać SIEM danymi o użytkowniku, urządzeniu i postawie). Jest **silnikiem polityk** łączącym **AAA, NAC i profilowanie**.

### Funkcje

| Funkcja | Opis |
| :--- | :--- |
| **AAA – RADIUS** | uwierzytelnianie i autoryzacja 802.1X, MAB, WebAuth (przewodowo, Wi-Fi, VPN) |
| **TACACS+ (Device Administration)** | uwierzytelnianie administratorów i **autoryzacja poleceń** na urządzeniach sieciowych |
| **Profilowanie (Profiling)** | automatyczna klasyfikacja urządzeń (DHCP, HTTP, SNMP, NMAP, NetFlow, AD) |
| **Posture** | ocena zgodności: poprawki, AV, szyfrowanie, firewall; remediacja |
| **Guest (dostęp gości)** | portale, sponsor, samorejestracja, hotspot |
| **BYOD / onboarding** | rejestracja urządzeń, certyfikaty (wbudowany CA), **MDM** |
| **TrustSec** | segmentacja oparta na tagach **SGT** (niezależna od IP/VLAN) |
| **pxGrid** | wymiana kontekstu tożsamości z zaporami, SIEM, EDR; **reakcja automatyczna** (Rapid Threat Containment, CoA) |
| **Integracja z katalogami** | **Active Directory**, LDAP, SAML IdP, tokeny OTP/MFA, PKI |
| **Raportowanie i audyt** | logi sesji, zgodność |

### Architektura (węzły)

- **PAN (Policy Administration Node)** – administracja i konfiguracja,
- **MnT (Monitoring and Troubleshooting)** – logi i raporty,
- **PSN (Policy Service Node)** – **przetwarza żądania** (RADIUS, TACACS+, profilowanie, posture); możliwe skalowanie i HA,
- wdrożenie samodzielne (standalone) lub rozproszone.

### Polityka w ISE (zasada działania)

**Policy set → warunki (np. typ połączenia, grupa AD, typ urządzenia, postura) → wynik autoryzacji (profil: VLAN, dACL, SGT, redirect).** Przykład: *użytkownik z grupy „Finanse", urządzenie zarządzane z certyfikatem, postura zgodna → VLAN 20 + SGT „Finance"; niezgodna postura → VLAN kwarantanny + portal naprawy.*

### Alternatywy

**Aruba ClearPass**, **Microsoft NPS** (RADIUS w Windows Server), **FreeRADIUS**, **Forescout**, **FortiNAC**, **Cisco Secure Access / SASE** w chmurze.

## Inne technologie uwierzytelniania i autoryzacji (kontekst z wykładów)

| Technologia | Zastosowanie |
| :--- | :--- |
| **Kerberos** | SSO w AD: **TGT** (ważny 10 h) i bilety usług (wykład, slajd 36); wymaga synchronizacji czasu, bilety zamiast haseł |
| **LDAP / Active Directory** | katalog tożsamości |
| **SAML 2.0, OAuth 2.0, OpenID Connect** | federacja i SSO dla aplikacji; OIDC = warstwa uwierzytelniania nad OAuth (wykład, slajd 33) |
| **MFA / FIDO2 / tokeny** | silne uwierzytelnianie (wiem/mam/jestem); **SSO + obowiązkowe MFA** (wykład, slajd 39) |
| **Certyfikaty X.509 / PKI** | EAP-TLS, uwierzytelnianie urządzeń |
| **ACL, RBAC, ABAC** | autoryzacja (wykład, slajdy 10, 23–28) |
| **PAM** | zarządzanie dostępem uprzywilejowanym (konta administracyjne, sejfy haseł, nagrywanie sesji) |

## Zagrożenia i dobre praktyki

- **Serwery AAA to cel o wysokiej wartości** – utwardzenie, segmentacja, redundancja (min. dwa), aktualizacje, **silne shared secret**, ograniczenie klientów RADIUS po IP, **RadSec** tam, gdzie to możliwe,
- **MFA** dla administratorów i VPN, konta imienne (brak współdzielonych), **fallback do konta lokalnego** tylko awaryjnie,
- **EAP-TLS** + walidacja certyfikatu serwera po stronie klienta (ochrona przed fałszywym serwerem),
- **rozdzielenie ról** (administrator urządzeń ≠ administrator polityk), przegląd uprawnień, logowanie i SIEM, backupy konfiguracji,
- **synchronizacja czasu (NTP)** – wymagana dla Kerberos i logów,
- spójne polityki z ISMS (ISO 27001: kontrola dostępu), zgodność (NIS2/PCI DSS).

## Podsumowanie

- **AAA** – uwierzytelnianie, autoryzacja, rozliczalność z centralnym serwerem.
- **RADIUS** (UDP, IETF) – dostęp **użytkowników** do sieci (802.1X, Wi-Fi, VPN); szyfruje tylko hasło; atrybuty autoryzacji (VLAN, ACL).
- **TACACS+** (TCP 49, Cisco/RFC 8907) – **administracja urządzeń**; rozdzielone AAA, **szyfruje całość**, **autoryzacja poleceń**.
- **Cisco ISE** – platforma polityk tożsamości: **RADIUS + TACACS+**, NAC, profilowanie, posture, goście/BYOD, TrustSec (SGT), pxGrid; węzły PAN/MnT/PSN; alternatywy: ClearPass, NPS, FreeRADIUS.

---
[⬅️ Poprzedni temat](4_Kontrola_dostępu_do_sieci_oraz_mechanizmy_NAC.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_Rodzaje_firewalli_oraz_zasady_tworzenia_polityk_i_reguł_bezpieczeństwa.md)