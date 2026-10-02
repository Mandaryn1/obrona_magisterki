# Architektura systemu operacyjnego z punktu widzenia bezpieczeństwa

> Wykład: W1 (slajdy 16–43: HAL, tryby, systemy plików, rozruch, procesy, pamięć, rejestr), W2 (podstawy Linuksa, pliki, serwery), MBK1 (TPM, Secure Boot), W8 (jądro Linux). Szczegóły architektury bezpieczeństwa – ***(uzupełnienie)***.

## Warstwy systemu i granice zaufania

```
 ┌────────────────────────────── aplikacje użytkownika ──────────────────────────────┐  tryb użytkownika (ring 3)
 │   przeglądarka, edytor, powłoka (CLI/GUI), usługi i demony (niższy poziom zaufania) │
 ├────────────────────────── usługi systemowe i biblioteki systemu ───────────────────┤
 ╞═══════════════════════ granica: wywołania systemowe (API/syscall) ═════════════════╡
 │ JĄDRO (kernel): procesy, pamięć, system plików, sieć, IPC, sterowniki, kontrola dostępu │  tryb jądra (ring 0)
 ├──────────────────────── HAL / warstwa abstrakcji sprzętu ───────────────────────────┤
 │ firmware (BIOS/UEFI), bootloader, TPM, sprzęt (CPU, MMU, pamięć, urządzenia)          │
 └────────────────────────────────────────────────────────────────────────────────────┘
```

**Trusted Computing Base (TCB)** – zbiór komponentów (sprzęt, firmware, jądro, mechanizmy bezpieczeństwa), których poprawne działanie jest konieczne dla bezpieczeństwa całości; im mniejszy i prostszy, tym łatwiejszy do weryfikacji. **Reference monitor** (monitor odwołań) – koncepcja: komponent, który **pośredniczy w każdym dostępie** podmiotu do obiektu, jest **niemożliwy do obejścia, odporny na manipulacje i na tyle mały, by dało się go zweryfikować**.

## Architektura Windows (wykład W1)

### HAL i jądro (slajdy 17–18)

**HAL (Hardware Abstraction Layer)** obsługuje całą komunikację między sprzętem a jądrem, izolując system od różnic sprzętowych. **Jądro** to rdzeń systemu kontrolujący cały komputer: obsługuje żądania wejścia/wyjścia, pamięć i urządzenia peryferyjne.

### Tryb użytkownika i tryb jądra (slajdy 19–20)

- aplikacje działają w **trybie użytkownika**, kod systemu operacyjnego w **trybie jądra**,
- kod trybu jądra ma **nieograniczony dostęp do sprzętu**, może wykonywać dowolne instrukcje procesora i odwoływać się do dowolnego adresu pamięci; **współdzieli jedną przestrzeń adresową** i **nie jest odizolowany** od systemu operacyjnego,
- kod w trybie użytkownika **nie ma bezpośredniego dostępu do sprzętu** – korzysta z wywołań systemowych.

***(uzupełnienie)*** Elementy bezpieczeństwa w jądrze Windows (NT Executive): **Security Reference Monitor (SRM)** – sprawdza prawa dostępu (porównuje **access token** procesu z **DACL** obiektu), **Object Manager**, **LSASS** (Local Security Authority – uwierzytelnianie, polityki, tokeny; proces `lsass.exe`), **Winlogon**, **SAM** (lokalna baza kont), **Kerberos/NTLM**, **UAC** i poziomy integralności (MIC), **Windows Defender**, **BitLocker**, **VBS/HVCI**, **Credential Guard** (izolacja sekretów w trybie wirtualizacji).

### Systemy plików Windows (slajdy 21–24)

- **FAT/exFAT** – proste, bez zabezpieczeń (brak uprawnień),
- **NTFS** – najczęściej używany: bardzo duże pliki i partycje, niezawodność i odzyskiwanie, **wiele funkcji bezpieczeństwa (ACL, EFS, kwoty, dziennik)**; struktury: sektor rozruchowy, **MFT** (Master File Table – atrybuty, w tym informacje o bezpieczeństwie), dziennik,
- **Alternatywne strumienie danych (ADS)** – plik jako zestaw atrybutów; dane w atrybucie `$DATA`; ADS adresowany `Plik.txt:ADS` – **może służyć do ukrywania danych/malware** (zagadnienie bezpieczeństwa).

### Proces uruchamiania (slajdy 25–35)

**BIOS** (od lat 80., ograniczenia) vs **UEFI** (następca). BIOS: inicjalizacja urządzeń, **POST**, wyszukanie **MBR**; następnie `Bootmgr.exe` → **BCD** (Boot Configuration Data) → `winload.exe` → jądro (ntoskrnl), HAL, sterowniki, usługi, logowanie. **Autostart:** klucze rejestru `HKLM` (usługi przy każdym uruchomieniu) i `HKCU` (przy logowaniu użytkownika); `msconfig` (karty: Ogólne, Rozruch, Usługi, Uruchamianie, Narzędzia) – autostart jest **typowym miejscem utrwalania malware**.

***(uzupełnienie)*** **Secure Boot** (UEFI sprawdza podpisy bootloadera i jądra), **Measured Boot** + **TPM** (pomiary w rejestrach PCR), **ELAM** (Early Launch Anti-Malware), podpisywanie sterowników.

### Procesy, wątki, usługi, pamięć, rejestr (slajdy 36–43)

