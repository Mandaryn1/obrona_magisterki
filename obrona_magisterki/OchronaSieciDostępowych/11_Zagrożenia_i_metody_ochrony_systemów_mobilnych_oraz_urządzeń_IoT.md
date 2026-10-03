# Zagrożenia i metody ochrony systemów mobilnych oraz urządzeń IoT

### Systemy mobilne (smartfony, tablety, laptopy)

*Zagrożenia:*
- **utrata lub kradzież** urządzenia (wyciek danych),
- **malware mobilne** i złośliwe aplikacje, **phishing i smishing**,
- **niezaufane sieci** (publiczne Wi-Fi, fałszywe AP, MITM),
- **nadmierne uprawnienia aplikacji**, brak aktualizacji,
- **BYOD** (prywatne urządzenia w sieci firmowej), **jailbreak/root**,
- juice jacking (złośliwe ładowarki), podglądanie ekranu.

*Ochrona:*
- **MDM (Mobile Device Management)/EMM** (np. Cisco Meraki): wymuszanie konfiguracji, aktualizacji i szyfrowania, monitoring, **zdalne wymazanie** (w BYOD selektywne, tylko danych firmowych),
- blokada ekranu, **MFA**, szyfrowanie urządzenia,
- **ocena postury** i **NAC**: urządzenie niezgodne z polityką nie dostaje pełnego dostępu,
- **VPN/ZTNA** dla dostępu zdalnego, antywirus/EDR, kontrola aplikacji i uprawnień,
- szkolenia użytkowników.

### Urządzenia IoT

*Specyfika:* ograniczone zasoby (moc, pamięć, zasilanie), brak standaryzacji, **długi cykl życia (10–15 lat)**, często brak interfejsu użytkownika, ogromna skala i fizyczny dostęp.

*Zagrożenia:*
- **słabe i domyślne hasła**, nieaktualne firmware i brak łatek,
- **botnety** (np. **Mirai**, ok. 600 tys. urządzeń, DDoS na Dyn w 2016 r.),
- naruszenia **prywatności** (kamery, asystenci, urządzenia domowe),
- ataki na IIoT/SCADA (np. **Stuxnet**) i urządzenia medyczne (pompy insulinowe, rozruszniki),
- brak szyfrowania, niezabezpieczone interfejsy, słaby łańcuch dostaw,
- ataki fizyczne.

*Ochrona:*
- **bezpieczne uwierzytelnianie:** unikalne hasła, brak haseł domyślnych, certyfikaty,
- **aktualizacje OTA** z podpisem cyfrowym i **secure boot**,
- **TPM/TEE** do przechowywania kluczy,
- **lekka kryptografia** (ChaCha20, AES-128, ECC) i bezpieczne protokoły (**MQTT z TLS**, **CoAP z DTLS**, WPA3),
- **segmentacja** (osobny VLAN dla IoT), **NAC/802.1X**, zasada **Zero Trust**,
- **monitoring i wykrywanie anomalii (ML)**, SIEM/SOAR,
- standardy i regulacje: **OWASP IoT Top 10**, **ETSI EN 303 645**, **unijny Cyber Resilience Act** (m.in. wsparcie aktualizacji, zgłaszanie podatności).

**Wniosek:** w obu przypadkach kluczowe są **zarządzanie urządzeniami, silne uwierzytelnianie, aktualizacje, szyfrowanie, segmentacja i monitoring**. Urządzeń nie można traktować jako „zaufanych" tylko dlatego, że są w sieci firmowej.

## Podsumowanie

- **Systemy mobilne:** zagrożenia – utrata/kradzież, malware, niezaufane sieci, phishing, BYOD; ochrona – **MDM** (konfiguracja, monitoring, aktualizacje, zdalne wymazanie, szyfrowanie), **NAC z oceną postury**, patching agentowy, polityki BYOD, MTD.
- **IoT:** zagrożenia – ograniczone zasoby, domyślne hasła, brak aktualizacji, **botnety (Mirai)**, prywatność, IIoT/medyczne; ochrona – **lekka kryptografia, TPM/TEE i secure boot, uwierzytelnianie urządzeń, bezpieczne protokoły (TLS/DTLS, WPA3), segmentacja VLAN i Zero Trust, monitoring i IDS, bezpieczne OTA, polityka end-of-life**, regulacje (ETSI 303 645, CRA, RODO).

---
[⬅️ Poprzedni temat](10_Systemy_zarządzania_bezpieczeństwem_informacji_w_ochronie_sieci_lokalnych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../BezpieczeństwoSieciTeleinformatycznych/BezpieczeństwoSieciTeleinformatycznych_tytul.md)