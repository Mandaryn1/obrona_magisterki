# Segmentacja sieci, minimalizacja uprawnień, inspekcja ruchu i kontrola dostępu w ograniczaniu skutków ataków

**Idea:** żadne zabezpieczenie nie zatrzyma wszystkich ataków, więc trzeba **ograniczać skutki udanego włamania**, czyli zmniejszać tzw. **promień rażenia**. Cztery mechanizmy z tytułu działają razem w ramach **obrony w głąb** i podejścia **Zero Trust**.

**1. Segmentacja sieci** dzieli sieć na odseparowane strefy (VLAN, podsieci, DMZ), między którymi ruch kontroluje zapora. Strefy to np. użytkownicy, serwery aplikacji, bazy danych, zarządzanie, goście, IoT. Skutek: atakujący, który przejmie jeden host, **nie ma bezpośredniego dostępu do reszty sieci** i trudniej mu o ruch boczny. Przykład: zainfekowana stacja nie dotrze do bazy danych. **Mikrosegmentacja** robi to na poziomie pojedynczych serwerów lub aplikacji (Security Groups, polityki sieciowe w Kubernetes).

**2. Minimalizacja uprawnień (zasada najmniejszych uprawnień)** oznacza, że użytkownik, proces i konto usługowe mają **tylko te uprawnienia, których potrzebują**. Stosuje się to przez role (RBAC), osobne konta administracyjne, **PAM** (zarządzanie dostępem uprzywilejowanym) i dostęp **just-in-time**, a także przeglądy uprawnień. Skutek: przejęte konto ma ograniczone możliwości, a szkodliwe oprogramowanie dziedziczy niskie uprawnienia.

**3. Inspekcja ruchu** kontroluje, co faktycznie przepływa: filtracja pakietów, **zapora stanowa**, **zapora aplikacyjna/WAF**, **NGFW** (kontrola aplikacji, antymalware, inspekcja TLS), **IDS/IPS**, a w komunikacji usług **mTLS**. Skutek: znane ataki, malware i nieautoryzowane protokoły są wykrywane lub blokowane na granicach stref.

**4. Kontrola dostępu** (uwierzytelnianie, MFA, autoryzacja, ACL, 802.1X/NAC) sprawdza, **kto i co** ma dostęp do zasobu, także wewnątrz sieci, a nie tylko na jej granicy.

Te mechanizmy nie zapobiegają samemu włamaniu, ale sprawiają, że ma ono **ograniczony zasięg i skutki**, a atakujący jest wykrywany wcześniej.

## 2. Minimalizacja uprawnień (zasada najmniejszych uprawnień)

**Least privilege**: każdy użytkownik, proces i aplikacja ma dostęp **wyłącznie do zasobów niezbędnych** do zadań. Wdrożenie: precyzyjna identyfikacja potrzeb, **regularne przeglądy uprawnień**, **audyt aktywności** i wykrywanie anomalii.

| Obszar | Przykłady |
| :--- | :--- |
| **konta użytkowników** | zwykłe konta zamiast administratora; **PAM** dla kont uprzywilejowanych; Linux: `sudo` zamiast root, wyłączone logowanie root |
| **procesy i usługi** | konta usługowe o minimalnych uprawnieniach, SELinux/AppArmor, kontenery nie jako root |
| **sieć (zapory)** | tylko niezbędne porty i adresy (default deny) |
| **dane i bazy** | RBAC, ACL, osobne konta dla usług, `GRANT` minimalne |
| **urządzenia sieciowe** | uprawnienia administracyjne wg ról (TACACS+/RADIUS), poziomy uprawnień |
| **Zero Trust** | **just-in-time / just-enough access**, ciągła weryfikacja, kontekstowa autoryzacja |

**Efekt:** przejęte konto lub usługa daje ograniczone szkody (nie ma „kluczy do królestwa"), utrudniona **eskalacja uprawnień**.

## 3. Inspekcja ruchu

**Inspekcja** – analiza ruchu przechodzącego między strefami i na brzegu w celu **wykrycia lub zablokowania** zagrożeń.

| Mechanizm | Zakres |
| :--- | :--- |
| **Filtracja pakietów** | adresy IP, porty, protokół, flagi TCP (L3–4) – szybka, bez analizy treści |
| **Zapory stanowe** | tabela stanów, dynamiczne reguły dla odpowiedzi (iptables/conntrack, Cisco ASA) |
| **Zapory aplikacyjne/WAF** | inspekcja HTTP/HTTPS, FTP, SMTP, DNS; SQLi, XSS, command injection, directory traversal |
| **NGFW** | zapora + **IPS**, kontrola aplikacji niezależnie od portu, antymalware, **inspekcja SSL/TLS** |
| **IDS/IPS** | wykrywanie/blokowanie (sygnatury, anomalie); inline IPS, NIDS na SPAN/TAP |
| **Inspekcja ruchu wschód–zachód** | kontrola ruchu **wewnątrz** sieci, nie tylko na perymetrze |
| **UTM** | wiele funkcji w jednym urządzeniu (zapora, IPS, AV, VPN, filtrowanie WWW, DLP) |
| **Proxy, filtrowanie DNS/URL, sandboxing** | blokada złośliwych domen i plików |

**Ograniczenia:** szyfrowanie (potrzebna inspekcja TLS z zachowaniem prawa/prywatności), wydajność (DPI → opóźnienia), SPOF.

## 4. Kontrola dostępu

Kto i do czego ma dostęp – na kilku poziomach:

- **sieć:** 802.1X/NAC, VPN z MFA, ACL, reguły zapory,
- **system:** ACL, RBAC, MAC (SELinux), polityki GPO,
- **aplikacja:** uwierzytelnianie, autoryzacja, sesje, tokeny,
- **dane:** szyfrowanie, klasyfikacja, DLP.

## Podsumowanie

- Cztery uzupełniające się mechanizmy: **segmentacja** (gdzie można dotrzeć), **minimalne uprawnienia** (co można zrobić), **inspekcja** (co płynie), **kontrola dostępu** (kto wchodzi).
- Wspólna zasada: **domyślna odmowa i biała lista** + założenie naruszenia (**Zero Trust**) → ograniczenie ruchu bocznego i skali szkód.
- Wdrożenie: strefy (DMZ, aplikacje, dane, zarządzanie), mikrosegmentacja, least privilege/PAM, zapory stanowe i NGFW/IPS, 802.1X i MFA, monitoring.

---
[⬅️ Poprzedni temat](9_Metody_zapobiegania_nieuprawnionemu_dostępowi_do_zasobów_sprzętowych_i_programowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](11_Podstawowe_działania_administratora_w_zakresie_zabezpieczania_infrastruktury_sieciowej.md)