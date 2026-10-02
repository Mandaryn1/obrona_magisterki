# Podstawowe cele bezpieczeństwa systemów operacyjnych i usług sieciowych

> Wykład: W8 (slajdy 2–7: cele, utwardzanie, najmniejsze uprawnienia, powierzchnia ataku), W2 (usługi, porty, zabezpieczanie urządzeń), MBK1 (znaczenie bezpieczeństwa OS). Zasady projektowe Saltzera i Schroedera – ***(uzupełnienie)***.

## Rola systemu operacyjnego

**System operacyjny** zarządza zasobami (procesor, pamięć, dyski, urządzenia, sieć), uruchamia procesy i pośredniczy w dostępie programów do zasobów. Wykład (MBK1, slajd 3): *systemy operacyjne stanowią fundament infrastruktury IT każdej organizacji; odpowiednia ochrona zapobiega nieautoryzowanemu dostępowi, kradzieży danych i atakom ransomware; zagrożenia ewoluują od prostych wirusów po ataki APT.*

**Usługa sieciowa** to proces (demon / usługa systemowa), który **nasłuchuje na porcie** i obsługuje żądania klientów (WWW, SSH, poczta, DNS, DHCP). Każda działająca usługa to potencjalny punkt wejścia – wykład W2 (slajd 20): *usługa „nasłuchuje" na porcie, gdy jest z nim powiązana; klienci używają dobrze znanych portów*; tabela m.in. 20/21 FTP, 22 SSH, 23 Telnet, 25 SMTP, 53 DNS, 67/68 DHCP, 69 TFTP, 80 HTTP, 110 POP3, 123 NTP, 143 IMAP, 161/162 SNMP, 443 HTTPS.

## Cele bezpieczeństwa (wykład W8, slajd 2)

| Cel | Znaczenie dla OS i usług | Przykładowe mechanizmy |
| :--- | :--- | :--- |
| **Poufność** | ochrona wrażliwych danych przed nieautoryzowanym dostępem i ujawnieniem | uprawnienia i ACL, szyfrowanie dysków i transmisji, izolacja procesów |
| **Integralność** | dane i konfiguracje nie są modyfikowane przez nieuprawnionych (osoby lub procesy) | uprawnienia zapisu, podpisy kodu, Secure Boot, kontrola integralności plików, audyt |
| **Dostępność** | stały dostęp do zasobów dla uprawnionych użytkowników i aplikacji | aktualizacje, redundancja, kopie zapasowe, ochrona przed DoS, monitoring |
| **Uwierzytelnienie** | weryfikacja tożsamości użytkowników i procesów przed przyznaniem dostępu | hasła, klucze, MFA, certyfikaty, Kerberos |

***(uzupełnienie)*** Rozszerzenia: **autoryzacja** (co wolno), **rozliczalność** (audyt – kto, co, kiedy), **niezaprzeczalność**, **autentyczność** oprogramowania (podpisy), **prywatność**. Łącznie: model **CIA + AAA**.

## Zasady (wykład W8: utwardzanie i najmniejsze uprawnienia)

**Utwardzanie systemu (hardening)** – zmniejszanie powierzchni ataku przez bezpieczną konfigurację. Pięć elementów (W8, slajd 5): **minimalizacja usług, ograniczenie uprawnień, kontrola dostępu, monitoring, aktualizacje.**

| System nieutwardzony | System utwardzony (W8, slajd 7) |
| :--- | :--- |
| wiele aktywnych usług i portów | minimalna liczba usług |
| domyślne konfiguracje | zoptymalizowane ustawienia bezpieczeństwa |
| niepotrzebne protokoły aktywne | wyłączone niepotrzebne protokoły |
| szeroka powierzchnia ataku | znacząco zredukowana powierzchnia ataku |

**Zasada najmniejszych uprawnień (W8, slajd 6):** każdy użytkownik, proces i aplikacja ma dostęp wyłącznie do niezbędnych zasobów – wymaga *precyzyjnej identyfikacji potrzeb, regularnego przeglądu uprawnień i audytu aktywności.*

