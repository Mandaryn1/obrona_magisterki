# Jakie są cechy chmury obliczeniowej wg NIST?

Według NIST (definicja SP 800-145) chmura obliczeniowa ma **pięć podstawowych cech**:

1. **Samoobsługa na żądanie (on-demand self-service):** użytkownik sam, automatycznie, bez udziału człowieka po stronie dostawcy, uruchamia zasoby (moc obliczeniową, pamięć, sieć).
2. **Szeroki dostęp sieciowy (broad network access):** usługi są dostępne przez sieć (Internet) za pomocą standardowych mechanizmów, z różnych urządzeń: laptopów, telefonów, tabletów.
3. **Pula zasobów (resource pooling):** zasoby dostawcy są współdzielone przez wielu klientów (model **multi-tenant**), dynamicznie przydzielane i zwalniane według zapotrzebowania. Klient zwykle nie wie i nie kontroluje dokładnej lokalizacji zasobów.
4. **Elastyczność (rapid elasticity):** zasoby można szybko zwiększać lub zmniejszać, często automatycznie, w zależności od obciążenia. Dla klienta wyglądają na nieograniczone.
5. **Mierzalność usługi (measured service):** zużycie zasobów jest monitorowane, mierzone i raportowane. Dzięki temu możliwe jest rozliczanie według faktycznego użycia (**pay-as-you-go**), kontrola i optymalizacja kosztów.

**Wskazówka do zapamiętania:** samoobsługa, dostęp sieciowy, pula zasobów, elastyczność, pomiar.

Definicja NIST wyróżnia ponadto **3 modele usług** (IaaS, PaaS, SaaS) i **4 modele wdrożenia** (publiczna, prywatna, hybrydowa, społeczności), o które często pytają w następnych pytaniach.

## Podsumowanie

- NIST: chmura = współdzielona pula konfigurowalnych zasobów dostępna na żądanie przez sieć, szybko przydzielana i zwalniana z minimalnym zaangażowaniem dostawcy.
- **5 cech:** samoobsługa na żądanie, szeroki dostęp do sieci, pula zasobów, szybka elastyczność, mierzalna usługa.
- Każda cecha niesie konkretne wymagania: **kontrola dostępu i monitoring**, **uwierzytelnianie i warstwy ochrony**, **izolacja i szyfrowanie**, **automatyzacja i monitoring w czasie rzeczywistym**, **ochrona danych metrycznych**.

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Główne_zagrożenia_bezpieczeństwa_chmury.md)