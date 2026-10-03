# Architektura systemu operacyjnego z punktu widzenia bezpieczeństwa

## 1. Pierścienie ochrony (Protection Rings)

* **Wsparcie sprzętowe:** Procesor udostępnia sprzętowe poziomy uprawnień (od Ring 0 do Ring 3).
* **Cel:** Odizolowanie zasobów krytycznych systemu od aplikacji użytkownika oraz uniemożliwienie procesom wzajemnego zakłócania pamięci i pracy.

---

## 2. Podział na Tryb Jądra i Tryb Użytkownika

* **Tryb Jądra (Kernel Mode / Ring 0):**
  * Kod jądra oraz sterowników ma nieograniczony dostęp do sprzętu, pełnej pamięci RAM i instrukcji procesora.
  * Awaria lub złośliwy kod w Ring 0 prowadzi do przejęcia systemu lub awarii całego SO (BSOD / Kernel Panic).
* **Tryb Użytkownika (User Mode / Ring 3):**
  * Aplikacje i usługi działają w odizolowanych, prywatnych przestrzeniach adresowych z ograniczonymi uprawnieniami.
  * Próba bezpośredniego odwołania do pamięci jądra wywołuje wyjątek sprzętowy i zamknięcie aplikacji.

---

## 3. Interfejsy i kontrola dostępu (Syscalls, Uchwyty, HAL)

* **Wywołania systemowe (Syscalls / API):** Bezpieczny mechanizm kontrolowanego przełączania kontekstu z Ring 3 do Ring 0, gdy aplikacja żąda zasobu (np. odczyt pliku).
* **Uchwyty (Handles):** Pośrednie identyfikatory obiektów jądra. System weryfikuje uprawnienia procesu przy każdym użyciu uchwytu.
* **Warstwa HAL (Hardware Abstraction Layer):** Izoluje jądro i sterowniki od różnic w fizycznym sprzęcie.

---

## 4. Modele architektury jądra

* **Jądro monolityczne (np. Linux, Windows NT):** Wszystkie kluczowe usługi i sterowniki działają w Ring 0. Wysoka wydajność, ale większa powierzchnia ataku (luka w sterowniku kompromituje cały system).
* **Mikrojądro (Microkernel):** W Ring 0 działa tylko absolutne minimum (pamięć, wątki, IPC). Sterowniki działają w Ring 3. Wysoka odporność na awarie, ale większy narzut wydajnościowy.

---

## 5. Bezpieczny rozruch (Secure Boot)

* **UEFI Secure Boot:** Weryfikacja podpisu cyfrowego programu ładującego (*Bootloader*).
* **KMCS (Kernel Mode Code Signing):** System weryfikuje podpisy cyfrowe sterowników przed wczytaniem ich do pamięci jądra.

---

## 6. Podsumowanie na obronę

> *"Architektura SO z punktu widzenia bezpieczeństwa opiera się na sprzętowej izolacji – podziale na uprzywilejowany Tryb Jądra i ograniczony Tryb Użytkownika. Aplikacje działają w odrębnych przestrzeniach wirtualnych, a dostęp do zasobów odbywa się wyłącznie przez wywołania systemowe i weryfikowane uchwyty. Bezpieczeństwo architektury wzmacnia bezpieczny rozruch Secure Boot oraz podpisywanie sterowników jądra."*

## Podsumowanie

- Bezpieczny OS opiera się na **izolacji trybu jądra i użytkownika**, **monitorze odwołań** pośredniczącym w dostępie, **procesach z własnymi przestrzeniami adresowymi**, **systemie plików z uprawnieniami**, **zaufanym rozruchu (UEFI Secure Boot + TPM)** oraz małym TCB.
- Windows: HAL, jądro, NT Executive (SRM, LSASS), NTFS (ACL, ADS), rejestr, usługi, AD/GPO; Linux: monolityczne jądro z LSM, „wszystko jest plikiem", prawa plików, PAM, SELinux/AppArmor, logi w `/var/log`.
- Każdy komponent (autostart, usługi, sterowniki, ADS, rejestr) to także potencjalny element **utrwalania zagrożeń**.

---
[⬅️ Poprzedni temat](1_Podstawowe_cele_bezpieczeństwa_systemów_operacyjnych_i_usług_sieciowych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](3_Jądro_systemu_separacja_przestrzeni_użytkownika_i_ochrona_pamięci.md)