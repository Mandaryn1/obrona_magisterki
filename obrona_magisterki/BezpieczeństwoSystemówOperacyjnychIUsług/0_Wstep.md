# Bezpieczeństwo systemów operacyjnych i usług – wprowadzenie

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