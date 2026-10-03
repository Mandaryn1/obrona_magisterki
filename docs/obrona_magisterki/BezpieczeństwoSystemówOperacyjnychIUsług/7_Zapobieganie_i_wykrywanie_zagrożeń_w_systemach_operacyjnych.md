# Zapobieganie i wykrywanie zagrożeń w systemach operacyjnych

## 1. Zapobieganie zagrożeniom (Prewencja i Hardening)

* **Utwardzanie systemu (Hardening):** Minimazowanie powierzchni ataku poprzez wyłączanie nieużywanych lub niezarządzanych usług i demonów działających w tle, zamykanie zbędnych portów oraz odinstalowanie niepotrzebnych pakietów oprogramowania.
* **Zarządzanie poprawkami (Patch Management):** Regularne sprawdzanie i instalowanie aktualizacji systemu operacyjnego oraz poprawek zabezpieczeń (np. za pomocą Windows Update), co pozwala wyeliminować znane luki w kodzie zanim zostaną one wykorzystane przez atakujących.
* **Zapory sieciowe (Firewall):** Selektywne filtrowanie i ograniczanie ruchu sieciowego przychodzącego i wychodzącego. Wdrażanie restrykcyjnych reguł (otwieranie tylko wymaganych portów i odrzucanie reszty ruchu).
* **Ochrona antywirusowa i punktów końcowych (AV / EDR):** Stosowanie oprogramowania ochronnego działającego w czasie rzeczywistym (np. Windows Defender) do wykrywania, blokowania i neutralizowania wirusów, trojanów, oprogramowania szpiegującego oraz phishingu.

---

## 2. Wykrywanie zagrożeń (Detekcja i Monitoring)

* **Analiza aktywnych połączeń i procesów:** Wykorzystywanie narzędzi systemowych (np. polecenia `netstat -abno` oraz Menedżera Zadań) do identyfikacji podejrzanych procesów oraz nieautoryzowanych usług nasłuchujących na otwartych portach.
* **Rejestrowanie zdarzeń (Event Logs):** Prowadzenie i regularna analiza dzienników zdarzeń (np. Podgląd Zdarzeń / Event Viewer w Windows), które rejestrują historię zdarzeń aplikacji, systemu i zabezpieczeń z podziałem na poziomy ważności (informacyjne, ostrzeżenia, błędy, krytyczne).
* **Systemy Wykrywania Włamań (IDS / IPS):** Narzędzia monitorujące ruch sieciowy lub aktywność hosta w czasie rzeczywistym w celu natychmiastowego wykrycia wzorców ataków i nieautoryzowanego dostępu.
* **Zarządzanie informacjami i zdarzeniami bezpieczeństwa (SIEM):** Centralne systemy zbierające i korelujące logi oraz alerty z wielu urządzeń i systemów w infrastrukturze w celu analizy zagrożeń w czasie rzeczywistym.

---

## 3. Podsumowanie do wypowiedzi na obronie

> *"Ochrona systemów operacyjnych przed zagrożeniami opiera się na dwóch filarach: prewencji oraz detekcji. Zapobieganie realizowane jest przez utwardzanie systemu (hardening), stałe aktualizowanie poprawek zabezpieczeń, filtrowanie ruchu zaporą ogniową oraz ochronę antywirusową w czasie rzeczywistym. Wykrywanie zagrożeń polega natomiast na stałym monitorowaniu procesów i otwartych portów (np. poleceniem netstat), analizie dzienników zdarzeń w Podglądzie Zdarzeń oraz wdrażaniu systemów IDS i SIEM do korelacji alertów bezpieczeństwa."*

## Podsumowanie

- **Zapobieganie:** utwardzanie (minimalizacja usług, najmniejsze uprawnienia, AppLocker/SELinux), **aktualizacje**, zapora, AV/Defender, Secure Boot+TPM, szyfrowanie, kopie zapasowe, MFA i blokady brute force, piaskownice, edukacja.
- **Wykrywanie:** **logi i audyt** (Event Log 4624/4625/4670/4663; `auth.log`, auditd), monitorowanie procesów i połączeń (`netstat`, `ps`, `ss`), **kontrola integralności**, wykrywanie rootkitów, HIDS/EDR/SIEM, reguły anomalii.
- Całość uzupełnia **reagowanie na incydenty** (temat 11) i **testy bezpieczeństwa** (temat 12).

---
[⬅️ Poprzedni temat](6_Mechanizmy_uwierzytelniania_hasła_klucze_SSH_2FA_IAM_i_SSO.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Zastosowanie_kryptografii_w_ochronie_danych_i_komunikacji.md)