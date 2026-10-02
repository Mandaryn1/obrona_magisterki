# Zapobieganie i wykrywanie zagrożeń w systemach operacyjnych

> Wykład: W1 (slajdy 74–86: netstat, Podgląd zdarzeń, aktualizacje, zasady, Defender, zapora), W2 (slajdy 28–31, 45–50, 53: logi, aktualizacje, malware, rootkity, UFW), W8 (utwardzanie, AppLocker, OpenSCAP, brute force, sandbox), MBK1 (TPM, Secure Boot, audyt). Narzędzia wykrywania (auditd, FIM, EDR, Sysmon) – ***(uzupełnienie)***.

## Podejście: prewencja – detekcja – reakcja

Żadne zabezpieczenie nie jest idealne, więc OS chroni się wielowarstwowo: **zapobieganie** (zmniejszanie powierzchni i prawdopodobieństwa ataku), **wykrywanie** (wiedza, że coś się dzieje), **reagowanie** (temat 11). Wykład (MBK1, slajd 50): *kluczem jest szybkie wykrycie, odpowiednia reakcja i wyciągnięcie wniosków.*

# A. ZAPOBIEGANIE

## 1. Utwardzanie systemu (hardening) – wykład W8

| Obszar | Windows (W8, slajdy 8–12, 25) | Linux (W8, slajdy 13–16, 26) |
| :--- | :--- | :--- |
| minimalizacja | usunięcie zbędnych usług, protokołów, ról | **instalacja tylko niezbędnych pakietów**, minimum usług |
| kontrola aplikacji | **AppLocker** – biała lista (wydawca, ścieżka, hash, wersja; tryb raportowania; GPO) | **SELinux/AppArmor** (MAC) |
| uprawnienia | UAC, brak pracy na administratorze, GPO | **`sudo`**, wyłączony root, minimalne prawa plików |
| ochrona przed malware | **Windows Defender** (ochrona w czasie rzeczywistym) | skanery, utwardzone jądro (ASLR, PaX), `rkhunter` |
| zapora | **Zapora Windows Defender** | iptables/nftables, **UFW**, firewalld |
| aktualizacje | Windows Update, WSUS/SCCM | `apt-get update/upgrade`, `unattended-upgrades`, Livepatch |
| audyt/zgodność | Menedżer zgodności, GPO | **OpenSCAP** (CIS, DISA STIG), `lynis` |
| zarządzanie | GPO, Intune | Ansible, Puppet, Chef (konfiguracja jako kod) |

Standardy: **CIS Benchmarks**, DISA STIG, ISO 27001 (OpenSCAP automatyzuje skanowanie i poprawki – wykład W8, slajd 16). Wspólne wyzwania (slajd 27): **zarządzanie poprawkami, zasada najmniejszych uprawnień, edukacja użytkowników, ciągły monitoring.**

## 2. Aktualizacje i zarządzanie poprawkami

- **Łatki** to aktualizacje kodu usuwające nowo wykryte podatności (wykład W1, slajd 78); Windows Update regularnie pobiera aktualizacje zabezpieczeń, krytyczne i service packi (slajd 79); Linux – menedżery pakietów (`apt-get update` pobiera listę, `upgrade` aktualizuje – W2 slajdy 45–47); nieplanowane aktualizacje przy poważnych lukach,
- proces: **identyfikacja → testy → wdrożenie → weryfikacja**; priorytet dla podatności aktywnie wykorzystywanych; **WannaCry** i **EternalBlue** – skutek braku łatki,
- aktualizacje firmware/UEFI, sterowników i aplikacji (przeglądarki, Java).

## 3. Kontrola dostępu i najmniejsze uprawnienia

Zob. tematy 4–5: konta standardowe, UAC, `sudo`, RBAC, **blokada kont po nieudanych próbach** (Account Lockout Policy – W1 slajd 82), **Fail2Ban** (W8, slajd 32), rate limiting, MFA (temat 6).

## 4. Zapora hostowa

