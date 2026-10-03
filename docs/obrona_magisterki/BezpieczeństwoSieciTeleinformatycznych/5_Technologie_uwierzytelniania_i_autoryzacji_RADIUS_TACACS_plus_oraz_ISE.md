# Technologie uwierzytelniania i autoryzacji: RADIUS, TACACS+ oraz ISE

**AAA (Authentication, Authorization, Accounting)** to framework centralnego zarządzania dostępem. Urządzenie sieciowe (przełącznik, AP, zapora, router, VPN) nie przechowuje poświadczeń, tylko pyta **serwer AAA**. Daje to centralne konta i polityki, natychmiastowe odebranie dostępu i jeden punkt audytu.

**RADIUS** (RFC 2865 i 2866, standard IETF):

- **UDP**, porty **1812** (uwierzytelnianie i autoryzacja) i **1813** (accounting).
- **Uwierzytelnianie i autoryzacja w jednej wymianie:** Access-Request → Access-Accept (z atrybutami: VLAN, ACL, czas sesji), Access-Reject lub Access-Challenge.
- **Szyfruje tylko hasło użytkownika**, reszta pakietu jest jawna. Dlatego używa się tunelowania EAP w TLS, silnego klucza współdzielonego i **RadSec** (RADIUS po TLS).
- **Zastosowanie:** **dostęp użytkowników do sieci**: 802.1X (przewodowo i Wi-Fi), VPN, hotspoty.
- Serwery: FreeRADIUS, Microsoft NPS, Cisco ISE, Aruba ClearPass.

**TACACS+** (pierwotnie Cisco, opisany w RFC 8907):

- **TCP**, port **49**.
- **Rozdziela** uwierzytelnianie, autoryzację i accounting.
- **Szyfruje całą treść pakietu.**
- **Autoryzacja poleceń:** precyzyjnie określa, które polecenia administrator może wydać na jakim urządzeniu, i rejestruje, kto co wpisał.
- **Zastosowanie:** **administracja urządzeniami sieciowymi** (router, przełącznik, zapora).

**Porównanie:** RADIUS jest otwarty, ma szerokie wsparcie i służy do **dostępu użytkowników** (802.1X, VPN). TACACS+ służy do **zarządzania urządzeniami**, ma pełne szyfrowanie i autoryzację poleceń. W praktyce stosuje się **oba**: RADIUS dla użytkowników, TACACS+ dla administratorów.

**Cisco ISE (Identity Services Engine)** to platforma polityk tożsamości łącząca AAA, NAC i profilowanie:

- działa jako serwer **RADIUS** (802.1X, MAB, WebAuth) i **TACACS+** (Device Administration),
- **profilowanie** urządzeń, **ocena postury** (poprawki, AV, szyfrowanie), dostęp **gości** i **BYOD** z wbudowanym CA,
- **TrustSec** (segmentacja po tagach SGT), **pxGrid** (wymiana kontekstu z zaporami, SIEM i EDR, reakcje automatyczne, **CoA**),
- integracja z Active Directory, LDAP i **MFA**,
- architektura: węzły **PAN** (administracja), **MnT** (monitoring i raporty), **PSN** (przetwarzanie żądań),
- polityka: **policy set → warunki → wynik autoryzacji** (np. VLAN, dACL, SGT). Przykład: użytkownik z grupy „Finanse" na zarządzanym urządzeniu z poprawną posturą dostaje VLAN 20, a niezgodny trafia do VLAN kwarantanny.
- alternatywy: Aruba ClearPass, Microsoft NPS, FreeRADIUS.

**Uzupełnienia:** Kerberos i SSO (bilety zamiast haseł), SAML/OAuth/OIDC (federacja), certyfikaty X.509 (EAP-TLS), PAM.

### Zagrożenia i dobre praktyki

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