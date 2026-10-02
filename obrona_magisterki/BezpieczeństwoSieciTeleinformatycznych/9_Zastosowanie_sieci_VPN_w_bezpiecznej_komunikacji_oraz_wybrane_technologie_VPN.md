# Zastosowanie sieci VPN w bezpiecznej komunikacji oraz wybrane technologie VPN

> Z wykładów: slajd *Firewall, NAT i VPN – synergiczne mechanizmy ochrony* (Zapory/IDS, slajd 24: szyfrowanie całego ruchu, poufność i integralność, **site-to-site**, **remote access**, protokoły **IPsec, OpenVPN, WireGuard**), VPN w zabezpieczeniu pracy zdalnej (W1, slajd 55) i VPN w UTM/ASA. Szczegóły techniczne – ***(uzupełnienie)***.

## Czym jest VPN

**VPN (Virtual Private Network)** – **wirtualna sieć prywatna** zestawiana **przez sieć publiczną lub współdzieloną** (Internet, sieć operatora), zapewniająca **bezpieczny, szyfrowany „tunel"** między punktami. Wykład: VPN *szyfruje cały ruch między punktami końcowymi, zapewnia poufność i integralność danych*; dostępne wersje **Site-to-Site** (połączenia oddziałów) i **Remote Access** (pracownicy zdalni); protokoły **IPsec, OpenVPN, WireGuard**.

## Cele i usługi bezpieczeństwa

| Własność | Realizacja |
| :--- | :--- |
| **Poufność** | szyfrowanie (np. AES-GCM, ChaCha20) |
| **Integralność** | MAC/AEAD, sumy kontrolne |
| **Uwierzytelnienie stron** | certyfikaty, klucze, PSK, MFA |
| **Ochrona przed powtórzeniem (anti-replay)** | numery sekwencyjne |
| **Enkapsulacja (tunelowanie)** | pakiet wewnętrzny „opakowany" w zewnętrzny (może przenosić prywatną adresację) |

## Zastosowania

