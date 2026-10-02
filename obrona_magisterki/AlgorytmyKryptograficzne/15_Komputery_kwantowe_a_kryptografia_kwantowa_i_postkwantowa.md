# Omów ogólnie, jakie wyzwania dla współczesnej kryptografii wiążą się z rozwojem komputerów kwantowych. Czym różni się kryptografia kwantowa od kryptografii postkwantowej?

**Wyzwania, jakie stwarzają komputery kwantowe:**

- **Algorytm Shora** pozwala na komputerze kwantowym w czasie wielomianowym rozkładać liczby na czynniki i liczyć logarytm dyskretny. Łamie więc **RSA, Diffiego-Hellmana, ElGamala i ECC**, czyli prawie całą dzisiejszą kryptografię klucza publicznego: wymianę kluczy i podpisy cyfrowe. Zwiększanie długości klucza nie pomaga.
- **Algorytm Grovera** przyspiesza przeszukiwanie, więc efektywną siłę kluczy symetrycznych zmniejsza mniej więcej o połowę (AES-128 daje ok. 64 bity). To da się naprawić prosto: stosuje się **AES-256**, a skróty z dłuższym wynikiem.
- **Zagrożenie „harvest now, decrypt later":** przeciwnik może już dziś zbierać zaszyfrowane dane, a odszyfrować je za kilkanaście lat. Dlatego migrację trzeba zacząć wcześniej, szczególnie dla danych, które muszą długo pozostać tajne.

**Kryptografia kwantowa a postkwantowa:**

- **Kryptografia kwantowa** wykorzystuje **prawa fizyki kwantowej**. Najważniejszym przykładem jest **QKD (np. protokół BB84)**: klucz przesyła się w stanach pojedynczych fotonów. Podsłuch zaburza stan fotonów (pomiar zmienia stan, a kwantowego stanu nie da się sklonować), więc można go wykryć. Wymaga **specjalnego sprzętu** (łącza światłowodowe, ograniczony zasięg) i uwierzytelnionego kanału klasycznego. Służy tylko do uzgadniania klucza, nie do podpisów.
- **Kryptografia postkwantowa (PQC)** to zwykłe **algorytmy klasyczne**, działające na zwykłych komputerach, oparte na problemach uważanych za trudne także dla komputerów kwantowych: kraty, kody korekcyjne, funkcje skrótu. Nie wymaga nowego sprzętu, więc można ją wdrożyć w istniejących systemach.

**Standardy NIST (sierpień 2024):** **ML-KEM** (FIPS 203, uzgadnianie kluczy), **ML-DSA** (FIPS 204) i **SLH-DSA** (FIPS 205) (podpisy). Wdraża się je często **hybrydowo**, razem z algorytmami klasycznymi, a ważna jest **kryptoagilność**, czyli możliwość łatwej wymiany algorytmów.

**W skrócie:** kryptografia kwantowa używa fizyki, żeby bezpiecznie wymienić klucz, a postkwantowa to nowe algorytmy matematyczne, które mają przetrwać komputery kwantowe, i to ona jest praktycznym rozwiązaniem problemu.

## Podsumowanie

- **Zagrożenie:** algorytm **Shora** łamie **RSA, DH, ElGamal, ECC** (wielomianowo); algorytm **Grovera** zmniejsza efektywną siłę kluczy symetrycznych i skrótów o połowę – wystarczy **AES-256** i skróty ≥ 256 bitów.
- Szczególnie groźne: **„harvest now, decrypt later"** – dane zbierane dziś, odszyfrowane później.
- **Kryptografia kwantowa (QKD, BB84):** bezpieczeństwo z fizyki, dedykowany sprzęt, tylko dystrybucja klucza, wymaga uwierzytelnionego kanału.
- **Kryptografia postkwantowa:** klasyczne algorytmy odporne na kwanty (kraty: **ML-KEM, ML-DSA**; hash-based: **SLH-DSA**; kody: HQC); standardy NIST 2024; migracja i **kryptoagilność**, rozwiązania hybrydowe.

---
[⬅️ Poprzedni temat](14_Kryptografia_krzywych_eliptycznych.md) | [🏠 Powrót do spisu treści](../../README.md) | [Następny temat ➡️](../BezpieczeństwoWSieciachKomputerowych/BezpieczeństwoWSieciachKomputerowych_tytul.md)