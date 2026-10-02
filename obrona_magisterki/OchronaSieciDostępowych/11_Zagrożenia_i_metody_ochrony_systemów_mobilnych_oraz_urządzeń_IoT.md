# Zagrożenia i metody ochrony systemów mobilnych oraz urządzeń IoT

> Część IoT oparta na wykładzie *Bezpieczeństwo Internetu Rzeczy (IoT)* (W10); część dotycząca urządzeń mobilnych na Module 4 kursu Cisco (MDM, zarządzanie poprawkami) i Module 2 (SIEM – postura MDM). Uzupełnienia – ***(uzupełnienie)***.

# Część A. Systemy mobilne (smartfony, tablety, laptopy)

## Specyfika

Urządzenia mobilne **nie są fizycznie kontrolowane w siedzibie organizacji**: łatwo je zgubić, ukraść lub naruszyć, co narażą dane i dostęp do sieci (kurs Cisco). Zasada: **administratorzy powinni zakładać, że wszystkie urządzenia mobilne są niezaufane**, dopóki nie zostaną odpowiednio zabezpieczone. Różnorodność urządzeń (Android, iOS, producenci, wersje) sprawia, że część jest z natury mniej bezpieczna. Zjawisko **BYOD** (własne urządzenia pracowników) zwiększa wyzwanie.

## Zagrożenia

| Zagrożenie | Opis |
| :--- | :--- |
| **Utrata/kradzież urządzenia** | dostęp do danych, sesji, kont, VPN |
| **Malware mobilne** | złośliwe aplikacje (spyware, trojany bankowe, ransomware, stalkerware), aplikacje spoza oficjalnych sklepów, sideloading |
| **Niezałatane systemy** | brak aktualizacji, urządzenia bez wsparcia producenta |
| **Niebezpieczne sieci** | otwarte hotspoty Wi-Fi, **evil twin**, MITM, SSL stripping |
| **Phishing mobilny** | smishing (SMS), vishing, fałszywe aplikacje i strony, ograniczona ocena URL na małym ekranie |
| **Nadmierne uprawnienia aplikacji** | dostęp do kontaktów, lokalizacji, mikrofonu, plików |
| **Wycieki danych** | synchronizacja z chmurą osobistą, kopiowanie danych firmowych, brak szyfrowania |
| **Jailbreak/root** | zdjęcie zabezpieczeń systemu |
| **Ataki na Bluetooth/NFC/USB** | BlueBorne, juice jacking, złośliwe ładowarki |
| **Słabe uwierzytelnianie** | brak blokady ekranu, słaby PIN |
| **Shadow IT i BYOD** | niezarządzane urządzenia w sieci firmowej; mieszanie danych prywatnych i służbowych |
| **Ataki na łańcuch dostaw aplikacji** | zainfekowane biblioteki, złośliwe aktualizacje |
| **Zagrożenia fizyczne** | podglądanie ekranu (*shoulder surfing*), manipulacja |

## Metody ochrony

### 1. MDM (Mobile Device Management) / EMM/UEM

Wg kursu Cisco: **MDM** umożliwia personelowi bezpieczeństwa **konfigurowanie, monitorowanie i aktualizowanie** zróżnicowanego zestawu klientów mobilnych **z chmury** (przykład: **Cisco Meraki Systems Manager**). Działania przy opuszczeniu urządzenia przez zaufaną stronę: **wyłączenie (zdalne zablokowanie/wymazanie)** utraconego urządzenia, **szyfrowanie danych** na urządzeniu i **silniejsze uwierzytelnianie**.

Typowe funkcje MDM *(uzupełnienie)*:

- **rejestracja (enrollment)** i inwentaryzacja urządzeń (zarządzanie aktywami),
- **wymuszanie polityk**: blokada ekranu, długość PIN, szyfrowanie, zakaz jailbreak/root, wersje systemu,
- **zdalne blokowanie i wymazywanie** (*remote wipe*, w BYOD – selektywne),
- **kontrola aplikacji** (biała/czarna lista, sklep firmowy, uprawnienia),
- **profile Wi-Fi/VPN/certyfikaty**,
- **kontenery/work profile** (rozdzielenie danych prywatnych i służbowych – Android Enterprise, iOS User Enrollment),
- **zarządzanie poprawkami** i aktualizacjami systemu,
- **raportowanie postury** (zgodność) do NAC/SIEM (kurs Cisco: SIEM używa informacji o zgodności z polityką MDM do oceny zdarzeń).

### 2. Zarządzanie poprawkami urządzeń mobilnych (kurs Cisco)

Podejście **oparte na agentach** jest **preferowane** dla urządzeń mobilnych (agent na urządzeniu raportuje i instaluje poprawki); **skanowanie bezagentowe** działa tylko w przeskanowanych segmentach sieci – problematyczne dla urządzeń mobilnych, które często są poza siecią firmową.

