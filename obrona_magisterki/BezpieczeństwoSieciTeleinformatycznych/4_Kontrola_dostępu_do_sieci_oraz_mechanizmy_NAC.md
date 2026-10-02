# Kontrola dostępu do sieci oraz mechanizmy NAC

> Opracowanie głównie z własnej wiedzy ***(uzupełnienie)***. Z wykładów: AAA, uwierzytelnianie vs autoryzacja, MFA, Kerberos/SSO (*Mechanizmy bezpieczeństwa*, slajdy 7–9, 14–22, 33–39), 802.1X/NAC w IoT (zalecenia dla administratorów), Cisco ISE (kurs Cisco), obrona w głąb i Zero Trust.

## Po co kontrola dostępu do sieci

Tradycyjnie „kto wpiął kabel lub połączył się z Wi-Fi – jest w sieci". **Kontrola dostępu do sieci (Network Access Control, NAC)** zapewnia, że **tylko uwierzytelnione i zgodne z polityką** urządzenia i użytkownicy otrzymują dostęp, **w odpowiednim zakresie**. Chroni przed:

- podpięciem obcych urządzeń (*rogue devices*, ataki L2, podsłuch),
- użyciem skradzionych poświadczeń z niezaufanego urządzenia,
- zainfekowanymi lub niezałatanymi urządzeniami (BYOD, goście, IoT),
- ruchem bocznym po przełamaniu perymetru.

## Pojęcia: AAA (wykład)

| Element | Pytanie | Mechanizmy |
| :--- | :--- | :--- |
| **Identyfikacja** | kim jesteś? | login, certyfikat, adres MAC |
| **Uwierzytelnianie** (*Authentication*) | czy jesteś tym, za kogo się podajesz? | hasła, tokeny, biometria, **certyfikaty**, **MFA** |
| **Autoryzacja** (*Authorization*) | co możesz robić? | VLAN, ACL, polityki, role (RBAC) |
| **Rozliczalność** (*Accounting*) | co zrobiłeś? | logi, czas sesji, ilości danych |

Wykład: *uwierzytelnianie odpowiada „Kim jesteś?", autoryzacja – „Co możesz zrobić?"*; metody: hasła, tokeny, biometria, **certyfikaty cyfrowe**; **MFA** (wiem/mam/jestem) znacząco redukuje ryzyko przejęcia konta.

## NAC – definicja i funkcje

**NAC** to **zestaw technologii i polityk** egzekwujących zasady dostępu do sieci na podstawie **tożsamości**, **typu i stanu urządzenia (postury)** oraz **kontekstu**. Typowe funkcje:

| Funkcja | Opis |
| :--- | :--- |
| **Uwierzytelnianie** użytkowników i urządzeń | 802.1X, MAB, portal WWW |
| **Autoryzacja i polityki dostępu** | przypisanie do VLAN, **dACL**, tagi **SGT** (TrustSec), ograniczenia czasowe |
| **Profilowanie urządzeń** | identyfikacja typu (drukarka, telefon IP, kamera, IoT) po DHCP, MAC (OUI), HTTP, SNMP, NetFlow |
| **Ocena postury (posture assessment)** | sprawdzenie zgodności urządzenia: **aktualne poprawki, antywirus, zapora, szyfrowanie dysku, zgodność z MDM** (kurs Cisco: SIEM wykorzystuje informacje o postawie) |
| **Kwarantanna i naprawa (remediation)** | niezgodne urządzenie trafia do VLAN naprawczego, otrzymuje instrukcje/aktualizacje |
| **Dostęp gości** | portal gościnny, rejestracja, sponsor, ograniczony czas i zasięg |
| **Onboarding BYOD** | rejestracja urządzeń własnych, wydanie certyfikatu |
| **Rozliczalność i raportowanie** | logi, integracja z **SIEM** |
| **Reakcja dynamiczna** | **CoA (Change of Authorization)** – zmiana uprawnień w trakcie sesji (np. izolacja po alercie IDS/SIEM) |

## Mechanizm podstawowy: IEEE 802.1X

**802.1X** – port-based network access control: **port (przewodowy lub radiowy) jest zablokowany**, dopóki klient nie zostanie uwierzytelniony.

```
 SUPPLICANT (klient) ◀─ EAPOL ─▶ AUTHENTICATOR (przełącznik / AP) ◀─ RADIUS ─▶ SERWER UWIERZYTELNIAJĄCY (AAA)
   urządzenie/użytkownik          pilnuje portu, przekazuje EAP          RADIUS, np. ISE, NPS, FreeRADIUS
                                                                                │
                                                          katalog (AD/LDAP), PKI, bazy
```

1. Klient podłącza się; port w stanie „nieautoryzowanym" przepuszcza tylko ramki **EAPOL**.
2. Przełącznik żąda tożsamości (**EAP-Request/Identity**).
3. Wymiana EAP jest **tunelowana w RADIUS** do serwera AAA.
4. Serwer weryfikuje i zwraca **Access-Accept** (z atrybutami: VLAN, ACL, SGT) lub **Access-Reject**.
5. Port przechodzi w stan autoryzowany; ruch trafia do wskazanego VLAN.

### Metody EAP

