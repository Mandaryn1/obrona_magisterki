# Zabezpieczanie styku sieci teleinformatycznej z sieciami zewnętrznymi

> Opracowanie z własnej wiedzy ***(uzupełnienie)*** z odwołaniami do wykładów: zapory, NGFW, IDS/IPS, NAT, VPN, UTM (*Zapory i IDS*), segmentacja i Zero Trust (W7 chmura), obrona w głąb (W1).

## Czym jest styk (perymetr, brzeg sieci)

**Styk z sieciami zewnętrznymi (perymetr/network edge)** to miejsca, w których sieć organizacji łączy się z **Internetem**, **sieciami partnerów i operatorów**, **chmurą**, **zdalnymi lokalizacjami i użytkownikami**. To obszar największej ekspozycji i pierwsza linia obrony. Współcześnie perymetr się **rozmywa** (chmura, praca zdalna, BYOD, SaaS), więc wzmacnia się go **zero trust, segmentacją i ochroną „na brzegu"** (*SASE/SSE*).

## Zagrożenia na styku

skanowanie i rozpoznanie, exploity usług wystawionych do Internetu (w tym urządzeń brzegowych: VPN, zapory, routery), ataki **DDoS**, **brute force i credential stuffing**, phishing i malware przez WWW/e-mail, **IP spoofing**, ataki na DNS i routing (**BGP hijacking**), eksfiltracja, ataki z sieci partnerów i dostawców (łańcuch dostaw), przejęcie połączeń VPN.

## Architektura styku

### Model stref (DMZ)

```
                         Internet
                            │
             ┌──────────────┴──────────────┐
             │  router brzegowy (ACL, uRPF, │
             │  rate limit, anty-DDoS)      │
             └──────────────┬───────────────┘
                   ┌────────▼─────────┐  zapora zewnętrzna / NGFW + IPS
                   │       DMZ        │  (serwery WWW, poczta, reverse proxy, VPN, DNS)
                   └────────┬─────────┘
                   ┌────────▼─────────┐  zapora wewnętrzna (segmentacja)
                   │  sieć wewnętrzna │  użytkownicy | serwery | dane | zarządzanie
                   └──────────────────┘
```

- **DMZ (strefa zdemilitaryzowana, screened subnet)** – usługi wystawione na zewnątrz **oddzielone** od sieci wewnętrznej; włamanie do serwera w DMZ nie daje bezpośredniego dostępu do LAN.
- **Dwie zapory różnych producentów** (zewnętrzna i wewnętrzna) lub jedna z wieloma strefami – większa odporność.
- **Wykład W7:** segmentacja i mikrosegmentacja jako „kluczowa technika obrony w głąb"; strefa **DMZ/publiczna**, warstwa aplikacyjna, danych, zarządzania; komunikacja między strefami przez **kontrolowane bramy z inspekcją zapory**.
- **Redundancja** (HA par zapór, dwóch operatorów, BGP multi-homing).

## Mechanizmy ochrony styku

| Mechanizm | Zadanie |
| :--- | :--- |
| **Router brzegowy (ACL)** | filtrowanie wstępne: blokada adresów prywatnych/bogon z Internetu (anti-spoofing), uRPF, wyłączenie zbędnych usług, ograniczenie ICMP, **rate limiting** |
| **Zapora stanowa / NGFW** (wykład) | polityka **domyślnej odmowy**, kontrola stref, **kontrola aplikacji** niezależnie od portu, **IPS, antymalware, inspekcja TLS** (slajd 9) |
| **IDS/IPS** (wykład) | wykrywanie i blokowanie ataków, skanowania, exploitów (slajd 17) |
| **WAF** (wykład) | ochrona aplikacji WWW (SQLi, XSS, command injection, path traversal) – slajdy 8, 31 |
| **Reverse proxy / load balancer** (wykład: ukrywa architekturę, rozdziela ruch, chroni przed DDoS) | terminacja TLS, ukrycie serwerów, odporność |
| **Ochrona przed DDoS** | **scrubbing** u operatora, CDN/anycast, rate limiting, SYN cookies, blackholing (RTBH); w chmurze dostawca (wykład, slajd 30) |
| **NAT/PAT** (wykład, slajd 24) | maskowanie adresacji wewnętrznej, utrudnienie rozpoznania topologii; *dodatkowa* warstwa, nie zabezpieczenie podstawowe |
| **VPN** (wykład) | szyfrowane połączenia: **site-to-site** (oddziały, partnerzy) i **remote access**; MFA, silne szyfry |
| **Proxy WWW i filtrowanie treści** | kontrola ruchu wychodzącego, reputacja URL, antymalware, DLP |
| **Bezpieczeństwo DNS** | filtrowanie DNS (np. Cisco Umbrella), DNSSEC, ograniczenie rekursji; ochrona przed tunelowaniem |
| **Bezpieczeństwo poczty** | bramka antyspam/antyphishing, **SPF, DKIM, DMARC**, sandbox załączników (wykład, slajd 34) |
| **Filtrowanie ruchu wychodzącego (egress)** | blokada nieautoryzowanych połączeń, wykrywanie C2 i eksfiltracji |
| **Zabezpieczenie routingu** | uwierzytelnianie protokołów, filtry prefiksów, **RPKI/ROA**, max-prefix |
| **Segmentacja i NAC** | ograniczenie ruchu bocznego po przełamaniu perymetru (tematy 4–5) |
| **Monitoring** | SIEM, NetFlow, IDS, logi zapór, SOC (wykład) |
| **Inspekcja ruchu szyfrowanego** | proxy TLS z zaufanym certyfikatem CA (z uwzględnieniem RODO i prywatności – wykład, slajd 27) |

