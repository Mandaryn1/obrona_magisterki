# Rola systemów Windows i Linux w analizie bezpieczeństwa sieci komputerowej

## Dwie role systemów operacyjnych

Windows i Linux pełnią w analizie bezpieczeństwa sieci dwie role. 
1. Są **narzędziem analityka**, czyli platformą do monitorowania, skanowania i badania ruchu. 
2. Są też **źródłem danych**: hosty generują logi i zdarzenia, a ich konfiguracja wpływa na bezpieczeństwo całej sieci.

## Linux

**Zastosowania w analizie bezpieczeństwa:**

- **Narzędzia analityczne:** tcpdump i Wireshark (przechwytywanie ruchu), **Nmap** (skanowanie portów i usług), **Snort/Suricata** (IDS/IPS), **Zeek** (metadane połączeń), dystrybucje Kali i Security Onion.
- **Zapora:** iptables/nftables, UFW, firewalld.
- **Logi:** `/var/log` (m.in. `auth.log` lub `secure` z logowaniami i `sudo`), **auditd**, journald.
- **Ochrona i wykrywanie:** fail2ban (blokada brute force), SELinux/AppArmor, kontrola integralności (AIDE).
- **Zalety:** otwartość, elastyczność, skryptowanie, wiele darmowych narzędzi. Wymaga większej wiedzy.

**Typowe polecenia diagnostyczne:**

```bash
ip a; ip r; ip neigh          # interfejsy, trasy, tablica ARP/ND
ss -tulpn                     # nasłuchujące porty i procesy
tcpdump -i eth0 -nn 'tcp port 22 or icmp'   # przechwytywanie
nmap -sS -sV 10.0.0.0/24      # inwentaryzacja usług
journalctl -u sshd --since "1 hour ago"
iptables -L -n -v ; nft list ruleset
```

## Windows

**Dominuje na stacjach i w środowiskach domenowych.**

**Zastosowania:**

- **Dzienniki zdarzeń (Event Log):** logowania udane (**4624**) i nieudane (**4625**), zmiany uprawnień (**4670**), dostęp do plików (**4663**), uruchomienia procesów.
- **Narzędzia:** Zapora Windows Defender, Defender/EDR, **AppLocker**, **GPO** (centralna polityka), **Sysmon** (szczegółowa telemetria), Active Directory.
- **Zarządzanie:** PowerShell, `netstat`, Podgląd zdarzeń.
- **Zalety:** centralne zarządzanie w domenie, bogate logowanie. Wady: duża powierzchnia ataku i popularność wśród atakujących.

**Typowe polecenia** *(uzupełnienie)*:

```powershell
netstat -ano                                  # połączenia z PID
Get-NetTCPConnection -State Listen            # nasłuchujące porty
Get-EventLog -LogName Security -InstanceId 4625 -Newest 20
arp -a ; ipconfig /all ; nslookup domena
netsh advfirewall show allprofiles
```


## Wspólne zastosowania:

- sprawdzanie **otwartych portów i usług** (`netstat`, `ss`),
- **korelacja logów** z obu systemów w **SIEM**, co pozwala odtworzyć przebieg ataku,
- wykrywanie brute force, skanowania i ruchu bocznego,
- weryfikacja konfiguracji (utwardzanie, CIS Benchmarks, aktualizacje),
- **analiza powłamaniowa** (logi, procesy, zrzuty pamięci).

**Wniosek:** w sieci działają oba systemy, więc analityk musi znać oba. Linux jest typowym środowiskiem narzędzi i serwerów, Windows głównym źródłem zdarzeń ze stacji i domeny.

## Podsumowanie

- Systemy Windows i Linux są jednocześnie **chronionymi zasobami** (utwardzanie, łatanie) i **platformami/źródłem danych** do analizy bezpieczeństwa sieci (narzędzia, logi, zapory hostowe).
- Windows: AD, GPO, Event Log (4624/4625), Defender Firewall, AppLocker; Linux: iptables/nftables, auditd, SELinux, fail2ban, narzędzia analityczne.
- Skuteczna analiza wymaga **korelacji** logów hosta z ruchem sieciowym.

---
[⬅️ Poprzedni temat](1_Podstawowe_zadania_bezpieczeństwa_i_cyberbezpieczeństwa_w_infrastrukturze_sieciowej.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️️](3_Analiza_protokołów_i_usług_sieciowych_w_ocenie_bezpieczeństwa.md)
