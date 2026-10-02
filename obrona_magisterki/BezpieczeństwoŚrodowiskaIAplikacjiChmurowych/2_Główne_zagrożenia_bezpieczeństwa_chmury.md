# Główne zagrożenia bezpieczeństwa chmury

## Zagrożenia wg wykładu

| Zagrożenie | Opis (wg wykładu) | Przykład / skutek |
| :--- | :--- | :--- |
| **Utrata danych** | w wyniku **błędnej konfiguracji**, ataku lub awarii | publiczny bucket S3, skasowany wolumen bez kopii, ransomware |
| **Przejęcie konta** (*account hijacking*) | prowadzi do nieautoryzowanego dostępu do wrażliwych danych i zasobów | phishing, wyciek kluczy API w repozytorium, brak MFA |
| **Nieautoryzowany dostęp** | wynika z błędów zarządzania tożsamością lub słabych mechanizmów uwierzytelniania; naruszenie **poufności, integralności i dostępności**; **eskalacja uprawnień** | zbyt szerokie role, nieaktualne uprawnienia byłych pracowników |
| **Ataki na interfejsy API** | wg wykładu **najbardziej niebezpieczne**, bo prowadzą do **masowych wycieków danych** | brak autoryzacji na poziomie obiektu, brak ograniczeń ruchu (rate limiting), wstrzykiwanie |
| **Złośliwe oprogramowanie** | wprowadzane przez **niebezpieczne obrazy kontenerów** lub nieautoryzowane aplikacje | obraz z publicznego rejestru z backdoorem, cryptominer |
| **Nadużycie usług chmurowych** | ze względu na **łatwość skalowania** może prowadzić do wysokich kosztów i naruszeń | przejęcie konta i uruchomienie setek maszyn do kopania kryptowalut, DDoS z własnych zasobów |

## Ryzyka zarządzania (slajd 36)

- **zależność od dostawcy** (*vendor lock-in*) – ryzyko dostępności usług i bezpieczeństwa danych,
- **utrata kontroli nad danymi** – ryzyko dla **poufności i integralności**,
- **nieautoryzowany dostęp** – ryzyko dla bezpieczeństwa i **zgodności (compliance)**.

Zarządzanie ryzykiem obejmuje **identyfikację, ocenę, leczenie i monitorowanie** ryzyk (frameworki: **COBIT 5**, **ISO 31000**).

## Uzupełnienie: inne typowe zagrożenia *(uzupełnienie)*

| Zagrożenie | Opis |
| :--- | :--- |
| **Błędna konfiguracja** | najczęstsza przyczyna incydentów w chmurze: otwarte porty, publiczne magazyny, domyślne hasła, zbyt szerokie reguły sieciowe |
| **Niewystarczające zarządzanie tożsamością i poświadczeniami** | brak MFA, stałe klucze dostępu, brak rotacji sekretów |
| **Ataki typu DoS/DDoS** | wyczerpanie zasobów lub budżetu (*Economic Denial of Sustainability*) |
| **Zagrożenia wewnętrzne (insider threat)** | pracownik lub administrator dostawcy |
| **Naruszenia izolacji w środowisku współdzielonym** | ucieczka z maszyny wirtualnej/kontenera, ataki przez kanały boczne |
| **Ataki na łańcuch dostaw** | skompromitowane biblioteki, obrazy, potoki CI/CD |
| **Niezgodność z regulacjami** | lokalizacja danych, RODO, brak audytu |
| **Brak widoczności i logowania** | atakujący pozostaje niewykryty |

Organizacja **Cloud Security Alliance (CSA)** publikuje cykliczne zestawienie *Top Threats to Cloud Computing* – dominują w nim: niedostateczne zarządzanie tożsamością, niebezpieczne API, błędne konfiguracje i brak architektury bezpieczeństwa chmury.

## Jak przeciwdziałać

| Zagrożenie | Środki zaradcze (powiązania z wykładem) |
| :--- | :--- |
| utrata danych | **backup** (zaszyfrowany, testowany), replikacja, **RPO/RTO**, wersjonowanie, blokada usuwania (tematy 10–11) |
| przejęcie konta | **MFA**, krótkie sesje i tokeny, brak stałych kluczy, monitoring logowań (tematy 6–7) |
| nieautoryzowany dostęp | **najmniejsze uprawnienia**, RBAC/ABAC, przeglądy dostępu, separacja obowiązków (tematy 6, 8) |
| ataki na API | uwierzytelnianie (OAuth 2.0/OIDC), autoryzacja, **rate limiting**, walidacja danych, WAF, monitoring (temat 15) |
| złośliwe oprogramowanie | **skanowanie obrazów**, minimalne obrazy bazowe, podpisy obrazów, runtime protection |
| nadużycie usług | **budżety i alerty kosztowe**, limity (quota), polityki tworzenia zasobów |
| błędna konfiguracja | IaC + skanowanie IaC, polityki (policy-as-code), CSPM |
| wyciek danych | **szyfrowanie** (temat 9), **DLP**, klasyfikacja danych, minimalizacja (temat 14) |
| brak widoczności | centralne logi, **SIEM**, alerty (slajdy 19, 31) |

## Podsumowanie

- Główne zagrożenia (wykład): **utrata danych, przejęcie konta, nieautoryzowany dostęp, ataki na API, złośliwe oprogramowanie, nadużycie usług**.
- Dla chmury charakterystyczne są **błędy konfiguracji** i **tożsamość** jako najsłabsze ogniwo.
- Ryzyka zarządzania: zależność od dostawcy, utrata kontroli nad danymi, nieautoryzowany dostęp.
- Ochrona: obrona w głąb (tożsamość, szyfrowanie, segmentacja, monitoring, kopie zapasowe).