- **proces** – wykonywany program (aplikacja może mieć wiele procesów); co najmniej jeden **wątek**; wątki jednego procesu **dzielą przestrzeń adresową** (nie szkodzą innym procesom),
- **usługi** – procesy działające długotrwale w tle (łączność bezprzewodowa, serwer FTP) – często z wysokimi uprawnieniami,
- **pamięć wirtualna** – każdy proces ma własną wirtualną przestrzeń adresową (wykład: 4 GB w systemie 32-bitowym; w 64-bitowym wykład podaje 8 TB – ***uwaga:*** w nowszych wersjach Windows jest to 128 TB); narzędzie **RAMMap**,
- **rejestr** – hierarchiczna baza (gałęzie `HKLM`, `HKCU`…, klucze, podklucze, wartości `REG_SZ`, `REG_DWORD`, `REG_BINARY`); nowych gałęzi nie można tworzyć; edycja `regedit` (tylko administrator, drobne zmiany mogą mieć katastrofalne skutki) – cel malware (utrwalenie) i narzędzie utwardzania.

## Architektura Linux (wykład W2)

- **Linux** (1991) – otwarty (open source), szybki, lekki, dostosowywalny, zaprojektowany do pracy w sieci; **dystrybucja** = jądro + narzędzia i pakiety; wybierany w **SOC** (Security Onion, Kali Linux, Wireshark, IDS, zapory) – W2 slajdy 4–8,
- użytkownik komunikuje się przez **CLI** (powłoka) lub **GUI** (X Window, GNOME/Unity – slajdy 42–43; ***uwaga:*** serwer X umożliwia zdalne wyświetlanie – dodatkowa powierzchnia ataku),
- **„w Linuksie wszystko jest plikiem"** (pamięć, dyski, monitor, katalogi) i konfiguracja opiera się na plikach (`/etc/...`), więc **prawa dostępu do plików są kluczowym mechanizmem** (slajd 15),
- **procesy i fork** – nowy proces powstaje przez `fork` (rodzic/dziecko, różne PID, ten sam kod); polecenia `ps`, `top`, `kill`,
- **systemy plików:** ext2/3/4, NFS, swap, CDFS, HFS+; **montowanie** katalogu do partycji; root `/` na `ext4` (`/dev/sda1`) – slajdy 34–35,
- **pakiety** (`apt`, `dpkg`, `pacman`) – aktualizacje i instalacja,
- **logi** w `/var/log/` (`messages`, `auth.log`/`secure`, `kern.log`, `boot.log`, `cron`, `mysqld.log` – slajdy 29–31).

***(uzupełnienie)*** Jądro Linuksa jest **monolityczne z modułami** (LKM); mechanizmy bezpieczeństwa: **LSM** (framework – SELinux, AppArmor), **capabilities**, **namespaces/cgroups/seccomp** (kontenery), **PAM**, **netfilter**, **IMA/EVM**, **auditd**.

## Modele jądra – porównanie *(uzupełnienie)*

| Model | Cechy | Wpływ na bezpieczeństwo | Przykłady |
| :--- | :--- | :--- | :--- |
| **monolityczny** | całość usług w jądrze (wspólna przestrzeń) | błąd sterownika = kompromitacja jądra; duża powierzchnia | Linux, klasyczny Unix |
| **mikrojądro** | minimalne jądro, usługi w przestrzeni użytkownika | mniejszy TCB, lepsza izolacja; wolniejsze IPC | Minix, seL4, QNX |
| **hybrydowe** | kompromis | część usług w jądrze | Windows NT, macOS (XNU) |

## Windows vs Linux – bezpieczeństwo architektury

| Aspekt | Windows | Linux |
| :--- | :--- | :--- |
| Tożsamość i dostęp | **SID, token dostępu, SRM, DACL/SACL** | **UID/GID, bity rwx, POSIX ACL, capabilities** |
| Zarządzanie centralne | **Active Directory, GPO** | LDAP/Kerberos/SSSD, Ansible/Puppet |
| Konfiguracja | rejestr, GPO, usługi | pliki tekstowe `/etc` |
| MAC | integrity levels (MIC), AppLocker | **SELinux, AppArmor** |
| Uprzywilejowany użytkownik | Administrator/SYSTEM (UAC) | root (sudo) |
| Szyfrowanie dysku | BitLocker (+TPM) | LUKS/dm-crypt |
| Rozruch | Secure Boot + Measured Boot | Secure Boot (shim/GRUB), IMA |
| Logi | Event Log (Security: 4624/4625…) | syslog/journald/auditd |

## Podsumowanie

- Bezpieczny OS opiera się na **izolacji trybu jądra i użytkownika**, **monitorze odwołań** pośredniczącym w dostępie, **procesach z własnymi przestrzeniami adresowymi**, **systemie plików z uprawnieniami**, **zaufanym rozruchu (UEFI Secure Boot + TPM)** oraz małym TCB.
- Windows: HAL, jądro, NT Executive (SRM, LSASS), NTFS (ACL, ADS), rejestr, usługi, AD/GPO; Linux: monolityczne jądro z LSM, „wszystko jest plikiem", prawa plików, PAM, SELinux/AppArmor, logi w `/var/log`.
- Każdy komponent (autostart, usługi, sterowniki, ADS, rejestr) to także potencjalny element **utrwalania zagrożeń**.

---
[⬅️ Poprzedni temat](1_Podstawowe_cele_bezpieczeństwa_systemów_operacyjnych_i_usług_sieciowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](3_Jądro_systemu_separacja_przestrzeni_użytkownika_i_ochrona_pamięci.md)