| Metoda | Uwierzytelnienie | Uwagi |
| :--- | :--- | :--- |
| **EAP-TLS** | wzajemne certyfikaty (klient i serwer) | **najbezpieczniejsza**; wymaga PKI |
| **PEAP (MSCHAPv2)** | certyfikat serwera + hasło w tunelu TLS | popularna w środowiskach Windows/AD |
| **EAP-TTLS** | tunel TLS + różne metody wewnętrzne | |
| **EAP-FAST** (Cisco) | PAC zamiast certyfikatów | |
| **EAP-MD5** | hasło | niezalecana (brak wzajemnego uwierzytelniania) |

### Tryby i wersje

- **Pojedynczy host / multi-host / multi-domain / multi-auth** na porcie (np. telefon IP + komputer),
- **MAB (MAC Authentication Bypass)** – dla urządzeń **bez supplicanta** (drukarki, kamery, IoT): uwierzytelnianie po adresie MAC (słabsze – MAC można podrobić; wymaga profilowania i ograniczonych uprawnień),
- **Web Authentication (WebAuth)** – portal przechwytujący dla gości,
- **MACsec (802.1AE)** *(uzup.)* – szyfrowanie L2 na łączu po uwierzytelnieniu 802.1X,
- w **Wi-Fi: WPA2/WPA3-Enterprise** (802.1X + RADIUS).

## Architektura NAC (rodzaje)

| Rodzaj | Opis |
| :--- | :--- |
| **Pre-admission** | kontrola **przed** dopuszczeniem (802.1X, MAB) |
| **Post-admission** | kontrola **po** dopuszczeniu (zmiana polityki, monitoring, CoA) |
| **Agentowy / bezagentowy** | agent na urządzeniu (posture) / ocena po stronie sieci (profilowanie, skan) |
| **Inline / out-of-band** | urządzenie w ścieżce ruchu / egzekwowanie na przełącznikach, AP, zaporach |
| **Zintegrowane z architekturą** | np. **Cisco ISE + TrustSec**, Aruba ClearPass, Forescout, FortiNAC, Microsoft NPS |

## Egzekwowanie polityki

- **VLAN dynamiczny** (z Access-Accept),
- **dACL (downloadable ACL)** – listy filtrujące pobierane z serwera,
- **SGT/SGACL (Security Group Tags, TrustSec)** – segmentacja oparta na rolach (niezależna od adresacji),
- **kwarantanna, izolacja**, ograniczona przepustowość (QoS),
- integracja z **zaporami** (pxGrid) i **SIEM/SOAR** (izolacja hosta po wykryciu).

## Wdrożenie NAC – dobre praktyki *(uzupełnienie)*

1. **Inwentaryzacja urządzeń** i profilowanie (co jest w sieci).
2. **Fazowo:** tryb **monitor/open mode** (logowanie bez blokowania) → **low-impact** → **closed mode**.
3. **PKI i certyfikaty urządzeń** (EAP-TLS) dla stacji zarządzanych; **MAB** dla urządzeń bez supplicanta z minimalnymi uprawnieniami.
4. **Segmentacja według ról** (użytkownicy, goście, IoT, drukarki, VoIP, zarządzanie).
5. **Redundancja serwerów AAA**, fail-open/fail-closed świadomie, tryb awaryjny.
6. **MFA** dla użytkowników zdalnych i administratorów; integracja z **SSO/Kerberos** (wykład: Kerberos i SSO w AD).
7. **Logowanie, SIEM, alerty** o nowych urządzeniach (wykład IoT: NAC z 802.1X, alerty o nowych urządzeniach, spike w transferze).
8. **Szkolenia i dokumentacja**, procedury dla wyjątków.
9. **Zero Trust** – ciągła weryfikacja, kontekstowa autoryzacja (wykład W7).

## Zagrożenia i obejścia NAC

podrabianie MAC (MAB), podpięcie urządzenia „za" uwierzytelnionym (hub, mostek 802.1X – brak MACsec), kradzież certyfikatów/poświadczeń, **MITM na EAP** (słabe PEAP bez walidacji certyfikatu serwera), ataki na serwer RADIUS (słaby *shared secret*), błędna konfiguracja fail-open, niezałatane urządzenia sieciowe.

## Znaczenie dla bezpieczeństwa i zgodności

NAC realizuje zasadę **najmniejszych uprawnień**, wspiera **segmentację**, **inwentaryzację zasobów** (ISO 27001, CIS Controls), **zgodność** (NIS2, PCI DSS – izolacja) i ograniczanie **ruchu bocznego**.

## Podsumowanie

- **NAC** = egzekwowanie dostępu do sieci wg tożsamości, typu i **postury** urządzenia: uwierzytelnianie (**802.1X**, MAB, WebAuth), autoryzacja (VLAN, dACL, SGT), profilowanie, **ocena postury i kwarantanna**, goście i BYOD, rozliczalność, **CoA**.
- 802.1X: **supplicant – authenticator – serwer RADIUS**, metody **EAP** (EAP-TLS najsilniejsza).
- Wdrażać etapowo, z PKI, segmentacją ról, redundancją AAA, monitoringiem i integracją z SIEM; uwaga na MAB, MITM EAP i brak MACsec.

---
[⬅️ Poprzedni temat](3_Zabezpieczanie_styku_sieci_teleinformatycznej_z_sieciami_zewnętrznymi.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Technologie_uwierzytelniania_i_autoryzacji_RADIUS_TACACS_plus_oraz_ISE.md)