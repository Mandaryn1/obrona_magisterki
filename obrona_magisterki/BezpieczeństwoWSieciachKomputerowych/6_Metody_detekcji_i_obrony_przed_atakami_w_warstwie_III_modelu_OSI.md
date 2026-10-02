# Metody detekcji i obrony przed atakami w warstwie III modelu OSI

> **Uwaga:** wykład wymienia tu m.in. **spoofing adresu IP, ICMP flood, Ping of Death, tunelowanie ICMP, ataki amplifikacyjne, NAT, VPN, rate limiting** (Zapory/IDS, slajdy 17, 24, 30). Reszta – ***(uzupełnienie)*** z własnej wiedzy.

## Specyfika warstwy 3

Warstwa sieciowa (**IP, ICMP, routing**) odpowiada za adresowanie i trasowanie pakietów między sieciami. Protokół IP **nie uwierzytelnia nadawcy**, a protokoły routingu domyślnie **ufają sąsiadom**, co umożliwia sfałszowanie adresu, zalanie ruchem i przejęcie tras. Zapory i routery są tu głównym miejscem obrony.

## Ataki L3 i obrona

### 1. IP spoofing (fałszowanie adresu źródłowego) – wykład

- **Atak:** podszycie się pod inny adres IP – obejście ACL opartych na adresie, ataki **odbite/amplifikacyjne**, ukrycie źródła DoS, ataki na zaufanie (hosty ufające adresom).
- **Wykrywanie:** pakiety z adresami **prywatnymi/bogon/własnej sieci** na interfejsie zewnętrznym; niespójne TTL; ruch wychodzący z adresami spoza własnych podsieci; analiza NetFlow.
- **Obrona:**
  - **filtrowanie wejściowe i wyjściowe (ingress/egress filtering, BCP 38/84)** – odrzucanie pakietów z nieprawidłowym źródłem,
  - **uRPF (Unicast Reverse Path Forwarding)** – sprawdzenie, czy trasa powrotna do źródła prowadzi przez ten sam interfejs,
  - listy ACL (blokada adresów prywatnych/bogon z Internetu),
  - uwierzytelnianie na wyższych warstwach, IPsec,
  - dla IPv6: **RA Guard**, ND inspection, SEND.

### 2. Ataki ICMP – wykład

| Atak | Opis | Obrona |
| :--- | :--- | :--- |
| **ICMP/ping flood** | zalanie echo request, wyczerpanie łącza lub CPU | **rate limiting** ICMP, filtrowanie typów |
| **Smurf** | ping do adresu rozgłoszeniowego z podrobionym źródłem ofiary → lawina odpowiedzi | blokada *directed broadcast* (`no ip directed-broadcast`) |
| **Ping of Death** | przekroczenie dozwolonego rozmiaru pakietu (fragmentacja) → awaria stosu | aktualizacje systemów, filtry fragmentów |
| **Tunelowanie ICMP** | ukryty kanał C2/eksfiltracji w ładunku ICMP | IDS (anomalie rozmiaru i częstości), ograniczenie ICMP |
| **ICMP redirect / router advertisement** | podsunięcie fałszywej trasy | wyłączenie przyjmowania redirectów |
| **Rozpoznanie (traceroute, ping sweep)** | mapowanie sieci | ograniczanie ICMP, nie odpowiadanie na zbędne typy |

Zasada: **nie blokować ICMP całkowicie** (potrzebny do PMTUD – typ 3 kod 4, diagnostyki), lecz **filtrować i limitować**.

### 3. DoS/DDoS na warstwie 3 i amplifikacja – wykład

- **Atak:** zalanie łącza lub urządzeń (UDP flood, ataki odbite DNS/NTP/memcached z podrobionym źródłem).
- **Wykrywanie:** gwałtowny wzrost wolumenu/pakietów na sekundę, rozkład źródeł, NetFlow/sFlow, IDS/IPS (**wykrywanie anomalii wolumenu i rodzaju ruchu**).
- **Obrona:** **rate limiting**, ACL, **RTBH (remotely triggered black hole)**, **usługi czyszczenia ruchu (scrubbing), CDN/anycast**, load balancing (wykład, slajd 30), **uRPF**, zamknięcie otwartych resolverów DNS/serwerów NTP, współpraca z operatorem (ISP).

### 4. Ataki na routing

| Atak | Opis | Obrona |
| :--- | :--- | :--- |
| **Fałszywe trasy w RIP/OSPF/EIGRP** | wstrzyknięcie tras – przekierowanie, black hole, MITM | **uwierzytelnianie routingu** (OSPF z kluczem kryptograficznym, EIGRP, RIPv2), **pasywne interfejsy**, filtrowanie tras |
| **BGP hijacking / route leak** | przejęcie prefiksu IP lub wyciek tras – przekierowanie ruchu | **filtry prefiksów**, **max-prefix**, **RPKI/ROA**, uwierzytelnianie sesji (TCP-AO/MD5), GTSM, monitoring BGP |
| **Źródło trasowane (source routing)** | napastnik narzuca ścieżkę | `no ip source-route` |
| **Ataki na bramę domyślną** | przejęcie roli (HSRP/VRRP spoofing) | uwierzytelnianie HSRP/VRRP |

