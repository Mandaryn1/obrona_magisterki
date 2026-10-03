# Jakie są główne zagrożenia bezpieczeństwa chmury?

Główne zagrożenia bezpieczeństwa chmury to:

- **Utrata lub wyciek danych:** z powodu błędów konfiguracji (np. publicznie dostępny zasób), ataku, awarii albo błędu dostawcy lub klienta. Dotyczy to także braku kopii zapasowych.
- **Przejęcie konta (account hijacking):** kradzież poświadczeń przez phishing, słabe lub wyciekłe hasła i brak MFA. Atakujący zyskuje dostęp do całych zasobów.
- **Nieautoryzowany dostęp:** zbyt szerokie uprawnienia, błędna konfiguracja IAM, brak zasady najmniejszych uprawnień, zagrożenia wewnętrzne.
- **Ataki na API i interfejsy zarządzania:** zasoby chmury są sterowane przez API, więc słabo zabezpieczone API (brak uwierzytelniania, limitów, walidacji) daje atakującemu duże możliwości. Jest to jedno z najbardziej niebezpiecznych zagrożeń.
- **Złośliwe oprogramowanie i ransomware:** infekcje maszyn wirtualnych, kontenerów lub aplikacji, szyfrowanie danych.
- **Nadużycie usług chmurowych:** wykorzystanie zasobów do celów przestępczych, np. kopanie kryptowalut albo ataki DDoS na koszt ofiary.
- **Błędy konfiguracji:** niezabezpieczone magazyny danych, otwarte porty, domyślne hasła. To najczęstsza przyczyna incydentów.
- **Współdzielenie zasobów (multi-tenancy):** ryzyko ucieczki z maszyny wirtualnej lub kontenera i przecieku między klientami.
- **Ataki DoS/DDoS** powodujące niedostępność usług oraz **zagrożenia łańcucha dostaw**.
- **Utrata kontroli i zgodności:** uzależnienie od dostawcy (vendor lock-in), problemy z lokalizacją danych i regulacjami (RODO).

**Jak ograniczać:** MFA i IAM z najmniejszymi uprawnieniami, szyfrowanie danych, bezpieczna konfiguracja i monitoring, zabezpieczenie API, kopie zapasowe oraz **model współdzielonej odpowiedzialności**, w którym klient odpowiada za swoje dane, konta i konfigurację.

## Podsumowanie

- Główne zagrożenia (wykład): **utrata danych, przejęcie konta, nieautoryzowany dostęp, ataki na API, złośliwe oprogramowanie, nadużycie usług**.
- Dla chmury charakterystyczne są **błędy konfiguracji** i **tożsamość** jako najsłabsze ogniwo.
- Ryzyka zarządzania: zależność od dostawcy, utrata kontroli nad danymi, nieautoryzowany dostęp.
- Ochrona: obrona w głąb (tożsamość, szyfrowanie, segmentacja, monitoring, kopie zapasowe).

---
[⬅️ Poprzedni temat](1_Cechy_chmury_obliczeniowej_wg_NIST.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](3_Modele_chmur_komputerowych.md)