1. **Dostęp zdalny pracowników** (praca zdalna, hybrydowa – W1: *„VPN, secure Wi-Fi, device security"*) do zasobów firmowych.
2. **Połączenie oddziałów (site-to-site)** zamiast kosztownych łączy dzierżawionych.
3. **Połączenia z partnerami i dostawcami (B2B/extranet)**.
4. **Chmura hybrydowa** – bezpieczne łącze do VPC/VNet (wykład: VPN i gatewaye – bezpieczne połączenia między środowiskiem lokalnym a chmurowym).
5. **Ochrona w niezaufanych sieciach** (hotspoty, hotele).
6. **Zarządzanie urządzeniami** (administracja zdalna, OOB), IoT/OT.
7. **Segmentacja i separacja ruchu** (VPN jako wydzielona sieć logiczna).
8. **Prywatność** (ukrycie ruchu przed operatorem/ISP; VPN konsumenckie – inny model zaufania).

## Rodzaje VPN

| Typ | Opis |
| :--- | :--- |
| **Remote Access (Client-to-Site)** | pojedynczy użytkownik/urządzenie z klientem VPN łączy się z bramą w firmie |
| **Site-to-Site** | stałe tunele między bramami lokalizacji; transparentne dla hostów |
| **Host-to-Host** | tunel bezpośrednio między dwoma hostami |
| **Full tunnel / Split tunnel** | cały ruch przez VPN / tylko ruch do zasobów firmowych (split – mniejsze obciążenie, ale ryzyko obejścia kontroli) |
| **Warstwa 3 / warstwa 2** | tunelowanie IP / ramek Ethernet (rozciągnięcie LAN, np. L2TP, VXLAN, OpenVPN TAP) |
| **VPN dostawcy (provider-provisioned)** | **MPLS L3VPN/L2VPN** – izolacja klientów w sieci operatora |
| **Overlay SD-WAN** | automatycznie zestawiane tunele IPsec między oddziałami z centralnym sterowaniem |

## Wybrane technologie VPN

### 1. IPsec (Internet Protocol Security)

**Zestaw protokołów** zapewniających bezpieczeństwo **warstwy 3** (RFC 4301 i in.); standard dla site-to-site i remote access.

| Element | Opis |
| :--- | :--- |
| **AH (Authentication Header)** | uwierzytelnienie i integralność (bez szyfrowania); nie przechodzi przez NAT – rzadko używany |
| **ESP (Encapsulating Security Payload)** | **szyfrowanie + integralność + uwierzytelnienie**; najczęściej używany |
| **Tryb tunelowy (tunnel)** | cały oryginalny pakiet IP enkapsulowany (nowy nagłówek IP) – site-to-site, bramy |
| **Tryb transportowy (transport)** | chroniona tylko dane (ładunek) – host-to-host |
| **SA (Security Association)** | jednokierunkowe powiązanie parametrów (algorytmy, klucze, czas życia); parametry w **SPI** |
| **IKE (Internet Key Exchange)** | **negocjacja SA i uzgadnianie kluczy** (UDP 500, **NAT-T UDP 4500**) |
| **IKEv1** | **Faza 1** (ISAKMP SA – kanał zarządzania; tryb główny/agresywny) i **Faza 2** (IPsec SA – tryb szybki) |
| **IKEv2** | prostszy, szybszy, MOBIKE (mobilność), wbudowane EAP, odporniejszy – **zalecany** |
| **Uwierzytelnienie** | **certyfikaty (PKI)** lub **PSK (klucz wstępnie współdzielony)**; w remote access także EAP/XAUTH/MFA |
| **Algorytmy** | AES-128/256 (zalecane **AES-GCM**), SHA-2, wymiana kluczy **DH grupa ≥ 14 / ECDH (19, 20, 21)**, **PFS** (Perfect Forward Secrecy) |
| **Typy konfiguracji** | policy-based (selektory ruchu) lub **route-based (VTI)** |
| **Rozszerzenia** | **GRE over IPsec**, **DMVPN** (dynamiczne tunele spoke-to-spoke), **FlexVPN**, **GETVPN** |
| **Zalety** | standard, wsparcie sprzętowe (ASIC), wydajność, interoperacyjność |
| **Wady** | złożona konfiguracja, problemy z NAT/firewall (NAT-T), wiele opcji → błędy (słabe PSK, przestarzałe algorytmy) |

### 2. SSL/TLS VPN

Wykorzystują **TLS** (HTTPS, port 443), więc łatwo przechodzą przez zapory i NAT. Dwie odmiany: **clientless** (portal WWW) i **z klientem** (np. **Cisco AnyConnect/Secure Client**, Fortinet FortiClient, GlobalProtect). Zalety: dostęp **granularny do aplikacji**, łatwość, MFA; wady: zależność od klienta/portalu, wydajność. Częsty wybór dla **remote access**.

### 3. OpenVPN

**Open source**, oparty na **TLS** (OpenSSL), transport **UDP 1194** (lub TCP, także 443), tryby **TUN (L3)** i **TAP (L2)**; uwierzytelnianie certyfikatami, hasłami, MFA; konfigurowalny; wielka społeczność. Wykład wymienia go obok IPsec i WireGuard (slajd 24). Wady: większy narzut i opóźnienia niż WireGuard, złożoność konfiguracji, działa w przestrzeni użytkownika.

### 4. WireGuard

**Nowoczesny, minimalistyczny** protokół VPN (od 2020 w jądrze Linux 5.6): ok. **4 tys. linii kodu** (łatwy audyt), **UDP**, **kryptografia „sztywna"**: **Curve25519** (ECDH), **ChaCha20-Poly1305** (AEAD), **BLAKE2s**, **Noise protocol framework**; uwierzytelnienie **kluczami publicznymi** peerów (podobnie do SSH), **cichy** (nie odpowiada nieuwierzytelnionym pakietom), szybki rekonfiguracja i roaming, wysoka wydajność, niski narzut (dobry dla IoT i mobilnych). Ograniczenia: mniej funkcji „enterprise" (brak wbudowanego zarządzania użytkownikami, dynamicznego przydziału adresów – rozwiązuje się narzędziami nad nim), statyczne adresy IP peerów, kwestie prywatności (stały klucz). Wykład: pfSense i inne zapory wspierają.

### 5. Inne technologie

| Technologia | Opis |
| :--- | :--- |
| **L2TP/IPsec** | L2TP (tunel L2) + IPsec (szyfrowanie); wsparcie natywne w systemach; starszy |
| **PPTP** | **przestarzały i złamany** (MS-CHAPv2) – **nie używać** |
| **SSTP** | Microsoft; PPP przez TLS |
| **GRE** | prosty tunel enkapsulacji (bez szyfrowania) – zwykle w parze z IPsec |
| **MPLS VPN (L3VPN/L2VPN)** | usługa operatora: izolacja klientów **bez szyfrowania** (prywatność logiczna, nie kryptograficzna) |
| **VXLAN, EVPN** | overlay w centrach danych |
| **SD-WAN** | zarządzane overlaye IPsec/WireGuard z politykami aplikacyjnymi |
| **ZTNA (Zero Trust Network Access)** | dostęp do **konkretnych aplikacji** po weryfikacji tożsamości i postury (alternatywa dla VPN – wykład W7: *ZTNA – każde połączenie wymaga uwierzytelnienia i autoryzacji*) |
| **SSH tunnel, Tor** | tunelowanie punktowe, anonimizacja |

## Porównanie głównych technologii

| Cecha | **IPsec (IKEv2)** | **SSL/TLS VPN** | **OpenVPN** | **WireGuard** |
| :--- | :--- | :--- | :--- | :--- |
| Warstwa | L3 | L7 (TLS) → L3/aplikacje | L2/L3 (TLS) | L3 |
| Transport / port | UDP 500/4500, ESP (IP 50) | TCP/UDP 443 | UDP 1194 / TCP | UDP (dowolny) |
| Uwierzytelnienie | certyfikaty, PSK, EAP | certyfikat + hasło/MFA | certyfikaty, hasła | klucze publiczne |
| Kryptografia | AES-GCM, SHA-2, DH/ECDH | TLS 1.2/1.3 | TLS + AES/ChaCha | Curve25519, ChaCha20-Poly1305 |
| Wydajność | wysoka (sprzęt) | średnia | średnia | **bardzo wysoka** |
| Złożoność | wysoka | niska–średnia | średnia | **niska** |
| Przechodzenie przez zapory/NAT | NAT-T | **bardzo dobre (443)** | dobre | dobre (UDP) |
| Typowe użycie | **site-to-site**, enterprise | **remote access** | remote access, elastyczne | nowoczesne wdrożenia, mobilne, IoT |

## VPN a firewall i NAT (wykład, slajd 24)

Współczesna architektura łączy **zaporę, NAT i VPN** jako **synergiczne mechanizmy ochrony**:

- **NAT/PAT** maskuje adresację wewnętrzną (dodatkowa warstwa *obscurity*) – VPN wymaga **NAT-T** (IPsec) lub działa przez TLS,
- **zapora** egzekwuje politykę **po rozszyfrowaniu** ruchu VPN (ruch z tunelu ma być filtrowany jak każdy inny – strefa VPN) i **inspekcja ruchu szyfrowanego** (wykład, slajd 27) bywa ograniczona,
- **UTM/NGFW** integrują bramę VPN (IPsec, SSL) z IPS i AV (wykład, slajd 32).

## Zagrożenia i dobre praktyki

| Zagrożenie | Środek zaradczy |
| :--- | :--- |
| **luki w urządzeniach VPN (częsty cel exploitów)** | **szybkie aktualizacje**, ograniczenie ekspozycji interfejsu zarządzania, monitoring, **IPS** |
| **słabe PSK, brute force, credential stuffing** | **certyfikaty/EAP-TLS i MFA**, silne hasła, blokada, rate limiting |
| **przestarzałe algorytmy i protokoły** (PPTP, 3DES, DH 1024, IKEv1 aggressive) | **IKEv2, AES-GCM, SHA-2, DH ≥ 14/ECDH**, PFS |
| **przejęte urządzenie klienta** | **ocena postury (NAC)**, EDR, ograniczenie dostępu do niezbędnych zasobów |
| **split tunneling** | ryzyko obejścia kontroli – świadoma decyzja, DNS i filtrowanie, ZTNA |
| **zbyt szeroki dostęp po zalogowaniu** | **najmniejsze uprawnienia**, segmentacja (strefa VPN), polityki per użytkownik/grupa (ISE/AD) |
| **brak widoczności** | logowanie sesji VPN do **SIEM**, alerty (nietypowe lokalizacje) |
| **DoS na bramę VPN** | anty-DDoS, redundancja, limity |
| **VPN a zgodność** | szyfrowanie wymagane przez RODO/PCI DSS/NIS2; polityka dostępu zdalnego (kurs Cisco: *polityka zdalnego dostępu*) |
| **ryzyko VPN konsumenckiego** | zaufanie do dostawcy – nie dla ruchu firmowego |

Dobre praktyki: **MFA dla wszystkich użytkowników VPN**, **zakaz haseł wyłącznie**, krótkie czasy życia sesji, **przegląd kont**, HA bram, projekt adresacji unikający kolizji, dokumentacja, testy (skan portów, pentest bramy – temat 10), rozważenie **ZTNA** dla aplikacji.

## Podsumowanie

- **VPN** = szyfrowany tunel przez sieć publiczną: **poufność, integralność, uwierzytelnienie**; zastosowania: **remote access**, **site-to-site**, B2B, chmura hybrydowa, praca w niezaufanych sieciach.
- **Technologie:** **IPsec** (AH/ESP, tryby tunelowy/transportowy, IKEv2, SA, certyfikaty/PSK) – standard enterprise; **SSL/TLS VPN** (443, AnyConnect) – dostęp zdalny; **OpenVPN** (TLS, UDP 1194, TUN/TAP); **WireGuard** (Curve25519, ChaCha20-Poly1305, mały kod, wysoka wydajność); też L2TP/IPsec, GRE, MPLS VPN, SD-WAN, ZTNA; **PPTP – nie używać**.
- Współpraca z **zaporą i NAT** (NAT-T, polityka dla ruchu z tunelu); zagrożenia: podatności bram VPN, słabe uwierzytelnianie, przestarzała kryptografia, split tunneling – środki: aktualizacje, **MFA**, silne algorytmy, najmniejsze uprawnienia, monitoring.

---
[⬅️ Poprzedni temat](8_Adaptacyjne_urządzenia_zabezpieczające_i_ich_rola_w_sieciach_korporacyjnych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Etapy_i_metody_testowania_bezpieczeństwa_sieci_teleinformatycznych.md)