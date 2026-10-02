# Rola systemów Windows i Linux w analizie bezpieczeństwa sieci komputerowej

## Dwie role systemów operacyjnych

1. **System jako element chronionej infrastruktury** – serwery, stacje robocze, routery/zapory programowe; ich konfiguracja, usługi i otwarte porty współtworzą **powierzchnię ataku**; muszą być **utwardzone** (wykład W8: usuwanie zbędnych usług, domyślne konfiguracje → utwardzone ustawienia, minimalna powierzchnia ataku).
2. **System jako platforma i źródło danych do analizy** – na nim uruchamia się narzędzia (skanery, sniffery, IDS), a on sam generuje **logi i telemetrię**, bez których nie da się wykryć ani zbadać incydentu.

## Linux

**Zastosowania w analizie bezpieczeństwa:**

- platforma narzędzi: **Kali Linux, Parrot, Security Onion, REMnux**; **Wireshark, tcpdump, nmap, Snort/Suricata, Zeek** (wykład: Zapory/IDS, slajdy 26, 33),
- **zapora jądra:** **iptables / nftables** (stanowa filtracja przez *conntrack*, NAT), firewalld/ufw (wykład, slajd 25),
- bramy, routery, VPN (WireGuard, OpenVPN, strongSwan), serwery DNS/DHCP/proxy, kontenery i chmura,
- narzędzia „w terminalu": skryptowanie (bash, Python), potoki (`grep | awk | sort`), automatyzacja,
- logi: `/var/log/auth.log` (Debian) lub `/var/log/secure` (RHEL) – logowania, sudo, SSH; **auditd** (audyt na poziomie jądra: `auditctl -w /etc/passwd -p wa -k passwd_changes`); `journalctl`, `rsyslog` (wykład, MBK1, slajd 49),
- mechanizmy ochronne: **SELinux/AppArmor** (MAC), `sudo`, **fail2ban** (blokada IP po nieudanych logowaniach, integracja z iptables), OpenSCAP (zgodność z CIS/STIG) – wykład W8.

**Typowe polecenia diagnostyczne** *(uzupełnienie)*:

```bash
ip a; ip r; ip neigh          # interfejsy, trasy, tablica ARP/ND
ss -tulpn                     # nasłuchujące porty i procesy
tcpdump -i eth0 -nn 'tcp port 22 or icmp'   # przechwytywanie
nmap -sS -sV 10.0.0.0/24      # inwentaryzacja usług
journalctl -u sshd --since "1 hour ago"
iptables -L -n -v ; nft list ruleset
```

## Windows

**Zastosowania:**

- dominujący system **stacji roboczych i serwerów korporacyjnych** (Active Directory, Kerberos/NTLM, SMB, RDP, GPO) – główny cel i ogniwo ataków (np. **WannaCry/EternalBlue** wykorzystujący **SMBv1** – wykład, slajd 35),
- narzędzia: **Wireshark (Npcap)**, **Sysmon** (Sysinternals), **PowerShell**, Windows Defender, **Windows Defender Firewall** (filtracja stanowa, profile sieci: domenowy/prywatny/publiczny, zarządzanie przez GPO), **AppLocker** (biała lista aplikacji) – wykład W8,
- logi: **Dziennik zdarzeń Security** (`eventvwr.msc`): **4624** (udane logowanie), **4625** (nieudane), **4670** (zmiana uprawnień), **4663** (dostęp do pliku) – wykład, MBK1, slajd 49; typy logowania (2 – interaktywne, 3 – sieciowe, 10 – RDP),
- polityki: **Account Lockout Policy** (blokada konta po N nieudanych próbach – ochrona przed brute force), **Credential Guard/VBS**, BitLocker + TPM,
- **GPO** i Active Directory do centralnego egzekwowania zasad bezpieczeństwa.

**Typowe polecenia** *(uzupełnienie)*:

```powershell
netstat -ano                                  # połączenia z PID
Get-NetTCPConnection -State Listen            # nasłuchujące porty
Get-EventLog -LogName Security -InstanceId 4625 -Newest 20
arp -a ; ipconfig /all ; nslookup domena
netsh advfirewall show allprofiles
```

## Porównanie

| Aspekt | **Windows** | **Linux** |
| :--- | :--- | :--- |
| Rola w sieci | stacje, serwery AD/pliki/aplikacje, klienci | serwery, bramy/zapory, urządzenia sieciowe, chmura, IoT, platformy analityczne |
| Zapora | Windows Defender Firewall (GPO) | iptables/nftables, firewalld, ufw |
| Logowanie zdarzeń | Event Log (ID zdarzeń), Sysmon | syslog, journald, auditd |
| Kontrola dostępu | ACL (NTFS), RBAC, GPO, AD | prawa Unix, ACL, **SELinux/AppArmor**, sudo |
| Narzędzia analityczne | Wireshark, Sysinternals, PowerShell | tcpdump, nmap, Zeek, Suricata, skrypty |
| Typowe wektory | SMB, RDP, NTLM relay, pass-the-hash, phishing/makra | SSH brute force, błędna konfiguracja usług, podatne aplikacje webowe, wyciek kluczy |
| Utwardzanie | AppLocker, Defender, aktualizacje, GPO | minimalizacja pakietów, SELinux, sudo, fail2ban, OpenSCAP |
| Uwierzytelnianie sieciowe | Kerberos (SSO), NTLM | PAM, Kerberos (SSSD), klucze SSH |

## Dlaczego oba są potrzebne w analizie

- **Heterogeniczne środowiska** – analityk musi czytać logi i ruch obu światów (np. Windows jako cel, Linux jako sensor).
- **Korelacja zdarzeń**: logowanie (4624/4625 w Windows, `auth.log` w Linuksie) + ruch z NIDS + logi zapory → pełny obraz incydentu w **SIEM** (wykład).
- **Weryfikacja ruchu na hoście**: czy proces (PID) odpowiada za podejrzane połączenie (`ss -p`, `netstat -ano`).
- **Powierzchnia ataku hosta** wpływa na bezpieczeństwo całej sieci (przejęty host to punkt ruchu bocznego).
- **Agenci HIDS/HIPS** (OSSEC/Wazuh, Sysmon + EDR) działają na obu platformach i uzupełniają NIDS.

## Podsumowanie

- Systemy Windows i Linux są jednocześnie **chronionymi zasobami** (utwardzanie, łatanie) i **platformami/źródłem danych** do analizy bezpieczeństwa sieci (narzędzia, logi, zapory hostowe).
- Windows: AD, GPO, Event Log (4624/4625), Defender Firewall, AppLocker; Linux: iptables/nftables, auditd, SELinux, fail2ban, narzędzia analityczne.
- Skuteczna analiza wymaga **korelacji** logów hosta z ruchem sieciowym.

---
[⬅️ Poprzedni temat](1_Podstawowe_zadania_bezpieczeństwa_i_cyberbezpieczeństwa_w_infrastrukturze_sieciowej.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️️](3_Analiza_protokołów_i_usług_sieciowych_w_ocenie_bezpieczeństwa.md)