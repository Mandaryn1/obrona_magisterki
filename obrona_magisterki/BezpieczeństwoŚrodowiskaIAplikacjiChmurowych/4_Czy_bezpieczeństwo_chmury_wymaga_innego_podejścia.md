# Czy do bezpieczeństwa chmury trzeba podchodzić inaczej niż do standardowych metod bezpieczeństwa IT?

## Odpowiedź

**TAK.** Cele bezpieczeństwa (poufność, integralność, dostępność – CIA) i zasady (obrona w głąb, najmniejsze uprawnienia) pozostają takie same, ale **bezpieczeństwo chmury różni się znacząco od tradycyjnego bezpieczeństwa IT** (slajd 4) ze względu na:

1. **współdzielenie infrastruktury**,
2. **dynamiczną naturę zasobów**,
3. **nowe modele zagrożeń**.

Chmura **nie jest po prostu innym miejscem uruchamiania aplikacji**, ale **fundamentalnie różnym modelem obliczeniowym z unikalnymi wyzwaniami bezpieczeństwa** (slajd 5).

## Dlaczego podejście musi być inne

| Aspekt | Tradycyjne IT (on-premises) | Chmura |
| :--- | :--- | :--- |
| **Odpowiedzialność** | organizacja odpowiada za **całość** (od zasilania serwerowni po aplikacje) | **model współdzielonej odpowiedzialności** dostawcy i klienta (temat 5); klient musi wiedzieć, co jest po jego stronie |
| **Infrastruktura** | **dedykowana** organizacji | **współdzielona** (multi-tenancy) – potrzebna izolacja klientów, szyfrowanie |
| **Granica sieci (perymetr)** | wyraźny obwód (firewall wokół firmy) | granica „rozmyta": usługi dostępne z Internetu, praca zdalna, wiele dostawców → **tożsamość jest nowym perymetrem**, podejście **Zero Trust** |
| **Zasoby** | statyczne, wolno zmieniane | **dynamiczne**, efemeryczne (kontenery, funkcje, autoskalowanie) – konieczna **automatyzacja** i monitoring w czasie rzeczywistym |
| **Zarządzanie** | zmiany przez zespół IT, formalne procedury | wszystko to **API i kod (IaC)** – jedna błędna konfiguracja lub wyciek klucza ma natychmiastowy, globalny skutek |
| **Widoczność i kontrola** | pełny dostęp do sprzętu i logów | **ograniczony** dostęp do infrastruktury dostawcy; audyt wymaga współpracy z dostawcą (slajd 37) |
| **Główne ryzyka** | włamanie do sieci, malware, awarie sprzętu | **błędna konfiguracja**, przejęte konta, ataki na API, zależność od dostawcy, utrata kontroli nad danymi |
| **Dostęp do zasobów** | w sieci lokalnej | **szeroki dostęp sieciowy** – większa powierzchnia ataku |
| **Zgodność z prawem** | dane w znanej lokalizacji | **lokalizacja danych**, transfery międzynarodowe (RODO), wiele jurysdykcji |
| **Skalowanie ataków i kosztów** | ograniczone sprzętem | łatwe skalowanie = także **skalowanie nadużyć** (koszty, DDoS) |
| **Tożsamości** | użytkownicy | użytkownicy + **aplikacje, usługi, funkcje** (każda mikrousługa może wymagać własnej tożsamości – slajd 14) |
| **Moment projektowania** | często „dołożenie" zabezpieczeń | bezpieczeństwo **od samego początku** projektowania (security by design) |

## Co pozostaje takie samo

- cele **CIA**, klasyfikacja danych, zarządzanie ryzykiem, zgodność (ISO 27001, SOC 2, PCI DSS, RODO),
- obrona w głąb (**defense in depth**), najmniejsze uprawnienia, szyfrowanie, monitoring, kopie zapasowe, reagowanie na incydenty,
- potrzeba procedur i audytu.

## Nowe elementy podejścia do chmury

- **Model współdzielonej odpowiedzialności** i znajomość podziału (IaaS/PaaS/SaaS).
- **IAM jako fundament** (federacja, SSO, MFA, najmniejsze uprawnienia, tożsamości usług) – temat 6.
- **Bezpieczeństwo jako kod:** IaC + skanowanie, **policy-as-code**, DevSecOps (SAST/DAST, skanowanie obrazów, SBOM) – slajd 216.
- **Zero Trust:** „nigdy nie ufaj, zawsze weryfikuj", minimalny dostęp, zakładanie naruszenia (slajd 238).
- **Segmentacja i mikrosegmentacja** (VPC, security groups, NetworkPolicies) – temat 12.
- **Zarządzanie kluczami** (KMS/HSM), szyfrowanie wszystkiego (w spoczynku, w tranzycie, w użyciu) – temat 9.
- **Ciągłe monitorowanie** i automatyczna reakcja (SIEM, alerty) zamiast okresowych kontroli.
- **Bezpieczeństwo API i kontenerów** jako nowe powierzchnie ataku.
- Zgodność z regulacjami i **audyt w warunkach ograniczonego dostępu** do infrastruktury.
- Zarządzanie ryzykiem **dostawcy** (vendor lock-in, SLA, certyfikaty dostawcy).

## Przykład

Otwarty publiczny bucket w chmurze (jedna linia konfiguracji lub przełącznik w konsoli) może ujawnić miliony rekordów w ciągu minut – w tradycyjnym IT taki wyciek wymagałby wielu naruszeń warstw sieciowych. Dlatego w chmurze **konfiguracja jest nową „powierzchnią ataku"** i wymaga automatycznego skanowania i polityk.

## Podsumowanie

- **Tak** – chmura wymaga innego podejścia: współdzielona odpowiedzialność, współdzielona infrastruktura, dynamiczne zasoby, rozmyty perymetr, zarządzanie przez API, ograniczona widoczność, nowe zagrożenia.
- Zasady podstawowe (CIA, najmniejsze uprawnienia, obrona w głąb) pozostają, lecz **ich realizacja jest inna** (IAM, automatyzacja, Zero Trust, szyfrowanie, DevSecOps).
- Bezpieczeństwo trzeba projektować **od początku** i obejmować **wszystkie warstwy** (holistycznie).

---
[⬅️ Poprzedni temat](3_Modele_chmur_komputerowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](5_Model_chmury_w_którym_dostawca_odpowiada_za_infrastrukturę_i_platformę.md)