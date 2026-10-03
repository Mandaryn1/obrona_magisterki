# Metody detekcji i obrony przed atakami w warstwie III modelu OSI

Warstwa III (sieciowa) odpowiada za adresowanie IP i routing. Ataki na nią często pochodzą z zewnątrz, a protokoły (IP, ICMP, protokoły routingu) pierwotnie nie miały uwierzytelniania.

**Ataki i obrona:**

- **IP spoofing (podszywanie adresu):** fałszywy adres źródłowy, używany do ominięcia filtrów lub w DDoS. Obrona: **uRPF** (Unicast Reverse Path Forwarding), **filtrowanie wejściowe i wyjściowe (BCP 38)**, blokada adresów prywatnych i bogon z Internetu, ACL.
- **Ataki ICMP:** ping flood, **smurf** (broadcast), Ping of Death, tunelowanie ICMP, ICMP redirect. Obrona: **ograniczenie szybkości (rate limiting)** i filtrowanie ICMP, wyłączenie `directed-broadcast`, `source-route` i przekierowań, zapora.
- **DDoS z amplifikacją i flooding:** wolumetryczne zalewanie łącza. Obrona: **RTBH (blackholing)**, **scrubbing** i anty-DDoS u operatora lub w chmurze, CDN, anycast, rate limiting.
- **Ataki na routing:** fałszywe ogłoszenia tras (BGP hijacking, RIP/OSPF poisoning), co powoduje przekierowanie ruchu. Obrona: **uwierzytelnianie protokołów routingu** (hasła/klucze OSPF, BGP MD5/TCP-AO), filtry prefiksów, max-prefix, **RPKI** dla BGP, interfejsy pasywne.
- **Fragmentacja i obejście zapór:** wymaga normalizacji i składania fragmentów w IPS/zaporze.
- **Ataki IPv6:** fałszywe Router Advertisement i Neighbor Discovery. Obrona: **RA Guard**, ND inspection.
- **Podsłuch i modyfikacja w tranzycie:** **IPsec/VPN** (szyfrowanie i integralność).

**Ogólne zabezpieczenia:** **zapory i listy ACL** (domyślna odmowa), segmentacja i VLAN-y z kontrolą routingu, **NAT** (dodatkowa warstwa, nie zabezpieczenie podstawowe), aktualizacje urządzeń sieciowych, wyłączanie zbędnych usług.

**Detekcja:**

- **NetFlow/sFlow:** nagły wzrost ruchu, nietypowe źródła i cele,
- **IDS/IPS** (Snort, Suricata): sygnatury skanowania, floodów i anomalii,
- **logi zapór i routerów** w **SIEM** (korelacja),
- **monitorowanie tras** i alerty o zmianach w routingu (np. nieoczekiwane prefiksy BGP),
- analiza w Wireshark: podejrzane pakiety ICMP, spoofowane adresy.

Skuteczna ochrona łączy filtrowanie na brzegu, zabezpieczenie routingu, kontrolę przepływów i monitoring z możliwością szybkiej reakcji.

## Podsumowanie

- Ataki L3: **IP spoofing**, ataki **ICMP** (flood, smurf, Ping of Death, tunelowanie), **DoS/DDoS i amplifikacja**, ataki na **routing** (fałszywe trasy, BGP hijacking), nadużycia fragmentacji.
- Obrona: **filtrowanie wejściowe/wyjściowe, uRPF, ACL, zapory stanowe, rate limiting ICMP, wyłączenie source routing/redirectów/directed broadcast, uwierzytelnianie routingu, RPKI, IPsec/VPN, scrubbing/CDN/RTBH**.
- Detekcja: **IDS/IPS, NetFlow, analiza pakietów, logi i SIEM**.

---
[⬅️ Poprzedni temat](5_Metody_detekcji_i_obrony_przed_atakami_w_warstwie_II_modelu_OSI.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](7_Narzędzia_do_identyfikacji_ataków_na_protokoły_i_usługi_sieciowe.md)