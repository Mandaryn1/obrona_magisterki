# Metody zapobiegania nieuprawnionemu dostępowi do zasobów sprzętowych i programowych

**Zasada ogólna:** zapobieganie nieuprawnionemu dostępowi opiera się na modelu **AAA** (uwierzytelnianie, autoryzacja, rozliczalność), zasadzie **najmniejszych uprawnień** i **obronie w głąb**, czyli kilku warstwach zabezpieczeń naraz.

**Zasoby sprzętowe:**

- **ochrona fizyczna:** zamykane pomieszczenia serwerowe, kontrola wstępu, monitoring, zabezpieczenie szaf i portów, blokada niewykorzystywanych portów USB,
- **szyfrowanie dysków** (BitLocker, LUKS) wraz z **TPM**, które chroni dane przy kradzieży. Przed atakiem typu Evil Maid (podmiana bootloadera) chroni Secure Boot,
- **kontrola dostępu do sieci:** **802.1X/NAC** przepuszcza tylko uwierzytelnione urządzenia, a **port security** ogranicza adresy MAC na portach. Nieużywane porty wyłączamy.

**Zasoby programowe i dane:**

- **uwierzytelnianie:** silne hasła i klucze (SSH), **MFA** (wiem, mam, jestem), tokeny, certyfikaty, biometria, SSO z Kerberosem. Blokada konta po nieudanych próbach i narzędzia jak **fail2ban**,
- **autoryzacja:** ACL, **RBAC** i DAC/MAC (np. SELinux), prawa do plików, zasada najmniejszych uprawnień, rozdzielenie ról. Konta administracyjne tylko do zadań administracyjnych,
- **wzmacnianie systemów (hardening):** usunięcie zbędnych usług, zmiana domyślnych haseł, aktualizacje, zapora hostowa,
- **kontrola aplikacji i danych:** AppLocker, białe listy, szyfrowanie danych, zabezpieczenie API.

**Ochrona sieciowa:** **zapory i segmentacja** (VLAN, DMZ), **VPN** z MFA dla dostępu zdalnego, **IDS/IPS**.

**Monitoring i rozliczalność:** logi (np. zdarzenia Windows 4624/4625, auditd w Linuksie), alerty i **SIEM**. Pozwalają wykryć próby nieuprawnionego dostępu i prześledzić incydent.

**Czynnik ludzki:** szkolenia (phishing, hasła), polityki dostępu i procedury odbierania uprawnień po odejściu pracownika.

**Wniosek:** wystarczy złamać jedną warstwę, więc łączy się ochronę fizyczną, kontrolę dostępu do sieci, silne uwierzytelnianie, autoryzację i monitoring.

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