# Jakie znasz modele chmur komputerowych?

Chmury komputerowe klasyfikuje się na dwa sposoby: według **modeli usług** i według **modeli wdrożenia** (definicja NIST).

**Modele usług (co dostaje klient):**

- **IaaS (Infrastructure as a Service):** klient dostaje infrastrukturę, czyli maszyny wirtualne, sieć i pamięć masową. Sam zarządza systemem operacyjnym, aplikacjami i danymi. Przykłady: AWS EC2, Azure Virtual Machines.
- **PaaS (Platform as a Service):** dostawca zapewnia platformę (system, środowisko uruchomieniowe, bazy danych). Klient zajmuje się aplikacjami i danymi. Przykłady: Google App Engine, Azure App Service, Heroku.
- **SaaS (Software as a Service):** gotowa aplikacja dostępna przez przeglądarkę, a klient tylko z niej korzysta. Przykłady: Microsoft 365, Gmail, Salesforce.

Im bliżej SaaS, tym więcej odpowiedzialności ma dostawca, a im bliżej IaaS, tym więcej klient.

**Modele wdrożenia (kto korzysta i kto zarządza):**

- **Chmura publiczna:** udostępniana wielu klientom przez dostawcę (AWS, Azure, Google Cloud). Jest tania i skalowalna, ale z mniejszą kontrolą.
- **Chmura prywatna:** zasoby przeznaczone dla jednej organizacji, we własnym centrum danych lub u dostawcy. Daje większą kontrolę i bezpieczeństwo, ale jest droższa.
- **Chmura hybrydowa:** połączenie chmury prywatnej i publicznej, np. dane wrażliwe lokalnie, a szczytowe obciążenia w chmurze publicznej.
- **Chmura społeczności (community):** wspólna dla kilku organizacji o podobnych potrzebach i wymaganiach (np. instytucje publiczne, sektor zdrowia).

Dodatkowo spotyka się **multi-cloud** (usługi od wielu dostawców) oraz modele pochodne, np. **FaaS/serverless** (uruchamianie funkcji bez zarządzania serwerami).

## Podsumowanie

- **Modele usług:** IaaS (klient zarządza prawie wszystkim powyżej wirtualizacji), PaaS (klient – aplikacje i dane), SaaS (klient – dostęp, konfiguracja, zgodność).
- **Modele wdrożenia:** public, private, hybrid, community.
- Im wyższy poziom usługi (IaaS → PaaS → SaaS), tym **mniejsza kontrola, ale i mniejsza odpowiedzialność** klienta za techniczną stronę bezpieczeństwa.

---
[⬅️ Poprzedni temat](2_Główne_zagrożenia_bezpieczeństwa_chmury.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Czy_bezpieczeństwo_chmury_wymaga_innego_podejścia.md)