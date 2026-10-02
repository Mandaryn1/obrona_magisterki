# Bezpieczeństwo środowiska i aplikacji chmurowych – wprowadzenie

> **Źródło:** opracowanie oparte na wykładzie „Bezpieczeństwo środowiska i aplikacji chmurowych" (P. Kopniak; 337 slajdów). Za podstawę zagadnień służą slajdy **7–37** (zasady i pojęcia bezpieczeństwa chmury), uzupełnione wybranymi slajdami z dalszej części (TDE, szyfrowanie, backup, VPC, Zero Trust, DevSecOps). Fragmenty oznaczone ***(uzupełnienie)*** pochodzą spoza wykładu.
>
> **Uwaga:** wykład ma tylko **wzmiankę** o OWASP Top 10 (slajdy 66, 122), a RPO/RTO – jedno zdanie (slajd 26). Te zagadnienia (10, 11, 15) są więc opracowane głównie z uzupełnień.

## Zakres wykładu

Plan wykładu (slajd 6): (1) zasady i pojęcia bezpieczeństwa chmury, (2) budowa aplikacji chmurowych w Spring, (3) narzędzia Spring Cloud, (4) Spring Security, (5) zarządzanie zasobami danych, (6) zarządzanie zasobami chmury, (7) tożsamość i dostęp, (8) podatności, (9) bezpieczeństwo sieci, (10) wykrywanie, reagowanie i odzyskiwanie po incydentach, (11) przechowywanie kluczy i certyfikatów.

Kluczowe tezy wstępu:

- bezpieczeństwo chmury wymaga zrozumienia **współdzielonej odpowiedzialności** dostawcy i klienta,
- architekturę bezpieczeństwa trzeba przygotować **od początku projektu**, nie jako dodatek na końcu,
- chmura **nie jest po prostu innym miejscem uruchamiania aplikacji**, lecz fundamentalnie innym modelem obliczeniowym,
- podejście musi być **holistyczne** – wszystkie warstwy, od infrastruktury po aplikacje.

## Mapa: slajdy → zagadnienia z listy

| Nr | Zagadnienie | Główne slajdy |
| :-: | :--- | :--- |
| 1 | Cechy chmury wg NIST | 8–9 |
| 2 | Główne zagrożenia bezpieczeństwa chmury | 12, 36 |
| 3 | Modele chmur | 10–11 |
| 4 | Czy chmura wymaga innego podejścia | 4–5, 36 |
| 5 | Model odpowiedzialności (PaaS) | 10, 13 |
| 6 | IAM | 14, 16, 18–20 |
| 7 | MFA | 15 |
| 8 | RBAC i ABAC | 17 |
| 9 | Szyfrowanie w spoczynku i w tranzycie | 22–24, 220–223 |
| 10 | RPO | 26 (+ 313–315) |
| 11 | RTO | 26 (+ 313–315) |
| 12 | VPC | 27, 299, 331 |
| 13 | TDE | 22, 221, 310 |
| 14 | Data minimization | 35, 189 |
| 15 | OWASP Top 10 | 66, 122, 216–217, 237 |

## Słowniczek skrótów

| Skrót | Znaczenie |
| :--- | :--- |
| **NIST** | National Institute of Standards and Technology |
| **IaaS / PaaS / SaaS** | Infrastructure / Platform / Software as a Service |
| **IAM** | Identity and Access Management |
| **MFA** | Multi-Factor Authentication |
| **SSO** | Single Sign-On |
| **SAML / OAuth 2.0 / OIDC** | standardy federacji tożsamości i autoryzacji |
| **RBAC / ABAC** | Role-Based / Attribute-Based Access Control |
| **KMS / HSM** | Key Management Service / Hardware Security Module |
| **TLS** | Transport Layer Security |
| **TDE** | Transparent Data Encryption |
| **DLP** | Data Loss Prevention |
| **RPO / RTO** | Recovery Point / Recovery Time Objective |
| **DR** | Disaster Recovery |
| **VPC / VNet** | Virtual Private Cloud (AWS, GCP) / Virtual Network (Azure) |
| **SG / NACL** | Security Group / Network ACL |
| **WAF / NGFW** | Web Application Firewall / Next-Generation Firewall |
| **SIEM** | Security Information and Event Management |
| **GDPR / RODO** | General Data Protection Regulation |
| **PII** | Personally Identifiable Information |
| **CCSP** | Certified Cloud Security Professional |
| **SAST / DAST** | Static / Dynamic Application Security Testing |
| **SBOM** | Software Bill of Materials |
| **OWASP** | Open Worldwide Application Security Project |
