# Modele chmur komputerowych

Modele chmury dzieli się według dwóch kryteriów: **modelu usług** (co dostawca dostarcza) i **modelu wdrożenia** (kto i dla kogo udostępnia chmurę).

## Modele usług (IaaS, PaaS, SaaS)

Każdy model usług wymaga **innych strategii bezpieczeństwa i narzędzi monitorowania**; zrozumienie podziału odpowiedzialności jest kluczowe dla planowania bezpieczeństwa i audytów.

| Model | Co dostarcza dostawca | Kontrola klienta | Odpowiedzialność za bezpieczeństwo (wg wykładu) | Przykłady |
| :--- | :--- | :--- | :--- | :--- |
| **IaaS** – Infrastructure as a Service | podstawowe zasoby obliczeniowe: **maszyny wirtualne, sieć, magazyn danych** | **największa** (system operacyjny, oprogramowanie pośredniczące, aplikacje, dane) | klient odpowiada za **większość zabezpieczeń** | AWS EC2, Azure Virtual Machines, Google Compute Engine, DigitalOcean |
| **PaaS** – Platform as a Service | **środowisko do tworzenia i uruchamiania aplikacji** (runtime, bazy, middleware) | nad **aplikacją i danymi**, nie nad infrastrukturą | dostawca – infrastruktura i platforma; klient – **aplikacje i dane** | Azure App Service, Google App Engine, Heroku, AWS Elastic Beanstalk, Cloud Foundry |
| **SaaS** – Software as a Service | **gotowa aplikacja** udostępniana przez Internet | **najmniejsza** (konfiguracja, użytkownicy, dane) | dostawca – większość aspektów; klient – **zarządzanie dostępem i zgodność z politykami** | Microsoft 365, Google Workspace, Salesforce, Dropbox |

*(uzupełnienie)* Rozszerzenia: **FaaS/serverless** (np. AWS Lambda, Azure Functions – klient dostarcza tylko kod funkcji), **CaaS** (Containers as a Service – np. Kubernetes zarządzany: EKS, AKS, GKE), **DBaaS**, **XaaS**.

### Stos warstw (kto zarządza)

```
 warstwa                 on-premises   IaaS     PaaS     SaaS
 ───────────────────────────────────────────────────────────────
 aplikacje                  klient     klient   klient   dostawca
 dane                       klient     klient   klient   klient*
 runtime / middleware       klient     klient   dostawca dostawca
 system operacyjny          klient     klient   dostawca dostawca
 wirtualizacja              klient     dostawca dostawca dostawca
 serwery / magazyny / sieć  klient     dostawca dostawca dostawca
 centrum danych (fizyczne)  klient     dostawca dostawca dostawca
 (* dane i dostęp użytkowników zawsze pozostają odpowiedzialnością klienta)
```

## Modele wdrożenia

| Model | Opis | Bezpieczeństwo (wg wykładu) |
| :--- | :--- | :--- |
| **Public (publiczna)** | zasoby udostępniane **ogółowi** przez dostawcę (AWS, Azure, GCP), współdzielone między klientami | wymaga **silnej izolacji między klientami** i szczególnej uwagi na **bezpieczeństwo danych** |
| **Private (prywatna)** | **dedykowana jednej organizacji** (w jej centrum danych lub u dostawcy) | większa **kontrola**, ale wymaga **więcej zasobów i ekspertyzy**; organizacja **ponosi pełną odpowiedzialność** za bezpieczeństwo |
| **Hybrid (hybrydowa)** | połączenie chmury prywatnej i publicznej (np. dane wrażliwe lokalnie, reszta w chmurze) | komplikuje **zarządzanie tożsamością** i **zgodnością (compliance)** – potrzebne **spójne polityki** w obu środowiskach |
| **Community (społecznościowa)** | współdzielona przez organizacje o **podobnych wymaganiach** (np. sektor publiczny, ochrona zdrowia, administracja) | szczególna uwaga na **zarządzanie dostępem i audyt**, bo zasoby są współdzielone między podmiotami o różnych wymaganiach bezpieczeństwa |

*(uzupełnienie)* **Multi-cloud** – użycie usług **kilku dostawców** (zmniejsza uzależnienie od dostawcy – *vendor lock-in*, ale zwiększa złożoność i ryzyko niespójnych konfiguracji).

## Porównanie z perspektywy bezpieczeństwa

| Kryterium | Public | Private | Hybrid | Community |
| :--- | :--- | :--- | :--- | :--- |
| Kontrola klienta | niska–średnia | **wysoka** | zróżnicowana | średnia |
| Izolacja | logiczna (multi-tenancy) | **fizyczna/dedykowana** | mieszana | częściowa |
| Koszt i elastyczność | **niski, wysoka** | wysoki, niższa | średni | średni |
| Główne ryzyko | współdzielenie, błędna konfiguracja | koszty i brak kompetencji | spójność polityk i tożsamości | różne wymagania uczestników |
| Odpowiedzialność za bezpieczeństwo | **współdzielona** | organizacja w całości | współdzielona/organizacji | wspólna |

## Dobór modelu

Zależy od: **wrażliwości danych** (klasyfikacja), **wymagań prawnych** (RODO, lokalizacja danych), **kosztów**, **kompetencji zespołu**, **potrzeb skalowania**, **wymagań dostępności (RPO/RTO)**.

## Podsumowanie

- **Modele usług:** IaaS (klient zarządza prawie wszystkim powyżej wirtualizacji), PaaS (klient – aplikacje i dane), SaaS (klient – dostęp, konfiguracja, zgodność).
- **Modele wdrożenia:** public, private, hybrid, community.
- Im wyższy poziom usługi (IaaS → PaaS → SaaS), tym **mniejsza kontrola, ale i mniejsza odpowiedzialność** klienta za techniczną stronę bezpieczeństwa.
