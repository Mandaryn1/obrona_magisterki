# Jądro systemu, separacja przestrzeni użytkownika i mechanizmy ochrony pamięci

## 1. Jądro systemu (Kernel)

* **Definicja:** Rdzeń systemu operacyjnego sprawujący bezpośrednią kontrolę nad sprzętem, przydziałem czasu procesora, pamięcią RAM oraz urządzeniami wejścia/wyjścia.
* **Tryb pracy (Kernel Mode / Ring 0):** Kod jądra i sterowników wykonuje się z pełnymi uprawnieniami procesora, mając nieograniczony dostęp do całej pamięci fizycznej i instrukcji sprzętowych.

---

## 2. Separacja przestrzeni (User Space vs Kernel Space)

* **Tryb Użytkownika (User Mode / Ring 3):** Aplikacje i usługi użytkownika działają w odizolowanym środowisku z ograniczonym dostępem do sprzętu.
* **Izolacja procesów:** Każdy proces otrzymuje własną prywatną przestrzeń adresową; nie może bezpośrednio odczytywać ani modyfikować pamięci jądra ani innych procesów, co zapobiega awariom i uszkodzeniu danych.
* **Kontrolowane przełączanie (Wywołania systemowe & Uchwyty):** Aplikacja żądająca zasobu jądra (np. odczytu pliku) wywołuje funkcję API/syscall i korzysta z pośrednich identyfikatorów (*Handles*), co pozwala jądru zweryfikować uprawnienia przed wykonaniem operacji.

---

## 3. Mechanizmy ochrony pamięci

* **Pamięć wirtualna i stronicowanie (Virtual Memory):** Odwzorowanie wirtualnych adresów procesu na fizyczne strony pamięci RAM za pomocą jednostki MMU (Memory Management Unit). Daje procesom iluzję wyłączności i ciągłości pamięci (do 4 GB w systemach 32-bit, do 8 TB w 64-bit).
* **DEP / NX (Data Execution Prevention / No-Execute):** Sprzętowo-programowa ochrona oznaczająca strony pamięci (np. stos i stertę) jako niewykonywalne (`Non-Executable`). Uniemożliwia to uruchomienie złośliwego kodu (np. *shellcode*) wstrzykniętego przez przepełnienie bufora (*Buffer Overflow*).
* **ASLR (Address Space Layout Randomization):** Losowa alokacja adresów pamięci dla jądra, stosu, sterty oraz bibliotek (DLL/so) przy każdym uruchomieniu. Atakujący nie jest w stanie przewidzieć stałych adresów funkcji w pamięci, co neutralizuje ataki typu *Return-to-libc* czy *ROP (Return-Oriented Programming)*.
* **Ochrona stosu (Stack Canaries / Cookie):** Wstawianie losowych wartości przed adresem powrotnym na stosie przed wykonaniem funkcji. Zmiana wartości wskaźnika powoduje natychmiastowe przerwanie procesu przed wykonaniem złośliwego kodu.

---

## 4. Podsumowanie na obronę

> *"Jądro systemu działa w uprzywilejowanym trybie jądra, zarządzając sprzętem, podczas gdy aplikacje są odizolowane w trybie użytkownika i własnych przestrzeniach wirtualnych. Ochrona pamięci opiera się na separacji procesów poprzez MMU, blokowaniu wykonywania kodu z obszarów danych za pomocą DEP/NX, losowaniu adresów w RAM poprzez ASLR oraz zabezpieczeniach stosu przed przepełnieniem bufora."*

## Podsumowanie

- **Separacja:** tryb użytkownika (ring 3) vs jądra (ring 0), **wywołania systemowe** jako jedyna brama, **własne przestrzenie adresowe** procesów, IPC kontrolowane, dodatkowo namespaces, seccomp, integrity levels, hypervisor.
- **Ochrona pamięci:** MMU i uprawnienia stron (R/W/X), **NX/DEP, ASLR/KASLR, stack canary, CFG/CET, SMEP/SMAP, KPTI**, podpisy sterowników i Secure Boot.
- Kompromitacja jądra = kompromitacja systemu (rootkity); utwardzone jądro (ASLR, PaX, sandboxing, ograniczenia, rozszerzony audyt – wykład W8) zmniejsza ryzyko.

---
[⬅️ Poprzedni temat](2_Architektura_systemu_operacyjnego_z_punktu_widzenia_bezpieczeństwa.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Zarządzanie_użytkownikami_uprawnieniami_i_kontrolą_dostępu_w_systemach_operacyjnych.md)