## Typy połączeń na styku i zasady

| Połączenie | Zasady zabezpieczenia |
| :--- | :--- |
| **Internet** | domyślna odmowa, tylko wymagane usługi (publikowane przez DMZ/reverse proxy), IPS, anty-DDoS, regularne skanowanie własnej ekspozycji |
| **Dostęp zdalny pracowników** | **VPN z MFA** lub **ZTNA**, ocena postury urządzenia (NAC), minimalne uprawnienia, brak publicznych RDP/SSH |
| **Łącza do partnerów/dostawców (B2B)** | dedykowane strefy (extranet), **VPN IPsec lub łącza prywatne**, ACL ograniczone do konkretnych usług, zasada najmniejszych uprawnień, umowy i audyt |
| **Oddziały (WAN)** | VPN site-to-site/SD-WAN, centralna polityka, segmentacja |
| **Chmura** | prywatne łącza (Direct Connect, ExpressRoute) lub VPN, **VPC/VNet, Security Groups**, spójne polityki i tożsamość (hybryda – wykład) |
| **Usługi SaaS** | CASB/SSE, SSO + MFA, DLP |
| **Zarządzanie (out-of-band)** | oddzielna sieć, bastion/jump host |

## Zasady projektowe

1. **Obrona w głąb** – wiele warstw na styku (router, zapora, IPS, WAF, proxy, EDR).
2. **Domyślna odmowa i najmniejsze uprawnienia** w regułach.
3. **Minimalna ekspozycja** – mniej usług w Internecie; inwentarz i skanowanie ekspozycji.
4. **Separacja stref** i kontrola ruchu **w obu kierunkach (ingress i egress)**.
5. **Zero Trust** – „nigdy nie ufaj, zawsze weryfikuj"; **założenie naruszenia**; weryfikacja tożsamości i urządzenia nawet za perymetrem (wykład W7).
6. **Redundancja i odporność** (HA, anty-DDoS, wiele łączy).
7. **Utwardzanie urządzeń brzegowych**: aktualizacje (szczególnie VPN/zapory – częste cele exploitów), zarządzanie tylko z sieci zarządzania, MFA dla administratorów, wyłączone zbędne usługi.
8. **Widoczność i reagowanie**: pełne logowanie, SIEM, SOC, playbooki.
9. **Zarządzanie zmianami i przeglądy reguł** (cykl życia polityk – wykład, slajd 29).
10. **Testy**: regularne skany podatności i testy penetracyjne perymetru (temat 10).

## Przykład – reguły styku (zapora stanowa Linux) *(uzupełnienie)*

```bash
# domyślna odmowa
iptables -P INPUT DROP; iptables -P FORWARD DROP; iptables -P OUTPUT DROP
# stan połączeń
iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
# anti-spoofing: adresy prywatne z Internetu
iptables -A FORWARD -i eth0 -s 10.0.0.0/8 -j DROP
iptables -A FORWARD -i eth0 -s 192.168.0.0/16 -j DROP
# Internet -> DMZ: tylko HTTPS do reverse proxy
iptables -A FORWARD -i eth0 -o dmz0 -p tcp -d 203.0.113.10 --dport 443 -j ACCEPT
# DMZ -> sieć wewnętrzna: tylko serwer aplikacyjny -> baza
iptables -A FORWARD -i dmz0 -o lan0 -s 10.10.1.5 -d 10.20.1.5 -p tcp --dport 5432 -j ACCEPT
# LAN -> Internet: tylko przez proxy (egress)
iptables -A FORWARD -i lan0 -o eth0 -s 10.20.5.10 -p tcp -m multiport --dports 80,443 -j ACCEPT
# logowanie reszty
iptables -A FORWARD -j LOG --log-prefix "FW-DROP: "
```

## Podsumowanie

- Styk = Internet, partnerzy, chmura, zdalni użytkownicy – największa ekspozycja; **perymetr się rozmywa**, więc łączy się go z segmentacją i Zero Trust.
- Architektura: **router brzegowy (ACL, uRPF, anty-DDoS) → zapora/NGFW + IPS → DMZ → zapora wewnętrzna → strefy**; redundancja.
- Mechanizmy: **zapory, IDS/IPS, WAF, reverse proxy, NAT, VPN z MFA, filtrowanie DNS/poczty/WWW, egress filtering, zabezpieczenie routingu, inspekcja TLS, SIEM**.
- Zasady: domyślna odmowa, minimalna ekspozycja, separacja stref, zero trust, utwardzanie urządzeń brzegowych, testy i przeglądy reguł.
