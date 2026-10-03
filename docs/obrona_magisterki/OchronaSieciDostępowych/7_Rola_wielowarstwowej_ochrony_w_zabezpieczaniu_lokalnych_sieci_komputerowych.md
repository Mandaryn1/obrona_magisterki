# Rola wielowarstwowej ochrony w zabezpieczaniu lokalnych sieci komputerowych

**Wielowarstwowa ochrona (defense in depth, obrona w głąb)** polega na stosowaniu **wielu niezależnych warstw zabezpieczeń**, tak aby przełamanie jednej nie dawało atakującemu pełnego dostępu. Wynika z założenia, że **żadne pojedyncze zabezpieczenie nie jest doskonałe**, a ataki będą się czasem udawać.

**Warstwy (od zewnątrz do danych):**

- **polityki i ludzie:** polityki bezpieczeństwa, szkolenia, procedury,
- **ochrona fizyczna:** kontrola dostępu do pomieszczeń i sprzętu,
- **perymetr:** zapory, IPS, VPN, DMZ,
- **sieć i segmentacja:** VLAN-y, podział na strefy, kontrola ruchu między nimi,
- **dostęp do sieci (L2):** 802.1X/NAC, port security, DHCP snooping, DAI,
- **host:** aktualizacje, antywirus/EDR, zapora hostowa, utwardzanie,
- **aplikacje:** bezpieczny kod, WAF,
- **dane:** szyfrowanie, kontrola dostępu, kopie zapasowe,

oraz **przekrojowo:** tożsamość (MFA, najmniejsze uprawnienia) i **monitoring** (logi, IDS, SIEM).

**Rola i znaczenie:**

- **Odporność na awarię jednej warstwy:** błąd konfiguracji lub nowa podatność w jednym mechanizmie nie przesądza o kompromitacji.
- **Różne warstwy zatrzymują różne etapy ataku:** na przykład filtry poczty i szkolenia ograniczają phishing, MFA przejęcie konta, segmentacja ruch boczny, a szyfrowanie i monitoring utrudniają eksfiltrację.
- **Spowolnienie atakującego i zwiększenie szans wykrycia:** każda przeszkoda daje czas na reakcję.
- **Ograniczenie skutków** udanego ataku (mniejszy promień rażenia).
- **Zgodność z zaleceniami:** podejście zalecają standardy, np. CIS Controls (kontrole podstawowe, fundamentalne, organizacyjne) i ISO 27001.

**Zasady:** warstwy powinny być **niezależne** (różne mechanizmy, ideal: różni dostawcy), uzupełniać się i być spójnie zarządzane.

**Ograniczenia:** większy koszt i złożoność zarządzania, ryzyko błędów konfiguracji i **fałszywe poczucie bezpieczeństwa**. Dlatego warstwy trzeba regularnie testować i aktualizować.

**Wniosek:** ochrona wielowarstwowa nie eliminuje ryzyka, ale znacząco je ogranicza i zwiększa szanse na wykrycie ataku, zanim wyrządzi poważne szkody.

### Rodzaje kontroli w każdej warstwie

- **zapobiegawcze** (preventive) – zapora, 802.1X, MFA, szyfrowanie,
- **wykrywające** (detective) – IDS, logi, SIEM, monitoring, audyt,
- **korygujące/odtwarzające** (corrective) – łatanie, backup, izolacja, plan awaryjny,
- **odstraszające/kompensacyjne** – ostrzeżenia, monitoring kamer, dodatkowe kontrole zastępcze.

Z innej perspektywy: **administracyjne** (polityki), **techniczne**, **fizyczne**.

### Dlaczego wielowarstwowość jest niezbędna w LAN

1. **Ataki są wieloetapowe** (kill chain/APT: rozpoznanie → wejście → utrwalenie → eskalacja → ruch boczny → eksfiltracja) – różne warstwy zatrzymują różne etapy.
2. **Perymetr nie wystarcza** – urządzenia mobilne, VPN, Wi-Fi, phishing i insiderzy omijają zaporę; zagrożenie **wewnątrz** sieci wymaga kontroli L2/L3, segmentacji, hostowej ochrony.
3. **Ograniczanie skutków** – segmentacja i least privilege zmniejszają „promień rażenia" naruszenia.
4. **Redundancja kontroli** – błąd konfiguracji jednej (np. zapory) nie oznacza kompromitacji całości.
5. **Różne klasy zagrożeń** – sieciowe, aplikacyjne, ludzkie, fizyczne – wymagają różnych środków.
6. **Zgodność z normami** (ISO 27001, NIS2, RODO) oczekuje podejścia warstwowego.
7. **Wykrywanie i reakcja** – nawet przy najlepszej prewencji trzeba mieć monitoring (wykład ryzyko: *pięć filarów* – identyfikacja ryzyka, środki minimalizujące, polityki, **monitoring i audyty**, edukacja).

## Podsumowanie

- **Ochrona wielowarstwowa (defense in depth)** – wiele niezależnych warstw kontroli; awaria jednej nie oznacza kompromitacji całości.
- Warstwy: **polityki/ludzie → fizyczna → perymetr → sieć wewnętrzna (segmentacja) → dostęp (802.1X, port security) → host → aplikacje → dane**, plus **tożsamość** i **monitoring/reagowanie** przekrojowo.
- Zatrzymuje wieloetapowe ataki, ogranicza ruch boczny i skutki, kompensuje błędy pojedynczych zabezpieczeń; wymaga równowagi kosztu, wydajności i złożoności.

---
[⬅️ Poprzedni temat](6_Zagrożenia_komunikacji_bezprzewodowej_i_sposoby_jej_zabezpieczania.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Audyt_bezpieczeństwa_testy_penetracyjne_i_ocena_podatności.md)