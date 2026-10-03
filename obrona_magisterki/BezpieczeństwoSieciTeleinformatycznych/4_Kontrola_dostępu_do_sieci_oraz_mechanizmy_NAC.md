# Kontrola dostępu do sieci oraz mechanizmy NAC

**Kontrola dostępu do sieci** zapewnia, że do sieci wchodzą tylko **uwierzytelnione i zgodne z polityką** urządzenia i użytkownicy, w odpowiednim zakresie. Zastępuje założenie „kto wpiął kabel, ten jest w sieci". Chroni przed obcymi urządzeniami (rogue devices), użyciem skradzionych poświadczeń z niezaufanego sprzętu, zainfekowanymi hostami (BYOD, goście, IoT) i ruchem bocznym.

**Podstawa to AAA:** identyfikacja i **uwierzytelnianie** (kim jesteś), **autoryzacja** (co możesz: VLAN, ACL, role), **rozliczalność** (logi).

**NAC (Network Access Control)** to zestaw technologii i polityk egzekwujących dostęp według **tożsamości, typu i stanu urządzenia (postury)** oraz kontekstu.

**Funkcje NAC:**

- **uwierzytelnianie** użytkowników i urządzeń (802.1X, MAB, portal WWW),
- **autoryzacja** i polityki (przypisanie VLAN, pobierane ACL, tagi SGT),
- **profilowanie urządzeń:** rozpoznanie typu (drukarka, telefon IP, kamera) po DHCP, MAC, ruchu,
- **ocena postury:** sprawdzenie poprawek, antywirusa, zapory, szyfrowania dysku,
- **kwarantanna i naprawa** niezgodnych urządzeń,
- **dostęp gości i BYOD** (portal, rejestracja, ograniczony czas),
- **reakcja dynamiczna (CoA):** zmiana uprawnień w trakcie sesji, np. izolacja po alercie z IDS lub SIEM,
- raportowanie i integracja z SIEM.

 **Rodzaje architektury NAC:**

| Rodzaj | Opis |
| :--- | :--- |
| **Pre-admission** | kontrola **przed** dopuszczeniem (802.1X, MAB) |
| **Post-admission** | kontrola **po** dopuszczeniu (zmiana polityki, monitoring, CoA) |
| **Agentowy / bezagentowy** | agent na urządzeniu (posture) / ocena po stronie sieci (profilowanie, skan) |
| **Inline / out-of-band** | urządzenie w ścieżce ruchu / egzekwowanie na przełącznikach, AP, zaporach |
| **Zintegrowane z architekturą** | np. **Cisco ISE + TrustSec**, Aruba ClearPass, Forescout, FortiNAC, Microsoft NPS |

----

<br>**Mechanizm podstawowy: IEEE 802.1X** (kontrola dostępu na poziomie portu):

- **Supplicant** (klient) ↔ **Authenticator** (przełącznik lub AP) ↔ **serwer RADIUS** (np. Cisco ISE, Microsoft NPS).
- Port jest zablokowany, dopóki klient się nie uwierzytelni. Przepuszcza tylko ramki EAPOL, a wymiana EAP jest tunelowana w RADIUS do serwera. Serwer zwraca **Access-Accept** z atrybutami (VLAN, ACL) albo odrzucenie.
- **Metody EAP:** **EAP-TLS** (wzajemne certyfikaty, najsilniejsza), PEAP/MSCHAPv2, EAP-TTLS. EAP-MD5 jest niezalecany.
- **MAB** (uwierzytelnianie po adresie MAC) służy dla urządzeń bez klienta 802.1X (drukarki, IoT). Jest słabsze, bo MAC można podrobić, więc takie urządzenia dostają minimalne uprawnienia.
- W sieciach Wi-Fi odpowiada mu **WPA2/WPA3-Enterprise**. **MACsec (802.1AE)** szyfruje łącze po uwierzytelnieniu.

**Egzekwowanie polityki:** dynamiczny VLAN, pobierane ACL (dACL), **TrustSec/SGT** (segmentacja oparta na rolach), kwarantanna, integracja z zaporami i SIEM.

**Dobre praktyki wdrożenia:** inwentaryzacja urządzeń i profilowanie, **fazowo** (tryb monitorowania → ograniczenia → pełne egzekwowanie), PKI i certyfikaty dla urządzeń zarządzanych, segmentacja według ról, **redundancja serwerów AAA**, MFA, monitoring i szkolenia.

**Zagrożenia i obejścia:** podrabianie MAC (przy MAB), podpięcie urządzenia za już uwierzytelnionym (bez MACsec), MITM na EAP przy braku walidacji certyfikatu serwera, słaby klucz współdzielony RADIUS, błędna konfiguracja fail-open.

NAC realizuje zasadę najmniejszych uprawnień, wspiera segmentację i inwentaryzację zasobów oraz zgodność z ISO 27001, NIS2 i PCI DSS.

## Podsumowanie

- **NAC** = egzekwowanie dostępu do sieci wg tożsamości, typu i **postury** urządzenia: uwierzytelnianie (**802.1X**, MAB, WebAuth), autoryzacja (VLAN, dACL, SGT), profilowanie, **ocena postury i kwarantanna**, goście i BYOD, rozliczalność, **CoA**.
- 802.1X: **supplicant – authenticator – serwer RADIUS**, metody **EAP** (EAP-TLS najsilniejsza).
- Wdrażać etapowo, z PKI, segmentacją ról, redundancją AAA, monitoringiem i integracją z SIEM; uwaga na MAB, MITM EAP i brak MACsec.

---
[⬅️ Poprzedni temat](3_Zabezpieczanie_styku_sieci_teleinformatycznej_z_sieciami_zewnętrznymi.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Technologie_uwierzytelniania_i_autoryzacji_RADIUS_TACACS_plus_oraz_ISE.md)
