# Klasyfikacja najważniejszych rodzajów ataków sieciowych

Ataki sieciowe można klasyfikować na kilka sposobów.

**1. Według naruszanego celu (triada CIA):**

- **ataki na dostępność:** DoS i DDoS (SYN flood, amplifikacja UDP, ICMP flood),
- **ataki na poufność:** podsłuch (sniffing), przechwycenie danych, MITM, rozpoznanie,
- **ataki na integralność:** modyfikacja danych w tranzycie, wstrzykiwanie kodu, spoofing.

**2. Według sposobu działania:**

- **pasywne:** obserwacja i podsłuch bez zmiany danych. Są trudne do wykrycia (sniffing, analiza ruchu, rozpoznanie pasywne),
- **aktywne:** ingerencja w sieć lub dane (skanowanie, spoofing, DoS, MITM, wstrzykiwanie).

**3. Według warstwy modelu OSI:**

- **L2:** MAC flooding, ARP spoofing, rogue DHCP, ataki na STP, VLAN hopping,
- **L3:** IP spoofing, ICMP flood, smurf, ataki na routing,
- **L4:** SYN flood, skanowanie portów, przejęcie sesji,
- **L7:** SQL Injection, XSS, DNS poisoning, ataki na HTTP.

**4. Według techniki:**

- **malware** (wirusy, robaki, trojany, ransomware, botnety),
- **ataki na hasła** (brute force, słownikowe, credential stuffing),
- **inżynieria społeczna** (phishing, spear phishing, vishing),
- **spoofing i MITM**,
- **DoS/DDoS**,
- **exploity podatności**, w tym zero-day.

**5. Według etapu ataku (APT):** rozpoznanie → pierwsze naruszenie → utrwalenie → eskalacja uprawnień → ruch boczny → eksfiltracja danych.

**6. Według sprawcy:** przestępczość zorganizowana, hacktywiści, podmioty państwowe, insiderzy.

**Przykłady historyczne:** WannaCry i NotPetya (robaki i ransomware), Mirai (botnet IoT, DDoS), Emotet i Zeus (malware bankowy).

Klasyfikacje się przenikają, bo ten sam atak można opisać jednocześnie według celu, warstwy i techniki. Pomagają dobrać odpowiednie zabezpieczenia.

## Podsumowanie

- Podstawowa klasyfikacja (wykład): ataki na **dostępność** (DoS/DDoS, flood), **poufność** (sniffing, MITM, podsłuch), **integralność** (IP spoofing, DNS i ARP poisoning).
- Dodatkowo: pasywne vs aktywne, według warstwy OSI, według etapu ataku (APT), według sprawcy, według techniki (malware, socjotechnika, hasła, aplikacje webowe).
- Znajomość klasyfikacji pozwala dobrać **warstwową obronę** i narzędzia detekcji.

---
[⬅️ Poprzedni temat](3_Analiza_protokołów_i_usług_sieciowych_w_ocenie_bezpieczeństwa.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Metody_detekcji_i_obrony_przed_atakami_w_warstwie_II_modelu_OSI.md)