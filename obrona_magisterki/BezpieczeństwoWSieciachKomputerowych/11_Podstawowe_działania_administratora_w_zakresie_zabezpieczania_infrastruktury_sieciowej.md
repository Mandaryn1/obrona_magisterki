# Podstawowe działania administratora w zakresie zabezpieczania infrastruktury sieciowej

> Opracowanie oparte na wykładach W8 (utwardzanie systemów, aktualizacje, MFA, audyt), W1/W7 (obrona w głąb, segmentacja, kopie), MBK1 (logi, incydenty) i *Zapory/IDS*; uzupełnienia – ***(uzupełnienie)***.

## Ujęcie procesowe

Zabezpieczanie infrastruktury to **proces ciągły**, a nie jednorazowa czynność (cykl PDCA: Plan – Do – Check – Act; wykład W8, slajd 30: audyt jako narzędzie ciągłego doskonalenia). Zadania administratora układają się w następujące obszary.

## 1. Poznanie i dokumentacja infrastruktury

- **Inwentaryzacja** urządzeń, systemów, usług, wersji, właścicieli; mapa topologii i przepływów,
- **klasyfikacja zasobów i danych** (publiczne, wewnętrzne, poufne, ściśle tajne – wykład, W1, slajdy 31–32),
- **zarządzanie konfiguracją i zmianami** (kopie konfiguracji, kontrola zmian, wersjonowanie).

## 2. Utwardzanie (hardening) systemów i urządzeń

Wykład W8: **minimalizacja powierzchni ataku** – usunięcie zbędnych usług, aplikacji i protokołów, zmiana domyślnych konfiguracji.

| Działanie | Windows | Linux | Urządzenia sieciowe |
| :--- | :--- | :--- | :--- |
| **Usunięcie zbędnych usług/portów** | wyłączenie SMBv1, zbędnych ról | minimalne pakiety i usługi (`ss -tulpn`) | wyłączenie Telnet/HTTP, CDP na portach brzegowych |
| **Domyślne hasła i konta** | zmiana, wyłączenie kont gości | wyłączenie logowania root, `sudo` | zmiana haseł fabrycznych, usunięcie kont domyślnych |
| **Biała lista aplikacji / MAC** | **AppLocker**, Defender | **SELinux/AppArmor** | listy ACL zarządzania |
| **Zapora hostowa** | Defender Firewall (GPO) | iptables/nftables, firewalld | ACL na interfejsach |
| **Szyfrowanie dysku** | BitLocker (+TPM) | LUKS | – |
| **Standardy** | **CIS Benchmarks**, DISA STIG, **OpenSCAP** (zgodność i audyt) – wykład W8, slajd 15 | | CIS dla Cisco/Juniper |

### Bezpieczna konfiguracja urządzeń sieciowych *(uzupełnienie)*

```text
! Zarządzanie tylko przez SSH, z ACL, z uwierzytelnianiem scentralizowanym
enable secret <silne_haslo>
service password-encryption
username admin privilege 15 secret <haslo>
aaa new-model
aaa authentication login default group tacacs+ local
ip domain-name firma.local
crypto key generate rsa modulus 2048
ip ssh version 2
line vty 0 4
 transport input ssh
 access-class MGMT-ACL in
 exec-timeout 10 0
no ip http server
ip http secure-server
no cdp run                          ! lub tylko na portach wewnętrznych
banner login ^Dostęp tylko dla upoważnionych^
logging host 10.0.99.10
ntp server 10.0.99.5
snmp-server group GRP v3 priv
```

Dodatkowo: **osobna sieć/VLAN zarządzania (out-of-band)**, konta imienne, RADIUS/TACACS+ (AAA), wyłączenie nieużywanych portów, kopie konfiguracji.

## 3. Aktualizacje i zarządzanie podatnościami

- **Regularne łatanie** systemów, aplikacji, firmware urządzeń (wykład W8, slajdy 11–12, 17–18); **WannaCry** – lekcja: zarządzanie poprawkami (wykład, slajd 35),
- środowisko testowe przed wdrożeniem, harmonogramy, priorytet dla podatności krytycznych (CVSS, aktywne exploity),
- **skanowanie podatności** (Nessus, OpenVAS) i testy penetracyjne, śledzenie biuletynów (CVE).

## 4. Ochrona perymetru i segmentacja

- **Zapory** (stanowe, NGFW), polityka **default deny**, reguły oparte na zasadzie najmniejszych uprawnień, regularny przegląd reguł; **NAT**; **DMZ**,
- **segmentacja** (VLAN, strefy, mikrosegmentacja), izolacja gości, IoT/OT, strefy zarządzania (wykład W7, slajd 18),
- **VPN** dla dostępu zdalnego i połączeń między oddziałami (IPsec, OpenVPN, WireGuard), **UTM** dla małych firm (wykład, slajdy 24, 32),
- **zabezpieczenia L2/L3** (port security, DHCP snooping, DAI, BPDU Guard, uRPF, ACL – tematy 5 i 6).

