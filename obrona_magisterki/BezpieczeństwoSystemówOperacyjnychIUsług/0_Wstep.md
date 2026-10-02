# Bezpieczeństwo systemów operacyjnych i usług – wprowadzenie

> **Źródła.** Materiały z tego zipa (kurs Cisco): **W1 – Windows** (91 slajdów), **W2 – Linux** (56 slajdów; to same skany obrazów, odczytane OCR-em, więc cytaty mogą mieć drobne różnice), **W3–W5** (etyczny hacking, planowanie testów, rozpoznanie i skanowanie podatności). Uzupełniłem je wykładami z wcześniejszej paczki *Mechanizmy bezpieczeństwa komputerowego*: **kontrola dostępu, MFA, RBAC, IAM, SSO, TPM** (MBK1), **kryptografia, PKI, szyfrowanie dysków** (MBK2), **bezpieczeństwo aplikacji webowych** (W5 web), **utwardzanie systemów** (W8), **zarządzanie incydentami** (MBK1 slajd 50) oraz własną wiedzą.
>
> Oznaczenia: „**wykład**" – z Twoich materiałów, ***(uzupełnienie)*** – z mojej wiedzy.

## Pokrycie zagadnień materiałami

| Nr | Zagadnienie | Pokrycie |
| :-: | :--- | :--- |
| 1 | Cele bezpieczeństwa OS i usług sieciowych | dobre – W8 (cele), W2 (usługi i porty, utwardzanie) |
| 2 | Architektura OS z punktu widzenia bezpieczeństwa | dobre – W1 (HAL, tryby, boot, rejestr, procesy), W2 |
| 3 | Jądro, separacja przestrzeni, ochrona pamięci | **częściowe** – W1 (tryby, pamięć), W8 (jądro Linux, ASLR); reszta własna |
| 4 | Użytkownicy, uprawnienia, kontrola dostępu | bardzo dobre – W1, W2 (uprawnienia plików), MBK1 (ACL) |
| 5 | DAC, MAC, RBAC | dobre – MBK1 (RBAC, BLP/Biba/Clark-Wilson), W8 (SELinux/AppArmor) |
| 6 | Hasła, klucze SSH, 2FA, IAM, SSO | bardzo dobre – MBK1 (hasła, tokeny, MFA, IAM, SSO) |
| 7 | Zapobieganie i wykrywanie zagrożeń w OS | bardzo dobre – W1, W2, W8, MBK1 |
| 8 | Kryptografia w ochronie danych i komunikacji | dobre – MBK2 (FDE, PKI), W5 web (TLS) |
| 9 | Ataki na aplikacje webowe | bardzo dobre – W5 web |
| 10 | Usługi sieci lokalnej (DHCP, DNS, NAT, HTTP, FTP…) | **słabe** – W2 (lista portów, Telnet/UFW); reszta własna |
| 11 | Reagowanie na incydenty i analiza powłamaniowa | **częściowe** – MBK1 (obowiązki), logi W1/W2; reszta własna |
| 12 | Testy penetracyjne, podatności, CVE, ATT&CK | bardzo dobre – W3, W4, W5 (+ moduły Cisco z przedmiotu *Ochrona sieci dostępowych*) |

## Windows a Linux – filozofia (wykład MBK1, slajd 48)

| | **Windows** | **Linux** |
| :--- | :--- | :--- |
| Podejście | bezpieczeństwo „*out of the box*" – domyślne ustawienia chronią | bezpieczeństwo „*by configuration*" – minimalna instalacja, administrator decyduje |
| Narzędzia | graficzne, własne Microsoft (AD, GPO, BitLocker, Defender, Windows Hello) | wiersz poleceń, otwarte standardy (PAM, LDAP, Kerberos), SELinux/AppArmor |
| Kontrola dostępu | ACL w NTFS, AD, GPO | prawa Unix, POSIX ACL, SELinux/AppArmor, sudo |
| Zalety | łatwość zarządzania w korporacji | elastyczność, kontrola, otwartość |
| Wady | ograniczona interoperacyjność, atrakcyjny cel ataków | wymaga wiedzy, ryzyko błędnej konfiguracji |

## Słowniczek

| Skrót | Znaczenie |
| :--- | :--- |
| **TCB** | Trusted Computing Base – część systemu, której zaufanie jest konieczne |
| **HAL** | Hardware Abstraction Layer |
| **ACL / DACL / SACL** | lista kontroli dostępu / uznaniowa / systemowa (audyt) |
| **DAC / MAC / RBAC / ABAC** | uznaniowa / obowiązkowa / oparta na rolach / oparta na atrybutach kontrola dostępu |
| **SELinux / AppArmor** | moduły MAC w Linuksie |
| **UAC** | User Account Control (Windows) |
| **GPO / AD** | Group Policy Object / Active Directory |
| **PAM** | Pluggable Authentication Modules (Linux) |
| **MFA / TOTP / FIDO2** | uwierzytelnianie wieloskładnikowe / hasło jednorazowe czasowe / klucze sprzętowe |
| **IAM / SSO** | zarządzanie tożsamością i dostępem / jednokrotne logowanie |
| **TPM / UEFI / Secure Boot** | układ zaufania / firmware / weryfikacja podpisów rozruchu |
| **FDE** | Full Disk Encryption |
| **PKI / CA / CRL / OCSP** | infrastruktura klucza publicznego / urząd certyfikacji / listy unieważnień / status online |
| **ASLR / DEP(NX)** | losowanie układu pamięci / zakaz wykonywania danych |
| **HIDS / EDR / SIEM** | hostowy IDS / wykrywanie i reagowanie na punktach końcowych / korelacja logów |
| **CVE / CWE / CVSS / NVD** | identyfikator podatności / słabości / ocena dotkliwości / baza NIST |
| **ATT&CK / TTP / IOC** | baza taktyk i technik MITRE / taktyki-techniki-procedury / wskaźniki włamania |

---
[⬅️ Poprzedni temat](BezpieczeństwoSystemówOperacyjnychIUsług_tytul.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](1_Podstawowe_cele_bezpieczeństwa_systemów_operacyjnych_i_usług_sieciowych.md)