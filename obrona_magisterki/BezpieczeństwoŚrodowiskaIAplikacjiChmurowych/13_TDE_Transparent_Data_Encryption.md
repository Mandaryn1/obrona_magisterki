# Czym jest mechanizm TDE?

**TDE (Transparent Data Encryption)** to mechanizm **przezroczystego szyfrowania danych w spoczynku na poziomie bazy danych**. Baza automatycznie szyfruje **pliki danych, logi transakcji i kopie zapasowe** przy zapisie na dysk i odszyfrowuje je przy odczycie do pamięci. Jest „transparentny", bo **aplikacje i użytkownicy nie muszą nic zmieniać**: zapytania SQL działają tak samo, a szyfrowaniem zajmuje się silnik bazy. Stosują go m.in. SQL Server, Oracle, MySQL, a w chmurze usługi typu Azure SQL czy AWS RDS.

**Jak działa (hierarchia kluczy):**

- Dane szyfruje klucz **DEK (Data Encryption Key)**, symetryczny (np. AES-256).
- DEK jest chroniony przez **klucz nadrzędny (KEK / master key)**, często w usłudze **KMS lub HSM** (np. Azure Key Vault, AWS KMS), z rotacją.

**Co chroni:** przed odczytem danych przez kogoś, kto zdobędzie **pliki bazy, dysk lub kopię zapasową** (kradzież nośnika, nieautoryzowany dostęp do magazynu). Pomaga też spełnić wymagania zgodności (RODO, PCI DSS).

**Czego nie chroni:** TDE **nie chroni przed uprawnionym dostępem**. Użytkownik, który pyta bazę przez SQL, widzi dane jawne. Nie chroni też przed atakami typu SQL Injection ani przed administratorem bazy. Dane nie są szyfrowane w tranzycie (potrzebny TLS) ani w pamięci. Do ochrony wybranych kolumn stosuje się dodatkowo szyfrowanie na poziomie kolumn lub aplikacji.

**Zalety:** prostota wdrożenia, brak zmian w aplikacjach, niewielki narzut wydajności. **Kluczowe** jest bezpieczne zarządzanie kluczami, bo bez klucza nie da się odzyskać danych.

## Podsumowanie

- **TDE** = automatyczne, przezroczyste dla aplikacji szyfrowanie plików bazy danych (dane w spoczynku), z kluczem chronionym w KMS/HSM.
- Chroni przed kradzieżą nośnika/kopii/snapshotu, **nie** przed uprawnionym dostępem i atakami na poziomie aplikacji.
- Uzupełnia się o RBAC, szyfrowanie kolumnowe, TLS i audyt; wymaga dobrego zarządzania kluczami.

---
[⬅️ Poprzedni temat](12_VPC_Virtual_Private_Cloud_w_bezpieczeństwie_chmury.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](14_Data_minimization_minimalizacja_danych.md)