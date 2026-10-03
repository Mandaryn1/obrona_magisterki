# Narzędzia służące do identyfikacji ataków na protokoły i usługi sieciowe

Narzędzia do identyfikacji ataków można podzielić według tego, co analizują i jak działają.

**1. Analiza pakietów (sniffery):**

- **Wireshark i tcpdump** przechwytują i dekodują ruch. Pozwalają zbadać protokoły, wykryć skanowanie, spoofing i niezaszyfrowane dane. Wymagają dostępu do ruchu (port SPAN lub TAP).

**2. Systemy wykrywania i zapobiegania włamaniom:**

- **NIDS/NIPS:** **Snort** i **Suricata** wykrywają ataki przez sygnatury, a IPS dodatkowo je blokuje.
- **HIDS:** **OSSEC/Wazuh** analizują logi, integralność plików i procesy na hoście.
- **NSM (monitorowanie bezpieczeństwa sieci):** **Zeek** tworzy logi metadanych połączeń (conn, dns, http) przydatne w analizie i threat huntingu.

**3. Monitoring przepływów:**

- **NetFlow/sFlow/IPFIX** pokazują, kto z kim i ile komunikuje się. Skutecznie wykrywają DDoS i anomalie, a także działają przy ruchu szyfrowanym.

**4. Zapory i filtry aplikacyjne:**

- **Zapory i NGFW** z logami, **WAF** dla aplikacji WWW.

**5. Korelacja i automatyzacja:**

- **SIEM** (np. Splunk, ELK, Wazuh) gromadzi logi z wielu źródeł i koreluje zdarzenia,
- **SOAR** automatyzuje reakcję.

**6. Skanery i testowanie:**

- **Nmap** (odkrywanie hostów i usług, także audyt własnej sieci), **Nessus/OpenVAS** (skanowanie podatności),
- narzędzia do testów: Metasploit, Scapy, hping.

**Metody wykrywania:**

- **sygnaturowa:** dokładna dla znanych ataków, ale bezradna wobec zero-day,
- **anomalii:** wykrywa nowe ataki kosztem fałszywych alarmów,
- **hybrydowa:** łączy oba podejścia.

**Wyzwania:** ruch szyfrowany (TLS) ogranicza analizę treści, fałszywe alarmy, wydajność przy dużym ruchu i to, że IPS inline może być pojedynczym punktem awarii.

**Wniosek:** skuteczna identyfikacja wymaga kilku narzędzi razem: sniffera do analizy szczegółów, IDS/IPS do wykrywania, NetFlow do widoczności i SIEM do korelacji.

## Podsumowanie

- Do identyfikacji ataków służą: **analizatory pakietów** (Wireshark, tcpdump), **NIDS/NIPS** (Snort, Suricata), **NSM** (Zeek), **HIDS/HIPS** (OSSEC/Wazuh), **analiza przepływów** (NetFlow/sFlow), **zapory/WAF**, **SIEM/SOAR**, skanery.
- Metody detekcji: **sygnaturowa, anomalii, hybrydowa**; **IDS wykrywa, IPS blokuje**.
- Główne wyzwania: szyfrowanie, fałszywe alarmy, wydajność, zero-day.

---
[⬅️ Poprzedni temat](6_Metody_detekcji_i_obrony_przed_atakami_w_warstwie_III_modelu_OSI.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](8_Monitorowanie_ruchu_sieciowego_w_wykrywaniu_nieprawidłowości_i_incydentów.md)