# Zabezpieczanie styku sieci teleinformatycznej z sieciami zewnętrznymi

**Styk (perymetr)** to miejsca, w których sieć organizacji łączy się z **Internetem, sieciami partnerów, operatorami, chmurą i użytkownikami zdalnymi**. Ma największą ekspozycję na ataki, więc jest pierwszą linią obrony. Współcześnie perymetr się rozmywa (chmura, praca zdalna), dlatego łączy się go z **segmentacją i podejściem Zero Trust**.

**Zagrożenia:** skanowanie i rozpoznanie, exploity usług wystawionych do Internetu (VPN, zapory, poczta), **DDoS**, brute force i credential stuffing, phishing i malware, **IP spoofing**, ataki na DNS i routing (BGP hijacking), eksfiltracja, ataki z sieci partnerów.

**Architektura:**

- **router brzegowy:** wstępne filtrowanie (ACL, **uRPF**, blokada adresów prywatnych i bogon z Internetu, rate limiting),
- **zapora/NGFW z IPS:** polityka domyślnej odmowy, kontrola aplikacji, antymalware, inspekcja TLS,
- **DMZ (strefa zdemilitaryzowana):** usługi wystawione na zewnątrz (WWW, poczta, reverse proxy, VPN) oddzielone od sieci wewnętrznej; włamanie do serwera w DMZ nie daje bezpośredniego dostępu do LAN,
- **zapora wewnętrzna** do segmentacji stref (użytkownicy, serwery, dane, zarządzanie),
- **redundancja** (HA zapór, dwóch operatorów).

**Mechanizmy ochrony:**

- **zapory, IDS/IPS, WAF** (ochrona aplikacji WWW), **reverse proxy i load balancer** (ukrywają architekturę),
- **ochrona przed DDoS:** scrubbing u operatora, CDN i anycast, rate limiting, blackholing (RTBH),
- **VPN** (IPsec, SSL) z **MFA** dla zdalnych użytkowników i oddziałów,
- **NAT/PAT:** maskuje adresację wewnętrzną (dodatkowa warstwa, nie główne zabezpieczenie),
- **bezpieczeństwo DNS i poczty:** DNSSEC, filtrowanie DNS, **SPF, DKIM, DMARC**, sandbox załączników,
- **filtrowanie ruchu wychodzącego (egress):** blokada nieautoryzowanych połączeń, wykrywanie C2 i eksfiltracji,
- **zabezpieczenie routingu:** uwierzytelnianie protokołów, filtry prefiksów, **RPKI**,
- **monitoring:** logi, SIEM, NetFlow, SOC.

**Różne typy połączeń:** dostęp do Internetu przez DMZ i proxy, zdalni pracownicy przez VPN z MFA lub ZTNA, partnerzy przez wydzielone strefy i VPN z ACL ograniczonymi do konkretnych usług, chmura przez VPC/VNet i prywatne łącza.

**Zasady projektowe:** **obrona w głąb**, **domyślna odmowa i najmniejsze uprawnienia**, minimalna ekspozycja usług (regularne skanowanie własnych adresów), kontrola ruchu w obu kierunkach, **Zero Trust**, utwardzanie urządzeń brzegowych (szybkie aktualizacje, zarządzanie z odrębnej sieci), przegląd reguł i regularne testy penetracyjne.

## Podsumowanie

- Styk = Internet, partnerzy, chmura, zdalni użytkownicy – największa ekspozycja; **perymetr się rozmywa**, więc łączy się go z segmentacją i Zero Trust.
- Architektura: **router brzegowy (ACL, uRPF, anty-DDoS) → zapora/NGFW + IPS → DMZ → zapora wewnętrzna → strefy**; redundancja.
- Mechanizmy: **zapory, IDS/IPS, WAF, reverse proxy, NAT, VPN z MFA, filtrowanie DNS/poczty/WWW, egress filtering, zabezpieczenie routingu, inspekcja TLS, SIEM**.
- Zasady: domyślna odmowa, minimalna ekspozycja, separacja stref, zero trust, utwardzanie urządzeń brzegowych, testy i przeglądy reguł.

---
[⬅️ Poprzedni temat](2_Standardy_i_dobre_praktyki_bezpieczeństwa_ISO_27001_NIS2_i_inne_normy.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Kontrola_dostępu_do_sieci_oraz_mechanizmy_NAC.md)