### 3. Kontrola dostępu do sieci

**NAC/802.1X** (certyfikat urządzenia, EAP-TLS), **ocena postury** przed dopuszczeniem do sieci, **kwarantanna**/VLAN dla niezgodnych, **oddzielna sieć gościnna/BYOD**, **ZTNA/VPN** dla dostępu zdalnego, **MFA**, dostęp warunkowy (urządzenie, lokalizacja, ryzyko).

### 4. Zabezpieczenia na urządzeniu

szyfrowanie pamięci (domyślne w nowych systemach), silna blokada (biometria + PIN), **aktualizacje**, tylko oficjalne sklepy, **ograniczanie uprawnień aplikacji**, antymalware/MTD (*Mobile Threat Defense*), ochrona przed phishingiem (filtry DNS), ustawienia prywatności, wyłączenie nieużywanych interfejsów (Bluetooth, NFC), „znajdź urządzenie".

### 5. Polityki i ludzie

**polityka BYOD / dopuszczalnego użytkowania**, zgody i regulaminy, szkolenia (phishing, otwarte Wi-Fi), procedura zgłaszania utraty, onboarding/offboarding.

# Część B. Urządzenia IoT

## Czym jest IoT i dlaczego jest wyzwaniem (wykład IoT)

**IoT** – sieć urządzeń fizycznych wyposażonych w sensory, oprogramowanie i łączność, wymieniających dane przez Internet (inteligentne domy, urządzenia medyczne, czujniki przemysłowe, transport). Prognoza wykładu: **30,9 mld urządzeń do 2025 r.** Urządzenia IoT **tworzą nową powierzchnię ataku**: **każde podłączone urządzenie jest potencjalną bramą do sieci domowej lub firmowej**; słabo zabezpieczone urządzenie → **penetracja** innych urządzeń → **kompromitacja** infrastruktury.

### Specyfika (wykład IoT)

| Cecha | Konsekwencja dla bezpieczeństwa |
| :--- | :--- |
| **Ograniczone zasoby** (moc, RAM/flash, bateria) | trudne zaawansowane mechanizmy kryptograficzne i zabezpieczenia znane z desktopów |
| **Brak standaryzacji** (producenci, systemy, protokoły) | każde urządzenie wymaga indywidualnego podejścia |
| **Długi cykl życia** (10–15 lat; w IIoT 20–30) | brak regularnych aktualizacji → kumulowanie podatności |
| **Brak interfejsu użytkownika** | trudne uwierzytelnianie, konfiguracja, zarządzanie tożsamością |
| **Skala** (miliardy urządzeń) | wymagana automatyzacja zarządzania |
| **Fizyczny dostęp** | manipulacje, porty debugowania |

## Zagrożenia (wykład IoT)

| Zagrożenie | Opis |
| :--- | :--- |
| **Nieautoryzowany dostęp** | słabe/domyślne hasła, niezałatane luki, **backdoory producentów** |
| **Naruszenia danych i prywatności** | wycieki danych osobistych, medycznych, finansowych; malware zamieniające urządzenia w narzędzia szpiegowskie; roboty sprzątające mapujące wnętrza domów |
| **Botnety IoT i DDoS** | przejmowanie tysięcy/milionów urządzeń do koordynowanych ataków; **Mirai (2016)** – skan w poszukiwaniu urządzeń z domyślnymi hasłami (kamery IP, routery), >600 000 urządzeń, atak DDoS na dostawcę DNS **Dyn** (niedostępne Twitter, Netflix, Reddit, GitHub); kod źródłowy upubliczniony → warianty |
| **Ataki na łańcuch dostaw** | kompromitacja przy produkcji, dystrybucji lub instalacji |
| **Ataki na komunikację** | podsłuch i MITM (dane jawne, słabe protokoły), podszywanie |
| **Ataki na aktualizacje** | niepodpisany firmware, brak OTA |
| **IIoT/SCADA** | ataki na PLC i czujniki (**Stuxnet 2010**), *false data injection* → szkody fizyczne, ekologiczne; protokoły bez szyfrowania (Modbus, OPC) |
| **Urządzenia medyczne** | pompy insulinowe (2018), rozruszniki serca (2017, wycofano 500 tys. urządzeń) – zagrożenie życia |
| **Prywatność/nadzór** | profilowanie, śledzenie lokalizacji, podsłuch asystentów głosowych; przypadek **pralki przesyłającej 4 GB danych dziennie** (2020) – błąd oprogramowania, ale pokazuje ryzyko eksfiltracji |

### OWASP IoT Top 10 (wykład IoT)

