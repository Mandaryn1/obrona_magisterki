# Podstawowe cele bezpieczeństwa systemów operacyjnych i usług sieciowych

## 1. Wstęp i definicja ogólna

Systemy operacyjne (SO) oraz zainstalowane oprogramowanie składają się z milionów linii kodu, co nieuchronnie stwarza ryzyko powstawania luk w zabezpieczeniach (podatności). Głównym celem zapewnienia bezpieczeństwa w systemach operacyjnych i usługach sieciowych jest ochrona zasobów sprzętowych, programowych oraz danych przed nieautoryzowanym dostępem, modyfikacją, kradzieżą czy przejęciem kontroli nad systemem.

---

## 2. Główne cele bezpieczeństwa (Triada CIA + Rozliczalność)

Wypowiedź na obronie warto oprzeć na fundamentach bezpieczeństwa informacji:

1. **Poufność (Confidentiality):**
   * **Cel:** Zapewnienie, że dane oraz usługi są dostępne wyłącznie dla upoważnionych podmiotów (użytkowników lub procesów).
   * **Realizacja:** 
     * Szyfrowanie danych w spoczynku (na dysku) oraz w transmisji sieciowej.
     * Wdrażanie zasad ograniczonego dostępu i kontroli uprawnień do plików oraz folderów.

2. **Integralność (Integrity):**
   * **Cel:** Ochrona danych, procesów i kodu systemowego przed nieuprawnioną, nieautoryzowaną lub przypadkową modyfikacją.
   * **Realizacja:**
     * Izolacja procesów w odrębnych prywatnych przestrzeniach adresowych pamięci, zapobiegająca modyfikacji kodu systemu przez aplikacje użytkownika.
     * Kontrola dostępu oraz weryfikacja uprawnień administracyjnych przy modyfikacji plików systemowych.

3. **Dostępność (Availability):**
   * **Cel:** Gwarancja, że autoryzowani użytkownicy mają ciągły dostęp do systemów, aplikacji i usług sieciowych w momencie, gdy tego potrzebują.
   * **Realizacja:**
     * Filtrowanie ruchu sieciowego za pomocą zapory ogniowej (Firewall) i stosowanie zasady domyślnego blokowania ruchu niewymaganego.
     * Ochrona przed złośliwym oprogramowaniem oraz zamykanie niepotrzebnych lub niezarządzanych usług działających w tle.

4. **Rozliczalność i Audytowalność (Accountability & Auditing):**
   * **Cel:** Umożliwienie jednoznacznego przypisania wykonanych w systemie działań i operacji konkretnemu użytkownikowi lub procesowi.
   * **Realizacja:**
     * Rejestrowanie zdarzeń systemowych, aplikacji i zabezpieczeń w dziennikach (np. Windows Event Log lub logi systemowe Linux).
     * Monitorowanie aktywnych połączeń sieciowych i przypisanych do nich procesów (np. za pomocą polecenia `netstat`).

---

## 3. Kluczowe mechanizmy i zasady w systemach operacyjnych i usługach

* **Zasada minimalnych uprawnień (Least Privilege):** Użytkownicy powinni pracować na kontach standardowych, a uprawnienia administracyjne (root/administrator) powinny być używane wyłącznie do wykonywania zadań wymagających podniesionych uprawnień. Zapobiega to dziedziczeniu pełnych uprawnień przez uruchamiane złośliwe oprogramowanie.
* **Minimalizacja powierzchni ataku (Attack Surface Reduction / Hardening):** Identyfikowanie i wyłączanie nieużywanych, niezarządzanych usług i demonów działających w tle oraz zamykanie zbędnych portów komunikacyjnych.
* **Zarządzanie poprawkami (Patch Management):** Regularne sprawdzanie i instalowanie aktualizacji oraz poprawek zabezpieczeń udostępnianych przez producentów, co pozwala wyeliminować znane luki w kodzie przed ich wykorzystaniem przez atakujących.
* **Filtrowanie ruchu (Zapora ogniowa / Firewall):** Selektywne otwieranie tylko niezbędnych portów komunikacyjnych i odrzucanie każdego pakietu, który nie został wyraźnie dozwolony.
* **Ochrona antywirusowa i monitoring (EDR / Defender):** Stosowanie oprogramowania ochronnego działającego w czasie rzeczywistym do wykrywania i neutralizowania złośliwego oprogramowania.

---

## 4. Podsumowanie do wypowiedzi na obronie

> *"Podstawowym celem bezpieczeństwa systemów operacyjnych i usług sieciowych jest ochrona zasobów poprzez realizację triady CIA — poufności, integralności i dostępności — oraz zapewnienie rozliczalności działań. Osiąga się to poprzez rozdzielenie uprawnień i stosowanie zasady minimalnych uprawnień, izolację pamięci, stałe zarządzanie poprawkami w celu eliminacji luk w kodzie, ograniczenie powierzchni ataku poprzez wyłączenie zbędnych usług oraz selektywne filtrowanie ruchu sieciowego zaporą ogniową."*

---
[⬅️ Poprzedni temat](0_Wstep.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](2_Architektura_systemu_operacyjnego_z_punktu_widzenia_bezpieczeństwa.md)