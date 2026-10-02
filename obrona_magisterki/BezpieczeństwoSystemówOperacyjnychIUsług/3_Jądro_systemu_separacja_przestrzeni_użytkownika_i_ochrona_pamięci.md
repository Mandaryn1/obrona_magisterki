# Jądro systemu, separacja przestrzeni użytkownika i mechanizmy ochrony pamięci

> Wykład: W1 (tryb użytkownika/jądra, pamięć wirtualna, wątki – slajdy 19–20, 36–39), W2 (procesy, fork, rootkity – slajdy 48, 50), W8 (slajd 14: utwardzone jądro – ASLR, PaX, sandboxing). Resztę (mechanizmy sprzętowe i programowe, ataki) opracowałem z własnej wiedzy ***(uzupełnienie)***.

## Jądro systemu

**Jądro (kernel)** – rdzeń OS działający z najwyższymi uprawnieniami; zarządza **procesami** (planista), **pamięcią** (wirtualna, ochrona), **urządzeniami** (sterowniki), **systemem plików** i **siecią**, a także egzekwuje **kontrolę dostępu**. Wykład (W1): jądro kontroluje cały komputer i obsługuje żądania I/O, pamięć i urządzenia peryferyjne; kod w trybie jądra może wykonać dowolną instrukcję i odwołać się do dowolnego adresu pamięci, w jednej wspólnej przestrzeni adresowej – **błąd lub kompromitacja jądra oznacza kompromitację całego systemu.**

## Separacja przestrzeni użytkownika i jądra

### Poziomy uprzywilejowania procesora

Procesor (np. x86) ma **pierścienie ochrony (ring 0–3)**; w praktyce używa się dwóch: **ring 0 – tryb jądra**, **ring 3 – tryb użytkownika** (ARM: EL0–EL3). Instrukcje uprzywilejowane (dostęp do portów I/O, modyfikacja tablic stron, wyłączanie przerwań) działają tylko w trybie jądra.

```
 aplikacja (ring 3)  ──syscall / sysenter / int──▶  jądro (ring 0)  ──▶ sprzęt
        ▲   próba instrukcji uprzywilejowanej → wyjątek (#GP) → proces zakończony
        └──────────────── wynik wywołania ◀──────────────────────────────
```

### Wywołania systemowe (system calls)

Jedyny **kontrolowany punkt wejścia** z trybu użytkownika do jądra. Aplikacja nie ma bezpośredniego dostępu do sprzętu – prosi jądro (np. `open`, `read`, `write`, `fork`, `execve`; w Windows – Win32 API → `ntdll` → `syscall`). Jądro **sprawdza argumenty i uprawnienia** (monitor odwołań), więc **walidacja danych z przestrzeni użytkownika** w jądrze jest krytyczna (błędy → eskalacja uprawnień).

### Izolacja procesów

- **każdy proces ma własną wirtualną przestrzeń adresową** (wykład W1: 4 GB w 32-bit; 8 TB w 64-bit wg wykładu), niewidoczną dla innych; **wątki jednego procesu dzielą przestrzeń** – błąd wątku może zniszczyć proces, ale nie inne procesy,
- komunikacja między procesami tylko przez **IPC** (potoki, gniazda, pamięć współdzielona, komunikaty) pod kontrolą jądra,
- osobne **tożsamości i uprawnienia** (UID/token) dla procesów,
- **dodatkowa izolacja (Linux):** **namespaces** (PID, sieć, montowania, użytkownicy…), **cgroups** (limity zasobów), **seccomp** (filtr wywołań systemowych), **capabilities** (podział uprawnień roota), **chroot**, **kontenery**; **(Windows):** **integrity levels**, **AppContainer**, **job objects**, **VBS** (wirtualizacja chroni jądro i sekrety), **Windows Sandbox**; **wirtualizacja (hypervisor)** – najsilniejsza izolacja.

## Ochrona pamięci

### Pamięć wirtualna i MMU *(uzupełnienie)*

**MMU** tłumaczy adresy wirtualne na fizyczne przez **tablice stron** zarządzane przez jądro. Każda strona ma bity uprawnień: **R (odczyt), W (zapis), X (wykonanie), U/S (użytkownik/nadzorca)**. Dostęp niezgodny z uprawnieniami → **błąd strony (page fault / segfault)** → jądro kończy proces. Skutki: proces **nie może czytać ani pisać cudzej pamięci**, a kod użytkownika nie może dotykać pamięci jądra.

### Mechanizmy utrudniające wykorzystanie błędów pamięci

