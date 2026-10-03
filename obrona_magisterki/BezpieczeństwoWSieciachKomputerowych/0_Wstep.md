# Bezpieczeństwo w sieciach komputerowych – wprowadzenie

## Model OSI / TCP-IP – przypomnienie (pod kątem bezpieczeństwa)

| Warstwa OSI | Urządzenia / protokoły | Typowe zagrożenia | Podstawowa obrona |
| :-: | :--- | :--- | :--- |
| **7 Aplikacji** | HTTP, DNS, SMTP, FTP, SSH, SMB | SQLi, XSS, DNS poisoning, phishing, brute force | WAF, walidacja, szyfrowanie, MFA |
| **6 Prezentacji** | TLS/SSL, kodowanie | słabe szyfry, downgrade, MITM TLS | TLS 1.2/1.3, certyfikaty |
| **5 Sesji** | sesje, tokeny | przejęcie sesji | zarządzanie sesją, timeouty |
| **4 Transportu** | TCP, UDP | SYN flood, skanowanie portów, hijacking | zapora stanowa, SYN cookies, rate limiting |
| **3 Sieciowa** | IP, ICMP, routing (OSPF, BGP), router | IP spoofing, smurf, ataki na routing, DDoS | ACL, uRPF, uwierzytelnianie routingu, IPsec |
| **2 Łącza danych** | Ethernet, ARP, VLAN, STP, przełącznik | MAC flooding, ARP spoofing, rogue DHCP, VLAN hopping | port security, DHCP snooping, DAI, 802.1X |
| **1 Fizyczna** | kable, Wi-Fi, huby | podsłuch, sabotaż, zagłuszanie | ochrona fizyczna, szyfrowanie |

## Pojęcia podstawowe

| Pojęcie | Znaczenie |
| :--- | :--- |
| **Zasób (asset)** | wszystko, co ma wartość (dane, sprzęt, usługi) |
| **Zagrożenie (threat)** | potencjalna przyczyna niepożądanego incydentu |
| **Podatność (vulnerability)** | słabość, którą zagrożenie może wykorzystać |
| **Ryzyko (risk)** | prawdopodobieństwo × skutek wykorzystania podatności |
| **Atak** | celowe wykorzystanie podatności |
| **Exploit / payload** | kod wykorzystujący podatność / jego ładunek |
| **IoC / TTP** | wskaźniki włamania / taktyki, techniki i procedury atakującego |
| **SOC / SIEM** | centrum operacji bezpieczeństwa / system korelacji logów |
| **IDS / IPS** | system wykrywania / zapobiegania włamaniom |
| **NGFW / WAF / UTM** | zapora nowej generacji / aplikacyjna / zintegrowane urządzenie bezpieczeństwa |
| **DMZ** | strefa zdemilitaryzowana dla usług wystawionych na zewnątrz |
| **NAC / 802.1X** | kontrola dostępu do sieci / uwierzytelnianie portowe |
| **SPAN / TAP** | lustrzane odbicie portu / sprzętowy rozgałęźnik do podsłuchu ruchu |

---
[⬅️ Poprzedni temat](BezpieczeństwoWSieciachKomputerowych_tytul.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](1_Podstawowe_zadania_bezpieczeństwa_i_cyberbezpieczeństwa_w_infrastrukturze_sieciowej.md)