1. nieaktualne oprogramowanie i firmware, 2. słabe uwierzytelnianie i autoryzacja (domyślne hasła), 3. niewłaściwe szyfrowanie danych, 4. brak ochrony prywatności, 5. ataki na łańcuch dostaw, 6. niezabezpieczone interfejsy sieciowe (otwarte porty, brak segmentacji), 7. brak bezpiecznej konfiguracji, 8. niewystarczające logowanie i monitorowanie, 9. niezabezpieczone komponenty i zależności, 10. niewystarczające zabezpieczenia fizyczne (porty debugowania, brak ochrony przed manipulacją).

**Przyczyny (diagram Ishikawy, wykład):** czynniki ludzkie (domyślne hasła, brak świadomości), organizacyjne (presja czasu, brak audytów i budżetów), technologiczne (podatności protokołów, błędy szyfrowania), środowiskowe (dostęp fizyczny).

## Metody ochrony IoT

### 1. Na urządzeniu i na „brzegu" (edge)

- **Edge security** – zabezpieczanie urządzenia w miejscu jego lokalizacji, zanim dane dotrą do chmury: **weryfikacja integralności** (sumy kontrolne, podpisy), **lekka kryptografia** (ChaCha20-Poly1305, AES-128, ASCON; hash BLAKE2s; ECC-256 zamiast RSA), **wzajemne uwierzytelnianie urządzeń** (X.509, tokeny, challenge-response).
- **Trusted Computing**: **TPM**, **TEE** (np. ARM TrustZone, Intel SGX) – bezpieczne przechowywanie kluczy, **secure boot**, **measured boot**, atestacja zdalna, izolacja procesów; framework **IoTrust** dla tanich urządzeń; wyzwania: koszt (do 20–30% wartości urządzenia), energia, skalowalność poświadczeń, cykl życia, zagrożenia postkwantowe.
- **Utwardzanie:** zmiana domyślnych haseł, wyłączenie zbędnych usług (Telnet, UPnP), minimalna konfiguracja, zabezpieczenia fizyczne (zalane porty debug, ochrona przed manipulacją).

### 2. Bezpieczna komunikacja

- protokoły: **MQTT z TLS** (wymaga ≥ ok. 10 KB RAM; MQTT-SN dla bardzo ograniczonych), **CoAP z DTLS** (10× mniejszy narzut; OSCORE), **ZigBee** (AES-128, zarządzanie kluczami), **Bluetooth LE** (LE Secure Connections, poprawne parowanie), **LoRaWAN**; **TLS 1.3 / DTLS 1.2+**, **WPA3** (SAE zamiast PSK) dla Wi-Fi,
- **szyfrowanie end-to-end, uwierzytelnianie, integralność**; dystrybucja kluczy (PSK, EDHOC).

### 3. Tożsamość i dostęp (IAM w IoT)

uwierzytelnianie urządzeń (certyfikaty X.509, tokeny, klucze), uwierzytelnianie użytkowników (MFA), autoryzacja (**RBAC/ABAC**), zarządzanie cyklem życia (provisioning, rotacja kluczy, decommissioning), polityki dostępu zależne od kontekstu, audyt; usługi: AWS IoT Core, Azure IoT Hub DPS.

### 4. Sieć – segmentacja i monitoring (rekomendacje wykładu dla administratorów)

- **segmentacja:** **VLAN dla IoT** oddzielone od sieci korporacyjnej i krytycznych systemów, **mikrosegmentacja i Zero Trust**, **NAC z 802.1X**, firewall ograniczający komunikację IoT do minimum, **DMZ** dla urządzeń wymagających Internetu, **osobna sieć zarządzania**,
- **monitoring:** **IDS/IPS** specjalizowane dla protokołów IoT (MQTT, CoAP), **NetFlow/sFlow**, **baseline** normalnego zachowania dla każdego typu urządzeń, **SIEM**, alerty o **nowych urządzeniach, nietypowych destynacjach i skokach transferu**, **honeypoty** wykrywające rekonesans,
- w domu: osobna sieć gościnna/Wi-Fi dla IoT, wyłączenie WPS i UPnP, aktualizacja routera, aplikacje typu **Fing/GlassWire** do przeglądu urządzeń.

### 5. Aktualizacje i cykl życia (wykład IoT)

- bezpieczne **OTA**: **podpisy cyfrowe** firmware, szyfrowanie, **rollback**, **A/B partitioning**, **delta updates**, **staged rollout**,
- problemy: skala, różnorodność sprzętu, urządzenia offline, ryzyko *brick*,
- **best practices:** automatyczne aktualizacje domyślnie, **min. 5 lat wsparcia** producenta, emergency patching, **polityka end-of-life** (wycofywanie urządzeń bez wsparcia), inwentaryzacja.

### 6. Audyt i zarządzanie ryzykiem