| Mechanizm | Działanie | Atak, któremu przeciwdziała |
| :--- | :--- | :--- |
| **NX / XD / DEP** (Data Execution Prevention; zasada **W^X**) | strony danych i stosu **niewykonywalne** | wstrzyknięcie i wykonanie shellcode'u na stosie/stercie |
| **ASLR** (Address Space Layout Randomization) | **losowy** układ stosu, sterty, bibliotek, kodu (przy PIE) przy każdym uruchomieniu | zgadnięcie adresów w exploitach (ROP/ret2libc); wykład W8: utwardzone jądro oferuje ASLR i PaX |
| **KASLR** | losowanie adresu jądra | ataki na jądro |
| **Stack canary** (stack protector) | wartość strażnicza przed adresem powrotu; sprawdzana przy powrocie | przepełnienie bufora na stosie |
| **PIE / RELRO / FORTIFY_SOURCE** | kod niezależny od adresu; ochrona tablicy GOT; bezpieczne wersje funkcji | nadpisanie GOT, błędy `memcpy` |
| **CFG / CFI / CET (shadow stack, IBT)** | kontrola przepływu sterowania, sprzętowy stos cieni | **ROP/JOP** (programowanie zorientowane na powroty) |
| **SMEP / SMAP** | jądro nie wykonuje i nie czyta bezkrytycznie pamięci użytkownika | eskalacja uprawnień przez wskaźnik do przestrzeni użytkownika |
| **KPTI** (page-table isolation) | oddzielne tablice stron jądra i użytkownika | **Meltdown** |
| **Guard pages, zero-initialization, bezpieczne alokatory** | ochrona granic i danych | przepełnienia, use-after-free |
| **Podpisy sterowników i modułów jądra, Secure Boot, PatchGuard/KPP, lockdown** | tylko zaufany kod w jądrze; ochrona struktur jądra | rootkity jądra, ładowanie złośliwego sterownika |
| **VBS/HVCI (Windows)**, **Credential Guard** | integralność kodu jądra egzekwowana przez hypervisor | kradzież poświadczeń, kod jądra |
| **Memory tagging (MTE), języki bezpieczne pamięciowo (Rust)** | wykrywanie błędów pamięci, eliminacja klasy błędów | use-after-free, przepełnienia |

### Przykład podatności – przepełnienie bufora *(uzupełnienie)*

```c
void f(char *input) {
    char buf[16];
    strcpy(buf, input);     // brak sprawdzenia długości → nadpisanie sąsiednich danych i adresu powrotu
}
```

Bez mechanizmów (NX, ASLR, canary) atakujący nadpisuje adres powrotu i przekierowuje wykonanie. Z mechanizmami musi je obejść (informacja o adresach, ROP) – podnosi to koszt. Obrona kodu: funkcje bezpieczne (`strncpy`/`snprintf`), kompilacja z `-fstack-protector-strong -D_FORTIFY_SOURCE=2 -fPIE -pie -Wl,-z,relro,-z,now`, bezpieczne języki.

## Ataki na jądro i separację

| Atak | Opis |
| :--- | :--- |
| **Eskalacja uprawnień (privilege escalation)** | wykorzystanie błędu w jądrze/sterowniku/usłudze SUID, by uzyskać root/SYSTEM |
| **Rootkit (wykład W2, slajd 50)** | złośliwe oprogramowanie zwiększające uprawnienia i ukrywające się; **zmienia kod jądra i jego moduły**, więc wykrycie jest trudne; większość ataków wymaga uprawnień administratora; metody kontroli: **behawioralne, skanowanie sygnatur, skanowanie różnic, analiza zrzutu pamięci**; usunięcie bywa niemożliwe – często **reinstalacja systemu** |
| **Złośliwy / podatny sterownik (BYOVD)** | ładowanie podpisanego, ale podatnego sterownika w celu wyłączenia zabezpieczeń |
| **Ucieczka z kontenera/VM** | błędy izolacji → dostęp do hosta |
| **Ataki sprzętowe** | Spectre/Meltdown (kanały boczne spekulacji), Rowhammer, DMA przez Thunderbolt/FireWire (obrona: IOMMU) |
| **Ataki typu cold boot, Evil Maid** | zob. temat 8 |

## Dobre praktyki

aktualizacje jądra i sterowników, **Secure Boot + podpisane moduły**, wyłączenie nieużywanych modułów i sterowników, **sysctl hardening** (`kernel.kptr_restrict`, `kernel.dmesg_restrict`, `kernel.yama.ptrace_scope`, `kernel.randomize_va_space=2`, `kernel.unprivileged_bpf_disabled`), **LSM (SELinux/AppArmor)**, **seccomp/capabilities** dla usług, kontenery bez roota, IOMMU, zasada najmniejszych uprawnień dla sterowników i usług, monitorowanie integralności i ładowania modułów (auditd, EDR).

## Podsumowanie

- **Separacja:** tryb użytkownika (ring 3) vs jądra (ring 0), **wywołania systemowe** jako jedyna brama, **własne przestrzenie adresowe** procesów, IPC kontrolowane, dodatkowo namespaces, seccomp, integrity levels, hypervisor.
- **Ochrona pamięci:** MMU i uprawnienia stron (R/W/X), **NX/DEP, ASLR/KASLR, stack canary, CFG/CET, SMEP/SMAP, KPTI**, podpisy sterowników i Secure Boot.
- Kompromitacja jądra = kompromitacja systemu (rootkity); utwardzone jądro (ASLR, PaX, sandboxing, ograniczenia, rozszerzony audyt – wykład W8) zmniejsza ryzyko.

---
[⬅️ Poprzedni temat](2_Architektura_systemu_operacyjnego_z_punktu_widzenia_bezpieczeństwa.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](4_Zarządzanie_użytkownikami_uprawnieniami_i_kontrolą_dostępu_w_systemach_operacyjnych.md)