Zapora selektywnie blokuje ruch: **otwarte tylko wymagane porty, wszystko inne odrzucane** (wykład W1, slajd 85); Windows: *Zezwalaj aplikacji lub funkcji na dostęp przez zaporę*, ustawienia zaawansowane, profile domenowy/prywatny/publiczny (GPO). Linux: **UFW** – `ufw default deny incoming; ufw allow 22/tcp; ufw deny 23` (laboratorium W2: blokada Telnet, wyłączenie usługi), `iptables`/`nftables`.

## 5. Ochrona przed malware

**Typy** (W1 slajd 83): wirusy, robaki, trojany, keyloggery, spyware, adware, **ransomware**, **rootkity**, fileless. **Ochrona antywirusowa** (stały monitoring, kwarantanna), **przed adware**, **antyphishingowa** (blokowanie IP znanych stron), **antyspyware**; **Windows Defender** wbudowany i domyślnie włączony (slajd 84); rozwiązania komercyjne (McAfee, Symantec, Kaspersky).

**Linux a malware (W2, slajd 49):** struktura systemu plików, uprawnienia i ograniczenia kont dają lepszą ochronę, **ale Linux nie jest odporny** – wykryto i wykorzystano wiele luk; poprawki bywają dostępne w ciągu godzin; *typowym wektorem ataku są jego usługi*; **uruchomiony złośliwy program wyrządza szkodę niezależnie od platformy.**

## 6. Zaufany rozruch i szyfrowanie

**Secure Boot + TPM** (pomiary w PCR, odmowa wydania klucza dla zmodyfikowanego systemu – MBK1, slajdy 41–47), **BitLocker/LUKS**, ochrona przed **Evil Maid** (temat 8).

## 7. Izolacja i piaskownice

kontenery, maszyny wirtualne, **AppContainer/Windows Sandbox**, seccomp, chroot; **środowisko testowe do analizy malware** (wykład W8, slajd 31): izolowana sieć, maszyny wirtualne, pełny monitoring aktywności (wywołania systemowe, ruch, zmiany plików), **analiza behawioralna i identyfikacja IoC**.

## 8. Kopie zapasowe i odtwarzanie

Punkty przywracania i kopie (W2, laboratorium slajd 53); **3-2-1**, kopie offline/niezmienne (ransomware), testy odtwarzania (RPO/RTO).

## 9. Człowiek

Edukacja użytkowników (phishing, hasła) – wykład W8, slajd 28.

# B. WYKRYWANIE

## 1. Logi i audyt

| System | Źródła | Co obserwować |
| :--- | :--- | :--- |
| **Windows** (W1 slajdy 76–77; MBK1 slajd 49) | **Podgląd zdarzeń (Event Viewer)** – dzienniki systemu Windows (**Security**, System, Application) oraz dzienniki aplikacji i usług; zdarzenie ma poziom (informacja, ostrzeżenie, błąd, krytyczny), datę, źródło i **Event ID** | **4624** udane logowanie, **4625** nieudane, **4670** zmiana uprawnień, **4663** dostęp do pliku (wymaga SACL), 4688 start procesu, 7045 nowa usługa, 1102 wyczyszczenie logu |
| **Linux** (W2 slajdy 29–31) | `/var/log/messages`, **`/var/log/auth.log`** (Debian/Ubuntu) lub **`/var/log/secure`** (RHEL/CentOS: sudo, SSH, SSSD), `/var/log/kern.log`, `boot.log`, `cron`, logi usług (`mysqld.log`); **`auditd`** (`auditctl -w /etc/passwd -p wa -k passwd_changes`), `journalctl` | nieudane logowania, użycie sudo, zmiany krytycznych plików, start usług |

Zasady: **centralizacja i synchronizacja czasu**, ochrona logów przed modyfikacją, retencja, **SIEM** (korelacja), alerty (wiele nieudanych logowań z jednego IP → blokada i powiadomienie – MBK1 slajd 50).

## 2. Monitorowanie procesów, usług i połączeń

