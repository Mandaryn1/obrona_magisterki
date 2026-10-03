# Ataki polegające na rozpoznaniu, uzyskaniu dostępu oraz inżynierii społecznej

Ataki te to początkowe etapy cyklu życia ataku: **rozpoznanie → uzyskanie dostępu → utrwalenie i eskalacja → ruch boczny → cel** (eksfiltracja lub szyfrowanie). Inżynieria społeczna może wspierać pierwsze dwa etapy.

**1. Rozpoznanie (reconnaissance)** to zbieranie informacji o celu i **pierwszy krok każdego ataku**.

- **Pasywne:** bez bezpośredniego kontaktu z celem, więc praktycznie niewykrywalne. Metody: **DNS i Whois**, **OSINT** (media społecznościowe, oferty pracy, repozytoria kodu), certyfikaty i Certificate Transparency (crt.sh), wyciekłe hasła, metadane plików, **Google dorks**, **Shodan**, Recon-ng.
- **Aktywne:** wysyłanie sond do celu, więc **wykrywalne** przez IDS i logi. Metody: skanowanie portów (**Nmap**: SYN, connect, UDP), enumeracja hostów, użytkowników, grup, udziałów i usług.
- **Obrona:** zapory z domyślną odmową, IDS/IPS (wykrywa skanowanie), minimalna ekspozycja usług, czyszczenie metadanych, kontrola informacji publicznych, honeypoty.

**2. Uzyskanie dostępu** to wejście do sieci lub systemu.

- **Exploity podatności** (EternalBlue, Heartbleed, luki w VPN i serwerach, zero-day).
- **Ataki na hasła:** brute force, słownikowe, password spraying, **credential stuffing** (hasła z wycieków), domyślne hasła.
- **Ataki na aplikacje webowe** (SQL Injection, XSS) oraz **przejęcie ruchu i sesji** (MITM, ARP spoofing).
- **Dostęp bezprzewodowy** (rogue AP, łamanie WPA), **fizyczny** (podpięcie urządzenia, USB), **łańcuch dostaw**, przejęte konta (brak MFA).
- **Obrona:** aktualizacje, **MFA**, silne hasła i blokady, WAF, szyfrowanie, segmentacja i najmniejsze uprawnienia (ogranicza ruch boczny), monitoring i SIEM.

**3. Inżynieria społeczna** to **psychologiczna manipulacja** w celu wyłudzenia danych lub dostępu przez wykorzystanie ludzkich słabości. Większość włamań zaczyna się właśnie od niej.

- **Phishing:** masowe wiadomości podszywające się pod instytucje.
- **Spear phishing:** ukierunkowany na konkretną osobę (wykorzystuje dane z rozpoznania).
- **Whaling:** ataki na kadrę kierowniczą.
- **Vishing** (telefon), **smishing** (SMS).
- **Pretexting:** fałszywy scenariusz (np. „jestem z działu IT").
- **Baiting:** zainfekowany pendrive jako przynęta.
- **Tailgating:** wejście do strefy za uprawnioną osobą.
- **BEC:** wyłudzenie przelewu przez podszycie się pod przełożonego lub dostawcę.
- **Obrona:** filtry poczty (**SPF, DKIM, DMARC**), sandboxing załączników, filtrowanie DNS, MFA odporne na phishing (FIDO2). Najważniejsze są **szkolenia, symulowane kampanie phishingowe**, procedury weryfikacji próśb (np. drugi kanał dla przelewów) i zachęcanie do zgłaszania incydentów.

**Przykład połączenia:** z LinkedIn i ofert pracy atakujący poznaje administratorów i używany VPN, wysyła spear phishing z linkiem do fałszywego portalu, loguje się skradzionymi poświadczeniami (brak MFA), skanuje sieć wewnętrzną i przechodzi do ruchu bocznego. Łańcuch przerywają MFA, szkolenia, filtry poczty, ograniczenie informacji publicznych oraz segmentacja z IDS.

## Podsumowanie

- **Rozpoznanie** (pierwszy krok): **pasywne** (DNS, Whois, OSINT, certyfikaty i CT/crt.sh, wycieki, metadane, Google dorks, Wayback, GitHub, Shodan, Recon-ng) i **aktywne** (skany portów Nmap: SYN, connect, UDP, FIN; enumeracja hostów, użytkowników, grup, udziałów, WWW, usług; Scapy); aktywne – wykrywalne przez IDS/IPS.
- **Uzyskanie dostępu:** exploity podatności, ataki na hasła, ataki aplikacyjne, MITM i przejęcie sesji, dostęp fizyczny i bezprzewodowy, łańcuch dostaw, przejęte konta; potem utrwalenie, eskalacja, ruch boczny.
- **Inżynieria społeczna:** phishing, spear phishing, whaling, vishing, smishing, pretexting, baiting, tailgating; obrona – filtry poczty (SPF/DKIM/DMARC), sandbox, filtrowanie DNS, **szkolenia i symulacje**.
- Obrona warstwowa: minimalna ekspozycja, MFA, aktualizacje, IPS/IDS, segmentacja, monitoring, świadomość użytkowników.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Standardy_i_dobre_praktyki_bezpieczeństwa_ISO_27001_NIS2_i_inne_normy.md)