kwartalne skany podatności, **roczne testy penetracyjne** (red team), zgodność z baseline (CIS, NIST), przegląd reguł zapory co 6 miesięcy, plany reagowania specyficzne dla IoT, ćwiczenia (*tabletop*), klasyfikacja systemów wg krytyczności (tier 1–3 – studium inteligentnego miasta), **macierzowy model bezpieczeństwa** (macierz dopuszczalnych oddziaływań – wykład).

### 7. Rola AI i chmury/edge

ML (autoenkodery, isolation forest) wykrywa anomalie zachowania urządzeń, **SOAR** automatycznie izoluje zainfekowane urządzenia; **edge** – dane wrażliwe przetwarzane lokalnie (prywatność, działanie offline), **chmura** – analityka, centralne zarządzanie; hybryda.

### 8. Prywatność i regulacje (wykład IoT)

**Anonimizacja, pseudonimizacja, minimalizacja danych, przetwarzanie lokalne, szyfrowanie E2E, differential privacy**; **RODO** (informowanie, prawo do usunięcia, zgody, zgłaszanie naruszeń w 72 h). Standardy: **ETSI EN 303 645** (13 wymagań bazowych: brak domyślnych haseł, polityka zgłaszania luk, aktualizacje, szyfrowanie), **EU Cyber Resilience Act** (security by design, 5 lat wsparcia, zgłaszanie wykorzystywanych luk w 24 h, znak CE; kary do 15 mln € lub 2,5% obrotu), **UK PSTI**, **NIST IoT**; współpraca: ENISA, IoT Security Foundation, ISAC, bug bounty.

### Rekomendacje dla użytkowników końcowych (wykład IoT)

automatyczne aktualizacje; **natychmiastowa zmiana domyślnych haseł** (min. 12 znaków, menedżer haseł, 2FA); **oddzielna sieć dla IoT** (guest/VLAN); regularne sprawdzanie listy urządzeń i uprawnień aplikacji; wyłączenie zbędnych funkcji (zdalny dostęp, UPnP); przed zakupem – historia aktualizacji i polityka producenta.

# Część C. Porównanie i wspólne zasady

| Aspekt | **Systemy mobilne** | **Urządzenia IoT** |
| :--- | :--- | :--- |
| Zasoby | duże (pełny OS) | ograniczone |
| Aktualizacje | regularne (jeśli wspierane), MDM | często brak, długi cykl życia |
| Interfejs użytkownika | jest (PIN, biometria) | brak lub minimalny |
| Główne zagrożenia | utrata, malware, phishing, niezaufane Wi-Fi | domyślne hasła, botnety, brak aktualizacji, podsłuch |
| Zarządzanie | **MDM/UEM**, agent | **IoT IAM**, provisioning, OTA |
| Dostęp do sieci | NAC + postura, BYOD sieć | **VLAN/segmentacja**, NAC (MAB), minimalne reguły zapory |
| Monitoring | postura (SIEM), MTD | baseline, IDS dla protokołów IoT, NetFlow |
| Prawo | RODO (BYOD, dane służbowe) | RODO, ETSI 303 645, CRA |

## Zasady wspólne dla ochrony systemów mobilnych i IoT w sieci dostępowej

1. **Zasada zerowego zaufania** – urządzenie niezaufane, dopóki nie zweryfikowane.
2. **Inwentaryzacja** (zarządzanie aktywami) i identyfikacja nieautoryzowanych urządzeń.
3. **Segmentacja** (VLAN dla BYOD, IoT, gości) i kontrola dostępu (802.1X/NAC, MAB, postura).
4. **Aktualizacje i zarządzanie poprawkami** (agent/OTA, polityka end-of-life).
5. **Silne uwierzytelnianie** (zmiana domyślnych haseł, certyfikaty, MFA).
6. **Szyfrowanie** (urządzenie, transmisja: TLS/DTLS, WPA3).
7. **Monitoring i wykrywanie anomalii** (NetFlow, IDS, SIEM, baseline).
8. **Polityki, szkolenia, regulacje** (BYOD, ETSI/CRA, RODO).

## Podsumowanie

- **Systemy mobilne:** zagrożenia – utrata/kradzież, malware, niezaufane sieci, phishing, BYOD; ochrona – **MDM** (konfiguracja, monitoring, aktualizacje, zdalne wymazanie, szyfrowanie), **NAC z oceną postury**, patching agentowy, polityki BYOD, MTD.
- **IoT:** zagrożenia – ograniczone zasoby, domyślne hasła, brak aktualizacji, **botnety (Mirai)**, prywatność, IIoT/medyczne; ochrona – **lekka kryptografia, TPM/TEE i secure boot, uwierzytelnianie urządzeń, bezpieczne protokoły (TLS/DTLS, WPA3), segmentacja VLAN i Zero Trust, monitoring i IDS, bezpieczne OTA, polityka end-of-life**, regulacje (ETSI 303 645, CRA, RODO).
