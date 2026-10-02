# Metody zapobiegania nieuprawnionemu dostępowi do zasobów sprzętowych i programowych

> Opracowanie oparte na wykładach *Mechanizmy bezpieczeństwa komputerowego* (kontrola dostępu, MFA, RBAC, IAM, SSO, TPM – slajdy 5–47), *Zapory/IDS* i W8 (utwardzanie); uzupełnienia oznaczone ***(uzupełnienie)***.

## Zasada: model AAA i wielowarstwowość

Dostęp do zasobu przechodzi przez trzy kroki (wykład, MBK1, slajdy 6–9):

1. **Identyfikacja** – kim jest podmiot (login, identyfikator urządzenia),
2. **Uwierzytelnianie** (*authentication*) – weryfikacja tożsamości,
3. **Autoryzacja** (*authorization*) – jakie uprawnienia ma uwierzytelniony podmiot,
4. **Rozliczalność/audyt** (*accounting*) – rejestracja działań (logi).

Zapobieganie nieuprawnionemu dostępowi wymaga **kilku warstw** (obrona w głąb): fizycznej, sieciowej, systemowej, aplikacyjnej i organizacyjnej.

## 1. Ochrona zasobów sprzętowych (fizyczna i platformowa)

| Metoda | Opis |
| :--- | :--- |
| **Kontrola dostępu fizycznego** | zamki, karty/biometria, monitoring (CCTV), rejestr wejść, strefy (serwerownia), ochrona okablowania i szaf (wykład, W1, slajd 23: kradzież, sabotaż, nieautoryzowany dostęp) |
| **Blokowanie portów i nośników** | wyłączenie nieużywanych gniazd sieciowych/USB, kontrola urządzeń (**device control**), ochrona przed *baiting* (zainfekowany USB) |
| **Hasło BIOS/UEFI, blokada rozruchu z zewnętrznych nośników, Secure Boot** | uniemożliwia start z obcego systemu i modyfikację bootloadera |
| **TPM (Trusted Platform Module)** (wykład, MBK1, slajdy 40–47) | sprzętowy układ przechowujący klucze; **pomiar rozruchu (PCR)**, klucz dysku „zapieczętowany" do stanu platformy; **BitLocker/LUKS z TPM (+PIN)**; ochrona kluczy prywatnych certyfikatów; **Credential Guard** (hashe haseł w VBS) |
| **Ochrona przed atakiem „Evil Maid"** | atakujący z fizycznym dostępem modyfikuje bootloader; **TPM** zmienia PCR → odmawia wydania klucza, dysk pozostaje zaszyfrowany (wykład, slajd 47) |
| **Szyfrowanie dysków (FDE)** | BitLocker, VeraCrypt, LUKS – utrata sprzętu nie oznacza utraty danych |
| **Zabezpieczenie urządzeń sieciowych** | hasło na konsolę, wyłączone nieużywane porty, plomby, kontrola dostępu do szaf |
| **Inwentaryzacja i kontrola urządzeń w sieci** | wykrycie nieautoryzowanych (rogue) urządzeń |

## 2. Kontrola dostępu do sieci

| Metoda | Opis |
| :--- | :--- |
| **802.1X / NAC** | uwierzytelnienie urządzenia/użytkownika (RADIUS) **przed** dostępem do portu/Wi-Fi; kwarantanna niezgodnych urządzeń |
| **Port Security, DHCP snooping, DAI** | ograniczenie MAC, kontrola serwera DHCP i ARP (zob. temat 5) |
| **Segmentacja (VLAN, strefy)** | użytkownik widzi tylko swój segment |
| **Zapory** | kontrola ruchu między strefami (zob. temat 10) |
| **VPN i dostęp zdalny** | uwierzytelniony, szyfrowany dostęp (IPsec, OpenVPN, WireGuard – wykład, slajd 24); **ZTNA** |
| **Bezpieczne zarządzanie urządzeniami** | SSH zamiast Telnet, osobna sieć zarządzania, **TACACS+/RADIUS** (AAA), wyjątki tylko z listy ACL |
| **WPA2/WPA3-Enterprise** | bezpieczne Wi-Fi |

## 3. Uwierzytelnianie (wykład, MBK1, slajdy 14–22)

| Metoda | Charakterystyka |
| :--- | :--- |
| **Hasła** | wymagana złożoność, długość, brak powtórzeń, haszowanie z solą; polityka haseł (wykład W8), **ochrona przed brute force** |
| **Tokeny** | sprzętowe (YubiKey, FIDO2), aplikacje TOTP; certyfikaty/karty |
| **Biometria** | odcisk palca, twarz; ryzyko fałszerstw i nieodwracalność wzorca |
| **MFA** | co najmniej dwa różne czynniki: **wiem / mam / jestem**; ogranicza skutki wycieku hasła (wykład, MBK1, slajdy 18–22; W8, slajdy 20–21) |
| **Kerberos / SSO** | jedno logowanie do wielu usług (Windows – Kerberos; Linux – SSSD/Kerberos, LDAP); mechanizm biletów (TGT, ST) – wykład, MBK1, slajdy 34–39 |
| **Klucze SSH** | zamiast haseł; wyłączenie logowania root i hasłem |