### 5. Fragmentacja i nietypowe pakiety

- **Ataki:** teardrop, nakładające się fragmenty, ukrywanie ładunku przed IDS (ominięcie reguł).
- **Obrona:** normalizacja i **ponowne składanie fragmentów** w zaporze/IPS, odrzucanie nieprawidłowych fragmentów, aktualizacje.

### 6. NAT, filtracja i zapory w warstwach 3–4

- **NAT/PAT** maskuje adresację wewnętrzną i utrudnia rozpoznanie (wykład, slajd 24: „ochrona przez *obscurity*" jako **dodatkowa** warstwa, nie zabezpieczenie podstawowe).
- **Zapora z filtracją pakietów** (adresy IP, porty, protokół, flagi TCP) i **zapora stanowa** (tabela stanów, dynamiczne reguły dla odpowiedzi – wykład, slajdy 6–7: iptables/conntrack, Cisco ASA).
- **ACL** na routerach (standardowe/rozszerzone) – filtracja na granicy stref.

### 7. VPN i szyfrowanie (IPsec) – wykład, slajd 24

IPsec zapewnia **poufność, integralność i uwierzytelnienie na warstwie 3** (tryb tunelowy/transportowy); VPN site-to-site i remote access (IPsec, OpenVPN, WireGuard). Chroni przed podsłuchem i spoofingiem w trasie.

## Przykład konfiguracji (Cisco IOS) *(uzupełnienie)*

```text
! uRPF – ochrona przed spoofingiem
interface GigabitEthernet0/0
 ip verify unicast source reachable-via rx
 no ip redirects
 no ip unreachables
 no ip directed-broadcast
!
no ip source-route
!
! ACL – blokada adresów prywatnych/loopback z Internetu
ip access-list extended OUTSIDE-IN
 deny ip 10.0.0.0 0.255.255.255 any
 deny ip 172.16.0.0 0.15.255.255 any
 deny ip 192.168.0.0 0.0.255.255 any
 deny ip 127.0.0.0 0.255.255.255 any
 permit icmp any any echo-reply
 permit icmp any any packet-too-big
 deny   icmp any any
 permit tcp any host 203.0.113.10 eq 443
 deny ip any any log
!
! Uwierzytelnianie OSPF
interface GigabitEthernet0/1
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 TajneHaslo
router ospf 1
 passive-interface default
 no passive-interface GigabitEthernet0/1
```

Reguła Linux (iptables) – limit ICMP i blokada spoofingu *(uzupełnienie)*:

```bash
iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 5/s -j ACCEPT
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
iptables -A INPUT -i eth0 -s 10.0.0.0/8 -j DROP     # prywatne z zewnątrz
```

## Metody detekcji

| Metoda | Co wykrywa |
| :--- | :--- |
| **NIDS/NIPS** (Snort, Suricata – wykład) | sygnatury ataków, anomalie ICMP, fałszywe adresy |
| **NetFlow/sFlow/IPFIX** (wykład, slajd 33) | nagłe skoki ruchu, rozkład źródeł, DDoS |
| **Analiza pakietów** (Wireshark, tcpdump) | niespójne TTL, fragmenty, spoofing |
| **Logi zapory i routerów** (syslog → SIEM) | odrzucone pakiety (bogon, spoof), zmiany tras |
| **Monitoring routingu** | zmiany tablic tras, nietypowe ogłoszenia BGP |
| **Honeypoty, systemy anty-DDoS** | wczesne ostrzeganie |

Przykładowa reguła Suricata *(uzupełnienie)*: `alert icmp any any -> $HOME_NET any (msg:"ICMP flood"; itype:8; threshold:type both, track by_dst, count 200, seconds 1; sid:1000001; rev:1;)`.

## Podsumowanie

- Ataki L3: **IP spoofing**, ataki **ICMP** (flood, smurf, Ping of Death, tunelowanie), **DoS/DDoS i amplifikacja**, ataki na **routing** (fałszywe trasy, BGP hijacking), nadużycia fragmentacji.
- Obrona: **filtrowanie wejściowe/wyjściowe, uRPF, ACL, zapory stanowe, rate limiting ICMP, wyłączenie source routing/redirectów/directed broadcast, uwierzytelnianie routingu, RPKI, IPsec/VPN, scrubbing/CDN/RTBH**.
- Detekcja: **IDS/IPS, NetFlow, analiza pakietów, logi i SIEM**.
