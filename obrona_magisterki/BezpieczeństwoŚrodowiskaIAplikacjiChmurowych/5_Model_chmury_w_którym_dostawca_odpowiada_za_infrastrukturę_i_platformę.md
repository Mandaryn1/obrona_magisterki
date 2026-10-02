# W jakim modelu chmury dostawca odpowiada za bezpieczeństwo infrastruktury i platformy, a klient za aplikacje i dane?

W modelu **PaaS (Platform as a Service)**.

W PaaS dostawca zapewnia **infrastrukturę** (sprzęt, sieć, wirtualizację, serwery) oraz **platformę**: system operacyjny, środowisko uruchomieniowe, bazy danych i oprogramowanie pośredniczące. Odpowiada za ich bezpieczeństwo i aktualizacje. Klient odpowiada za **swoje aplikacje i dane**, a także za konfigurację dostępu: konta, uprawnienia, uwierzytelnianie.

**Dla porównania (model współdzielonej odpowiedzialności):**

- **IaaS:** dostawca odpowiada za infrastrukturę fizyczną i wirtualizację. Klient za system operacyjny, oprogramowanie pośredniczące, aplikacje i dane.
- **PaaS:** dostawca odpowiada za infrastrukturę i platformę. Klient za aplikacje i dane.
- **SaaS:** dostawca odpowiada niemal za wszystko, łącznie z aplikacją. Klient za dane, konta użytkowników i konfigurację dostępu.

Niezależnie od modelu klient zawsze odpowiada za **dane, tożsamości i dostęp** do swoich zasobów.

## Podsumowanie

- Odpowiedź: **PaaS** – dostawca: infrastruktura i platforma; klient: aplikacje i dane.
- IaaS – klient odpowiada za OS, aplikacje, dane i sieć; SaaS – dostawca za większość, klient za dostęp i zgodność.
- Zawsze: **dostawca – infrastruktura fizyczna; klient – dane i aplikacje (oraz tożsamości i konfiguracja)**.

---
[⬅️ Poprzedni temat](4_Czy_bezpieczeństwo_chmury_wymaga_innego_podejścia.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](6_IAM_w_bezpieczeństwie_chmurowym.md)