**Ochrona przed brute force (wykład W8, slajd 32):** **Fail2Ban** (blokada IP po N nieudanych próbach, integracja z iptables/firewalld), **Account Lockout Policy** (GPO), **rate limiting**, **CAPTCHA**.

## 4. Autoryzacja i zarządzanie uprawnieniami (wykład, MBK1, slajdy 9–13, 23–33)

- **Zasada najmniejszych uprawnień** (wykład W8, slajd 6): dostęp wyłącznie do niezbędnych zasobów; regularne przeglądy uprawnień; audyt aktywności.
- **ACL (listy kontroli dostępu)**: Windows – NTFS/DACL (`icacls`), Linux – prawa Unix + POSIX ACL (`setfacl`).
- **RBAC** – uprawnienia przypisane do ról (grupy AD, role w systemach, `sudo`); prostsze zarządzanie, mniej błędów.
- **IAM** – cykl życia tożsamości: tworzenie, zmiany, usuwanie, przeglądy, standardy (SAML, OAuth 2.0, OIDC), **PAM** dla kont uprzywilejowanych.
- **MAC** (obowiązkowa kontrola dostępu): **SELinux, AppArmor** (wykład W8, slajd 13), **sudo** zamiast logowania jako root.
- **Separacja obowiązków**, **just-in-time access** (Zero Trust – wykład, W7, slajd 19).

## 5. Ochrona zasobów programowych (aplikacji i systemów)

| Metoda | Opis |
| :--- | :--- |
| **Utwardzanie systemów** (wykład W8) | usunięcie zbędnych usług i protokołów, minimalna powierzchnia ataku, bezpieczne konfiguracje (CIS, OpenSCAP) |
| **Aktualizacje i poprawki** | łatanie podatności (WannaCry – lekcja: patch management, wykład, slajd 35) |
| **Biała lista aplikacji** | **AppLocker** (Windows), kontrola uruchamianych programów |
| **Antywirus/EDR, HIPS** | wykrywanie malware, kontrola procesów (wykład, slajdy 15, 20) |
| **Zapory hostowe** | Windows Defender Firewall, iptables/nftables |
| **Szyfrowanie danych** | w spoczynku i w tranzycie (TLS, IPsec) |
| **Walidacja i zabezpieczenia aplikacji** | WAF, parametryzowane zapytania, zarządzanie sesją |
| **Kontenery i izolacja** | minimalne obrazy, uprawnienia, Network Policies |
| **Klasyfikacja i DLP** | ochrona wrażliwych danych przed wyciekiem |

## 6. Wykrywanie prób nieuprawnionego dostępu (audyt – wykład, MBK1, slajdy 49–50)

- **Windows:** Event Log Security – 4624 (udane), **4625 (nieudane logowanie)**, 4670 (zmiana uprawnień), 4663 (dostęp do pliku).
- **Linux:** `/var/log/auth.log` (secure), **auditd** (`auditctl -w /etc/passwd -p wa -k passwd_changes`).
- **SIEM** – agregacja i korelacja; alerty i **automatyczne blokowanie** (IP po wielu nieudanych logowaniach); rejestr prób naruszeń; procedury reakcji; post-mortem.

## 7. Czynnik ludzki

**Szkolenia** (phishing, socjotechnika, tailgating – wykład, W1, slajd 36), polityki haseł i urządzeń, zgłaszanie incydentów, kontrola dostępu fizycznego (zasada „nie wpuszczaj za sobą").

## Podsumowanie (schemat warstwowy)

| Warstwa | Przykładowe środki |
| :--- | :--- |
| fizyczna | zamki, karty, CCTV, blokada USB, Secure Boot, **TPM**, FDE |
| sieciowa | **802.1X/NAC**, VLAN, zapory, VPN, port security, WPA-Enterprise |
| tożsamości | **MFA**, SSO/Kerberos, silne hasła, klucze SSH, IAM/PAM |
| uprawnień | **najmniejsze uprawnienia**, ACL, **RBAC**, MAC (SELinux), sudo |
| systemowa i aplikacyjna | utwardzanie, aktualizacje, **AppLocker**, AV/EDR, WAF, szyfrowanie |
| nadzór | logi, SIEM, audyt, reagowanie na incydenty |
| ludzie | szkolenia i procedury |

---
[⬅️ Poprzedni temat](8_Monitorowanie_ruchu_sieciowego_w_wykrywaniu_nieprawidłowości_i_incydentów.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️️](10_Segmentacja_sieci_minimalizacja_uprawnień_inspekcja_ruchu_i_kontrola_dostępu.md)