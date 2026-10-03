# Zastosowanie sieci VPN w bezpiecznej komunikacji oraz wybrane technologie VPN

**VPN (Virtual Private Network)** to **szyfrowany tunel** zestawiany przez sieć publiczną (Internet) między punktami, który zapewnia bezpieczną komunikację, jakby były połączone prywatnym łączem.

**Usługi bezpieczeństwa VPN:**

- **poufność:** szyfrowanie (AES-GCM, ChaCha20),
- **integralność** i **ochrona przed powtórzeniem** (anti-replay),
- **uwierzytelnianie** stron (certyfikaty, klucze, PSK, MFA),
- **enkapsulacja:** pakiet wewnętrzny jest opakowany w zewnętrzny.

**Zastosowania:**

- **dostęp zdalny pracowników (remote access)** do zasobów firmy,
- **połączenie oddziałów (site-to-site)** zamiast łączy dzierżawionych,
- komunikacja z partnerami i dostawcami (B2B),
- **chmura hybrydowa:** bezpieczne łącze do VPC/VNet,
- praca w niezaufanych sieciach (hotspoty),
- zdalna administracja urządzeniami.

**Rodzaje:** remote access (client-to-site), site-to-site, host-to-host, **full tunnel** (cały ruch przez VPN) i **split tunnel** (tylko ruch do zasobów firmowych, mniejsze obciążenie, ale ryzyko obejścia kontroli), VPN dostawcy (MPLS L3VPN, bez szyfrowania) oraz overlay SD-WAN.

**Wybrane technologie:**

- **IPsec** (warstwa 3), standard dla site-to-site i enterprise:
  - **ESP** (szyfrowanie, integralność, uwierzytelnienie), AH (tylko uwierzytelnienie, rzadko),
  - tryb **tunelowy** (cały pakiet w nowym) i **transportowy** (tylko ładunek),
  - **IKE** (negocjacja i wymiana kluczy; IKEv2 jest zalecany, UDP 500/4500 i NAT-T), **SA** (powiązanie bezpieczeństwa), **PFS**,
  - uwierzytelnianie **certyfikatami** lub PSK.
- **SSL/TLS VPN** (np. Cisco AnyConnect): przez port 443, łatwo przechodzi przez zapory, dostęp granularny do aplikacji. Typowy wybór dla zdalnego dostępu.
- **OpenVPN:** open source, oparty na TLS, UDP 1194 lub TCP, tryby TUN (L3) i TAP (L2), elastyczny, ale wolniejszy.
- **WireGuard:** nowoczesny i minimalistyczny (ok. 4 tys. linii kodu), **Curve25519, ChaCha20-Poly1305**, uwierzytelnianie kluczami publicznymi, szybki i prosty (mobilne i IoT), mniej funkcji enterprise.
- Inne: **L2TP/IPsec**, SSTP, GRE (bez szyfrowania, zwykle z IPsec), **MPLS VPN** (izolacja bez szyfrowania). **PPTP jest złamany i nie należy go używać.**
- Alternatywa dla VPN: **ZTNA** (dostęp do konkretnych aplikacji po weryfikacji tożsamości i postury).

**Współpraca z zaporą i NAT:** ruch z tunelu powinien przechodzić przez zaporę jak każdy inny (osobna strefa VPN), IPsec wymaga NAT-T, a NGFW/UTM często zawierają bramę VPN.

**Zagrożenia i dobre praktyki:**

- **luki w bramach VPN** (częsty cel ataków): szybkie aktualizacje, ograniczenie ekspozycji zarządzania, IPS,
- słabe hasła i PSK: **certyfikaty, MFA**, blokady,
- przestarzałe algorytmy (PPTP, 3DES, słabe grupy DH): **IKEv2, AES-GCM, SHA-2, PFS**,
- przejęte urządzenie klienta: ocena postury (NAC), EDR,
- zbyt szeroki dostęp po zalogowaniu: **najmniejsze uprawnienia**, segmentacja,
- brak widoczności: logi do SIEM.

**Wniosek:** do site-to-site zwykle wybiera się IPsec, do zdalnego dostępu SSL VPN lub WireGuard, a niezależnie od technologii liczą się silne uwierzytelnianie (MFA), nowoczesna kryptografia i kontrola dostępu po zalogowaniu.

## Podsumowanie

- **VPN** = szyfrowany tunel przez sieć publiczną: **poufność, integralność, uwierzytelnienie**; zastosowania: **remote access**, **site-to-site**, B2B, chmura hybrydowa, praca w niezaufanych sieciach.
- **Technologie:** **IPsec** (AH/ESP, tryby tunelowy/transportowy, IKEv2, SA, certyfikaty/PSK) – standard enterprise; **SSL/TLS VPN** (443, AnyConnect) – dostęp zdalny; **OpenVPN** (TLS, UDP 1194, TUN/TAP); **WireGuard** (Curve25519, ChaCha20-Poly1305, mały kod, wysoka wydajność); też L2TP/IPsec, GRE, MPLS VPN, SD-WAN, ZTNA; **PPTP – nie używać**.
- Współpraca z **zaporą i NAT** (NAT-T, polityka dla ruchu z tunelu); zagrożenia: podatności bram VPN, słabe uwierzytelnianie, przestarzała kryptografia, split tunneling – środki: aktualizacje, **MFA**, silne algorytmy, najmniejsze uprawnienia, monitoring.

---
[⬅️ Poprzedni temat](8_Adaptacyjne_urządzenia_zabezpieczające_i_ich_rola_w_sieciach_korporacyjnych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_Etapy_i_metody_testowania_bezpieczeństwa_sieci_teleinformatycznych.md)