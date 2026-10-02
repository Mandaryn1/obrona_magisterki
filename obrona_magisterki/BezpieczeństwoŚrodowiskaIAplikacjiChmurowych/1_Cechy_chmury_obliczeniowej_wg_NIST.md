# Cechy chmury obliczeniowej wg NIST

## Definicja NIST (SP 800-145)

**NIST (National Institute of Standards and Technology)** definiuje chmurę obliczeniową jako **model umożliwiający wszechobecny, wygodny dostęp sieciowy na żądanie do współdzielonej puli konfigurowalnych zasobów obliczeniowych** (sieci, serwery, magazyny danych, aplikacje, usługi), które można **szybko udostępniać i zwalniać przy minimalnym wysiłku zarządzania lub interakcji z dostawcą usługi**.

Źródło: NIST Special Publication 800-145 *The NIST Definition of Cloud Computing* (2011).

*(uzupełnienie)* Definicja NIST ma układ: **5 cech podstawowych + 3 modele usług + 4 modele wdrożenia** (modele – zob. temat 3).

## Pięć istotnych cech chmury wg NIST

| Nr | Cecha (ang.) | Znaczenie | Konsekwencje dla bezpieczeństwa (wg wykładu) |
| :-: | :--- | :--- | :--- |
| 1 | **Samoobsługa na żądanie** (*on-demand self-service*) | użytkownik **samodzielnie** uruchamia i zarządza zasobami (moc obliczeniowa, pamięć) bez udziału człowieka po stronie dostawcy | wymaga odpowiednich **kontroli dostępu i monitorowania** (kto może tworzyć zasoby, limity, audyt) |
| 2 | **Szeroki dostęp do sieci** (*broad network access*) | usługi dostępne przez sieć (Internet), przez standardowe mechanizmy, z różnych urządzeń (laptop, telefon, tablet) | **zwiększa powierzchnię ataku** – potrzebne dodatkowe warstwy zabezpieczeń i **uwierzytelnianie** |
| 3 | **Pula zasobów** (*resource pooling*) | zasoby dostawcy **współdzielone** przez wielu klientów (**multi-tenancy**), dynamicznie przydzielane; klient zwykle nie zna dokładnej lokalizacji | wymaga **silnej izolacji między klientami** i mechanizmów **szyfrowania** |
| 4 | **Szybka elastyczność** (*rapid elasticity*) | zasoby można błyskawicznie zwiększać lub zmniejszać (często automatycznie), pozornie w nieograniczonej ilości | zagrożenia mogą się **szybko rozprzestrzeniać**, jeśli nie są kontrolowane → **automatyzacja bezpieczeństwa i monitoring w czasie rzeczywistym** |
| 5 | **Mierzalna usługa** (*measured service*) | zużycie zasobów jest **mierzone, monitorowane, raportowane** (płatność za użycie – *pay-as-you-go*) | dane metryczne (o użyciu zasobów) mogą zawierać **wrażliwe informacje** – wymagają ochrony i prywatności |

## Dlaczego te cechy są istotne dla bezpieczeństwa

Każda cecha, która jest korzyścią biznesową, tworzy jednocześnie specyficzne ryzyko:

| Korzyść | Ryzyko |
| :--- | :--- |
| samoobsługa → szybkość | „shadow IT", niekontrolowane zasoby, wysokie koszty, błędne konfiguracje |
| dostęp przez sieć → wygoda | ekspozycja usług w Internecie, ataki na API i konta |
| pula zasobów → niskie koszty | współdzielenie sprzętu, ucieczka z izolacji, wycieki między najemcami |
| elastyczność → skalowalność | szybkie skalowanie także ataku/nadużycia (np. kryptowalutowe „koparki", wzrost kosztów) |
| pomiar → rozliczalność | wyciek metadanych o użyciu |

## Powiązane pojęcia *(uzupełnienie)*

- **Multi-tenancy (wielodostępność)** – wielu klientów na tej samej infrastrukturze; wymaga logicznej izolacji (hypervisor, kontenery, sieć, szyfrowanie).
- **Provisioning** – szybkie przydzielanie zasobów; **Infrastructure as Code (IaC)** automatyzuje je (i błędy konfiguracji też).
- Dodatkowe cechy omawiane w literaturze (ISO/IEC 22123): m.in. **wielodostępność** jako cecha chmury.

## Podsumowanie

- NIST: chmura = współdzielona pula konfigurowalnych zasobów dostępna na żądanie przez sieć, szybko przydzielana i zwalniana z minimalnym zaangażowaniem dostawcy.
- **5 cech:** samoobsługa na żądanie, szeroki dostęp do sieci, pula zasobów, szybka elastyczność, mierzalna usługa.
- Każda cecha niesie konkretne wymagania: **kontrola dostępu i monitoring**, **uwierzytelnianie i warstwy ochrony**, **izolacja i szyfrowanie**, **automatyzacja i monitoring w czasie rzeczywistym**, **ochrona danych metrycznych**.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Główne_zagrożenia_bezpieczeństwa_chmury.md)