## 5. Kontrola dostępu i tożsamości

- **Zasada najmniejszych uprawnień**, regularne przeglądy uprawnień, usuwanie kont po odejściu pracownika,
- **silne polityki haseł** (długość, złożoność, rotacja po incydencie, brak powtarzania – wykład W8, slajd 19) i **MFA** (zwłaszcza administratorzy, VPN, zdalny dostęp – wykład W8, slajdy 20–21),
- **ochrona przed brute force:** Fail2Ban, Account Lockout Policy, rate limiting, CAPTCHA (wykład W8, slajd 32),
- **IAM/SSO/RBAC**, oddzielne konta administracyjne i użytkownika (PAM), **802.1X/NAC**.

## 6. Szyfrowanie i bezpieczna komunikacja

- **TLS** dla usług (certyfikaty zarządzane centralnie i odnawiane; wyłączenie SSLv3/TLS 1.0/1.1), **SSH** zamiast Telnet, **SFTP** zamiast FTP, **SNMPv3**, **WPA2/WPA3**,
- szyfrowanie dysków i kopii zapasowych; zarządzanie kluczami.

## 7. Monitorowanie, logowanie i wykrywanie

- centralne logowanie (**syslog**, Windows Event Forwarding) z **synchronizacją czasu (NTP)** i **SIEM** (wykład, MBK1, slajd 49),
- **IDS/IPS** (Snort/Suricata, HIDS: OSSEC/Wazuh), NetFlow, alerty i progi (wykład, *Zapory/IDS*),
- **audyt** zdarzeń: logowania 4624/4625, zmiany uprawnień, dostęp do plików; `auditd`,
- przegląd logów i raportów, archiwizacja i ochrona logów przed modyfikacją.

## 8. Kopie zapasowe i ciągłość działania

- **kopie 3-2-1**, **offline/air-gapped** (lekcja NotPetya/WannaCry – wykład, slajd 35), szyfrowane i testowane odtwarzanie (wykład W7, slajd 10),
- redundancja kluczowych urządzeń i łączy, plany DR (**RPO/RTO**), zasilanie awaryjne.

## 9. Reagowanie na incydenty (wykład, MBK1, slajd 50)

- **plan reakcji** i procedury (playbooki), role i kontakty, rejestr incydentów,
- **automatyczne powiadamianie** i blokady (np. IP po wielu nieudanych logowaniach),
- **izolacja hosta/segmentu**, zabezpieczenie dowodów (logi, obrazy dysków, pcap), **post-mortem** i aktualizacja polityk,
- współpraca z SOC/CSIRT.

## 10. Ochrona stacji końcowych i malware

- **antywirus/EDR** z aktualizacjami, sandboxing, kontrola makr i załączników, filtracja poczty (**SPF, DKIM, DMARC**), filtrowanie DNS/WWW (wykład, slajdy 20, 34),
- kontrola urządzeń wymiennych i USB.

## 11. Aspekty fizyczne

zamknięte szafy, kontrola dostępu do serwerowni, monitoring, zasilanie i klimatyzacja, ochrona okablowania, nieużywane gniazda wyłączone.

## 12. Ludzie, polityki i zgodność

- **szkolenia i świadomość** (phishing, socjotechnika – wykład, W1, slajd 55; „*szkolenia dla wszystkich użytkowników*" – wykład, slajd 34),
- **polityki bezpieczeństwa** i procedury, **audyty** (wewnętrzne i zewnętrzne), zgodność (ISO 27001, RODO, NIS2), **zarządzanie ryzykiem** (identyfikacja, ocena, postępowanie: akceptacja/unikanie/mitygacja/transfer, monitorowanie – wykład, W1, slajd 52).

## Lista kontrolna administratora (skrót)

| Obszar | Pytanie kontrolne |
| :--- | :--- |
| Inwentaryzacja | czy znam wszystkie urządzenia i usługi? |
| Konfiguracja | czy usunięto zbędne usługi i domyślne hasła? |
| Aktualizacje | czy łatki krytyczne wdrożono w terminie? |
| Dostęp | czy MFA, minimalne uprawnienia, zarządzanie przez SSH/AAA? |
| Sieć | czy działa segmentacja i zapory default deny? |
| Monitoring | czy logi trafiają do SIEM i są analizowane? |
| Kopie | czy kopie są offline i testowane? |
| Incydenty | czy istnieje i jest ćwiczony plan reakcji? |
| Ludzie | czy użytkownicy są szkoleni? |

## Podsumowanie

- Administrator: **inwentaryzuje, utwardza, łata, segmentuje, kontroluje dostęp, szyfruje, monitoruje, wykonuje kopie, reaguje na incydenty i szkoli** – w ramach ciągłego cyklu PDCA.
- Podstawa: **najmniejsze uprawnienia, domyślna odmowa, minimalna powierzchnia ataku, obrona w głąb** oraz audyt i doskonalenie.
