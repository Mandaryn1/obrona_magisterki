# Na czym polega szyfrowanie danych w spoczynku i w tranzycie?

**Szyfrowanie danych w spoczynku (at rest)** chroni dane **przechowywane** na dyskach, w bazach danych, magazynach obiektowych i kopiach zapasowych. Jeśli ktoś zdobędzie nośnik lub dostęp do magazynu, nie odczyta danych bez klucza. Stosuje się symetryczne szyfrowanie **AES-256**: szyfrowanie dysków i wolumenów, szyfrowanie baz (**TDE**), szyfrowanie po stronie serwera w magazynach (np. S3). **Klucze** przechowuje się osobno, w usługach **KMS** lub sprzętowych modułach **HSM**, z rotacją i kontrolą dostępu. Można też użyć kluczy zarządzanych przez klienta (CMK, BYOK).

**Szyfrowanie danych w tranzycie (in transit)** chroni dane **przesyłane przez sieć**: między użytkownikiem a chmurą, między usługami i między centrami danych. Zapobiega podsłuchowi i modyfikacji, np. w ataku man-in-the-middle. Stosuje się **TLS/HTTPS** (aktualnie TLS 1.2 lub 1.3), **VPN (IPsec)**, SSH oraz **mTLS** w komunikacji między usługami. Wymaga to certyfikatów i ich zarządzania. W TLS asymetryczna kryptografia służy do uzgodnienia klucza, a dane szyfruje się szybko algorytmem symetrycznym.

**Dodatkowo** wspomina się o **szyfrowaniu danych w użyciu** (podczas przetwarzania w pamięci), np. confidential computing, które dopiero się rozwija.

**Dlaczego to ważne w chmurze:** dane przechodzą przez wiele sieci i leżą na współdzielonej infrastrukturze, a szyfrowanie jest wymagane przez regulacje (RODO, PCI DSS). Kluczowe jest dobre **zarządzanie kluczami**, bo szyfrowanie jest tak silne, jak ochrona klucza.

## Podsumowanie

- **W spoczynku:** szyfrowanie dysków, baz (TDE), pól, kopii zapasowych; klucze **oddzielnie** od danych, **rotowane**, w KMS/HSM.
- **W tranzycie:** **TLS** (najnowsze wersje), HTTPS wszędzie, mTLS między usługami, VPN; certyfikaty zarządzane centralnie i odnawiane; monitorowanie prób obejścia.
- **W użyciu:** SGX/SEV (confidential computing) – najtrudniejsze, najsilniejsze.
- Szyfrowanie wymaga planowania, testów wydajności i **dobrego zarządzania kluczami**.

---
[⬅️ Poprzedni temat](8_Mechanizmy_RBAC_i_ABAC.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](10_RPO_Recovery_Point_Objective.md)