### Zasady projektowe systemów bezpiecznych *(uzupełnienie – Saltzer i Schroeder)*

| Zasada | Sens |
| :--- | :--- |
| **fail-safe defaults** (domyślna odmowa) | dostęp tylko po jawnym zezwoleniu |
| **complete mediation** | każdy dostęp sprawdzany (pojęcie *reference monitor*) |
| **least privilege** | minimalne uprawnienia |
| **separation of privilege** | krytyczne akcje wymagają wielu warunków/osób |
| **economy of mechanism** | prostota (mały kod → mniej błędów) |
| **least common mechanism** | minimalizacja współdzielonych zasobów |
| **open design** | zasada Kerckhoffsa – bezpieczeństwo nie z tajności projektu |
| **psychological acceptability** | zabezpieczenia możliwe do stosowania przez ludzi |
| **defense in depth** | wiele niezależnych warstw |

## Zagrożenia dla OS i usług *(wykład W1 z poprzedniego przedmiotu + uzupełnienie)*

malware (wirusy, robaki, trojany, ransomware, rootkity – W2 slajd 49–50), wykorzystanie podatności usług (**wektorem ataku na Linuksa są głównie jego usługi** – W2), eskalacja uprawnień, brute force, błędna konfiguracja, brak aktualizacji (WannaCry/EternalBlue na SMBv1), zagrożenia wewnętrzne, ataki fizyczne (kradzież, *Evil Maid*), ataki na łańcuch dostaw.

## Podstawowe praktyki zabezpieczania serwerów i urządzeń (wykład W2, slajd 28)

- zapewnić **bezpieczeństwo fizyczne**,
- **zminimalizować liczbę zainstalowanych pakietów**, wyłączyć nieużywane usługi,
- **używać SSH**, wyłączyć logowanie root przez SSH,
- **aktualizować system** (każdego dnia odkrywane są nowe luki; producenci wydają poprawki),
- wyłączyć automatyczne wykrywanie USB, wymuszać silne hasła i definiować **role administracyjne**.

Usługi zarządzane są plikami konfiguracyjnymi (port, lokalizacja zasobów, autoryzacja klientów; zmiany zwykle wymagają restartu usługi; edycja – uprawnienia superużytkownika – W2 slajd 27). Przykład na laboratorium: **UFW blokuje niezabezpieczony Telnet i wyłączenie usługi Telnet** (W2 slajd 53).

## Cele bezpieczeństwa usług sieciowych – skrót

1. **uruchamiać tylko niezbędne usługi** i słuchać tylko na potrzebnych interfejsach,
2. **szyfrować komunikację** (SSH, HTTPS, SFTP) i uwierzytelniać klientów,
3. **ograniczyć dostęp zaporą** (host firewall + zapora sieciowa), segmentacja,
4. **aktualizować i konfigurować bezpiecznie** (bez domyślnych haseł),
5. **logować i monitorować** (kto się łączył, błędy),
6. **uruchamiać usługę z najmniejszymi uprawnieniami** (dedykowane konto, nie root/SYSTEM, izolacja, chroot/kontenery),
7. **ochrona dostępności** (rate limiting, odporność na DoS),
8. **kopie i odtwarzanie.**

## Podsumowanie

- Cele: **poufność, integralność, dostępność, uwierzytelnienie** (+ autoryzacja, rozliczalność); realizują je **uprawnienia i kontrola dostępu, szyfrowanie, aktualizacje, monitoring i audyt**.
- Podstawowa strategia: **utwardzanie** (minimalizacja usług, najmniejsze uprawnienia, kontrola dostępu, monitoring, aktualizacje) i redukcja **powierzchni ataku**.
- Usługi sieciowe = otwarte porty = wektory ataku; wymagają bezpiecznej konfiguracji, szyfrowania, zapór i monitoringu.