- **Windows:** Menedżer zadań (procesy, usługi, użytkownicy, wydajność), **Monitor zasobów**, **Process Explorer**/RAMMap (Sysinternals), **`netstat`** (W1 slajdy 74–75: złośliwe oprogramowanie często **otwiera porty komunikacyjne**; `netstat` pokazuje aktywne połączenia TCP i procesy nasłuchujące; powiązanie z **PID** w Menedżerze zadań, zamknięcie i oczyszczenie), `Get-NetTCPConnection`, `tasklist`, `sc query`, autostart (`msconfig`, rejestr `Run`),
- **Linux:** `ps`, `top`, `ss -tulpn`/`netstat`, `lsof -i`, `last`/`lastb`, `w`, `crontab -l` (utrwalanie), pliki `.bashrc`, `systemctl list-units`; **pipowanie** poleceń (`ls -l | grep host`, `grep "Failed password" /var/log/auth.log | …` – W2 slajd 51).

## 3. Kontrola integralności i wykrywanie rootkitów

- **FIM** (File Integrity Monitoring): **AIDE, Tripwire, OSSEC/Wazuh**, `debsums`, `rpm -V`; `sfc /scannow` (Windows); **IMA/EVM** (Linux), **Measured Boot/TPM**,
- **rootkity (W2, slajd 50):** metody **behawioralne, sygnaturowe, porównanie różnic, analiza zrzutu pamięci**; narzędzia `chkrootkit`, `rkhunter`, Volatility; reinstalacja przy jądrze zainfekowanym,
- *(uzup.)* porównanie list procesów/plików z poziomu systemu i z zewnątrz (live USB).

## 4. HIDS/HIPS, EDR/XDR, antywirus

**HIDS** (OSSEC/Wazuh, Sysmon + SIEM) – logi, integralność, procesy, rejestr; **EDR** (Defender for Endpoint, CrowdStrike…) – telemetria i detekcja behawioralna, izolacja hosta; **YARA** (reguły wykrywania malware); **Falco** (kontenery/Linux).

## 5. Wykrywanie ataków brute force i anomalii

Reguły: wiele zdarzeń 4625/`Failed password` z tego samego źródła; logowania poza godzinami; nowe konta administratorów; nowe usługi/zaplanowane zadania; duże transfery; skanowanie portów (IDS). Wykład W8: **Fail2Ban**, **lockout**, **rate limiting**, **CAPTCHA**; AI w bezpieczeństwie systemów (slajd 34).

## 6. Audyt bezpieczeństwa (W8, slajdy 22–24, 30)

Audyt jest narzędziem **ciągłego doskonalenia**: **narzędzia audytu systemów** (Windows – Microsoft Baseline Security Analyzer/Compliance Manager; Linux – OpenSCAP, Lynis), przeglądy konfiguracji względem benchmarków, skanery podatności (Nessus, OpenVAS – temat 12).

## Przykładowe polecenia *(uzupełnienie)*

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security';Id=4625} -MaxEvents 20
netstat -ano | findstr LISTENING ; Get-Process -Id <PID>
```
```bash
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr
ss -tulpn ; last -a | head ; auditctl -l ; aide --check
```

## Podsumowanie

- **Zapobieganie:** utwardzanie (minimalizacja usług, najmniejsze uprawnienia, AppLocker/SELinux), **aktualizacje**, zapora, AV/Defender, Secure Boot+TPM, szyfrowanie, kopie zapasowe, MFA i blokady brute force, piaskownice, edukacja.
- **Wykrywanie:** **logi i audyt** (Event Log 4624/4625/4670/4663; `auth.log`, auditd), monitorowanie procesów i połączeń (`netstat`, `ps`, `ss`), **kontrola integralności**, wykrywanie rootkitów, HIDS/EDR/SIEM, reguły anomalii.
- Całość uzupełnia **reagowanie na incydenty** (temat 11) i **testy bezpieczeństwa** (temat 12).

---
[⬅️ Poprzedni temat](6_Mechanizmy_uwierzytelniania_hasła_klucze_SSH_2FA_IAM_i_SSO.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Zastosowanie_kryptografii_w_ochronie_danych_i_komunikacji.md)