# W jakim modelu chmury dostawca odpowiada za bezpieczeństwo infrastruktury i platformy, a klient za aplikacje i dane?

## Odpowiedź

**W modelu PaaS (Platform as a Service).**

Wg wykładu (slajd 13): *w modelu PaaS dostawca odpowiada za bezpieczeństwo **infrastruktury i platformy**, a klient za **aplikacje i dane**, co wymaga zrozumienia ograniczeń platformy i odpowiedniego projektowania aplikacji.*

## Model współdzielonej odpowiedzialności (Shared Responsibility Model)

Zasada ogólna: **dostawca zawsze odpowiada za bezpieczeństwo infrastruktury fizycznej** (centra danych, sprzęt, zasilanie, sieć szkieletowa), natomiast **klient zawsze odpowiada za dane i aplikacje** (oraz za swoje tożsamości i konfigurację). Zakres po obu stronach zmienia się wraz z modelem usług.

Często opisuje się to jako: dostawca – bezpieczeństwo **„*of* the cloud"** (chmury), klient – bezpieczeństwo **„*in* the cloud"** (w chmurze).

| Model | Odpowiedzialność dostawcy | Odpowiedzialność klienta | Uwagi (wykład) |
| :--- | :--- | :--- | :--- |
| **IaaS** | infrastruktura fizyczna, sieć, wirtualizacja (hypervisor) | **system operacyjny, aplikacje, dane, sieć** (konfiguracja sieci wirtualnej, zapory, aktualizacje, tożsamości) | wymaga **znacznej inwestycji w ekspertyzę techniczną i zasoby** |
| **PaaS** | **infrastruktura + platforma** (system operacyjny, środowisko uruchomieniowe, middleware) | **aplikacje i dane** (kod, konfiguracja aplikacji, kontrola dostępu) | trzeba rozumieć **ograniczenia platformy**, odpowiednio projektować aplikacje |
| **SaaS** | **większość aspektów** (infrastruktura, platforma, aplikacja) | **zarządzanie dostępem** (konta, uprawnienia, MFA) i **zgodność z politykami**; dane | trzeba rozumieć **możliwości konfiguracji** udostępniane przez dostawcę |

## Macierz odpowiedzialności

| Warstwa | On-premises | IaaS | **PaaS** | SaaS |
| :--- | :-: | :-: | :-: | :-: |
| Dane i klasyfikacja | K | K | **K** | K |
| Tożsamości i dostęp | K | K | **K** | K |
| Aplikacja | K | K | **K** | D |
| Konfiguracja platformy / bazy | K | K | **D/K** | D |
| Środowisko uruchomieniowe, middleware | K | K | **D** | D |
| System operacyjny | K | K | **D** | D |
| Wirtualizacja i sieć wirtualna | K | D | **D** | D |
| Sprzęt i centrum danych | K | D | **D** | D |

(K – klient, D – dostawca; „D/K" – odpowiedzialność dzielona, np. konfiguracja usług zarządzanych).

## Co zawsze zostaje po stronie klienta

Niezależnie od modelu *(uzupełnienie)*:

- **dane** (klasyfikacja, szyfrowanie, retencja, zgodność z RODO),
- **tożsamości i dostęp** (konta, uprawnienia, MFA),
- **konfiguracja** udostępnianych usług (np. publiczne/prywatne zasoby),
- zgodność z regulacjami wynikającymi z przetwarzanych danych.

Typowy błąd: założenie, że „dostawca zabezpiecza wszystko" – większość incydentów w chmurze wynika z błędów **klienta** (konfiguracja, uprawnienia).

## Przykład: PaaS

Aplikacja Spring Boot na Azure App Service: **dostawca** łata system operacyjny, aktualizuje środowisko Java, zabezpiecza sieć i hypervisor. **Klient** odpowiada za kod aplikacji (np. walidację danych, brak SQL injection), uwierzytelnianie i autoryzację w aplikacji, szyfrowanie danych wrażliwych, konfigurację dostępu do bazy, logowanie i obsługę sekretów.

## Podsumowanie

- Odpowiedź: **PaaS** – dostawca: infrastruktura i platforma; klient: aplikacje i dane.
- IaaS – klient odpowiada za OS, aplikacje, dane i sieć; SaaS – dostawca za większość, klient za dostęp i zgodność.
- Zawsze: **dostawca – infrastruktura fizyczna; klient – dane i aplikacje (oraz tożsamości i konfiguracja)**.
