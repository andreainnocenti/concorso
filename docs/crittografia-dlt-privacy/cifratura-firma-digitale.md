# Cifratura a Chiave Pubblica e Privata, Firma Digitale, Meccanismi di Integrità e Autenticazione

## 📖 Introduzione

La crittografia moderna garantisce quattro proprietà fondamentali nelle comunicazioni digitali:

- **Confidenzialità**: Solo destinatari autorizzati possono leggere i dati
- **Integrità**: I dati non sono stati modificati
- **Autenticità**: L'identità del mittente è verificabile
- **Non ripudio**: Il mittente non può negare di aver inviato il messaggio

---

## 🔐 Crittografia Simmetrica vs Asimmetrica

### Crittografia Simmetrica

Usa la **stessa chiave** per cifrare e decifrare. È veloce ed efficiente per grandi volumi di dati.

**Algoritmi principali:**
- **AES (Advanced Encryption Standard)**: Standard attuale, chiavi 128/192/256 bit
- **ChaCha20**: Alternativa moderna, ottima per mobile e dispositivi senza hardware AES

**Vantaggi:** Velocità elevata, ideale per cifrare file e comunicazioni
**Svantaggio:** Problema della distribuzione sicura della chiave condivisa

**Esempio uso:** Cifratura disco, VPN, backup crittografati

### Crittografia Asimmetrica

Usa una **coppia di chiavi**: pubblica (per cifrare) e privata (per decifrare).

**Algoritmi principali:**
- **RSA**: Basato su fattorizzazione di numeri primi, chiavi 2048-4096 bit
- **ECC (Elliptic Curve Cryptography)**: Curve ellittiche, chiavi più corte (256 bit = sicurezza RSA 3072 bit)

**Vantaggi:** Risolve il problema della distribuzione chiavi, permette firma digitale
**Svantaggio:** Molto più lenta della simmetrica

**Approccio ibrido (usato in pratica):**
1. Asimmetrica per scambiare una chiave di sessione
2. Simmetrica per cifrare i dati effettivi
3. Esempio: TLS usa questo approccio

---

## ✍️ Firma Digitale

La firma digitale è l'equivalente crittografico della firma autografa, ma con garanzie matematiche più forti.

### Come Funziona

**Processo di firma:**
```
1. Calcola hash del documento: h = SHA-256(documento)
2. Cifra l'hash con chiave privata: firma = Sign(chiave_privata, h)
3. Invia documento + firma
```

**Processo di verifica:**
```
1. Ricalcola hash: h' = SHA-256(documento_ricevuto)
2. Decifra firma con chiave pubblica: h_verificato = Verify(chiave_pubblica, firma)
3. Se h' = h_verificato → firma valida
```

**Perché si firma l'hash e non il documento intero?**
- Efficienza: l'hash è sempre 256 bit, il documento può essere gigabyte
- RSA può cifrare solo dati piccoli
- L'hash garantisce che qualsiasi modifica al documento invalida la firma

### Algoritmi di Firma

**RSA con PSS (Probabilistic Signature Scheme):**
- Standard consolidato
- Chiavi 2048-4096 bit
- Usato in certificati digitali, PDF firmati

**ECDSA (Elliptic Curve Digital Signature Algorithm):**
- Firme più corte
- Più veloce di RSA
- Usato in Bitcoin, Ethereum

**Ed25519 (Edwards-curve):**
- Più moderno e sicuro
- Deterministico (no vulnerabilità da RNG debole)
- Usato in SSH, Signal, Git

### Proprietà Garantite

1. **Autenticità**: Solo chi possiede la chiave privata può creare la firma
2. **Integrità**: Qualsiasi modifica al documento invalida la firma
3. **Non ripudio**: Il firmatario non può negare di aver firmato (la chiave privata è solo sua)

---

## #️⃣ Funzioni Hash Crittografiche

Una funzione hash trasforma input di qualsiasi dimensione in output di dimensione fissa (digest).

### Proprietà Richieste

1. **Deterministica**: Stesso input → stesso output sempre
2. **One-way**: Impossibile invertire (dato hash, non si può recuperare l'input)
3. **Collision-resistant**: Impossibile trovare due input diversi con stesso hash
4. **Avalanche effect**: Piccola modifica input → hash completamente diverso

**Esempio:**
```
SHA-256("hello") = 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824
SHA-256("hallo") = d3751d33f9cd5049c4af2b462735457e4d3baf130bcbb87f389e349fbaeb20b9
                   ↑ Una sola lettera cambiata, hash totalmente diverso
```

### Algoritmi Hash Moderni

**SHA-256 (SHA-2 family):**
- Output: 256 bit
- Standard attuale per integrità e blockchain
- Usato in: Bitcoin, TLS, certificati digitali

**SHA-3 (Keccak):**
- Struttura diversa da SHA-2 (sponge construction)
- Alternativa sicura, resistente a certi attacchi
- Usato in: Ethereum (variante Keccak-256)

**BLAKE2:**
- Più veloce di SHA-2 e SHA-3
- Sicurezza equivalente
- Usato in: Argon2 (password hashing)

### Applicazioni Pratiche

**1. Integrità dei dati:**
```bash
# Calcolo checksum di un file
sha256sum file.iso
# Verifica che il file scaricato non sia corrotto
```

**2. Password storage (con algoritmi dedicati):**
- ❌ MAI usare SHA-256 diretto per password (vulnerabile a rainbow tables)
- ✅ Usare **bcrypt** o **Argon2** (lenti di proposito, con salt automatico)

**3. HMAC (Hash-based Message Authentication Code):**
- Combina hash + chiave segreta
- Garantisce autenticità e integrità
- Usato in: JWT, API authentication, TLS

**4. Blockchain:**
- Ogni blocco contiene hash del blocco precedente
- Modifica un blocco → invalida tutta la catena successiva

---

## 🔒 Meccanismi di Autenticazione

### Challenge-Response

Protocollo per autenticare senza trasmettere password in chiaro:

```
1. Client: "Voglio autenticarmi come Alice"
2. Server: Invia challenge (numero random)
3. Client: Firma il challenge con chiave privata
4. Server: Verifica firma con chiave pubblica di Alice
```

**Vantaggi:**
- Password/chiave mai trasmessa
- Resistente a replay attacks (challenge sempre diverso)
- Usato in: SSH, smart card

### Certificati Digitali (X.509)

Un certificato digitale è come una "carta d'identità digitale" che lega una chiave pubblica a un'identità.

**Contenuto certificato:**
- Identità (nome dominio, organizzazione)
- Chiave pubblica
- Periodo di validità
- Firma della Certificate Authority (CA)

**Catena di fiducia:**
```
Root CA (auto-firmato, nel browser)
    ↓ firma
Intermediate CA
    ↓ firma
Certificato sito web (example.com)
```

**Verifica certificato:**
1. Controlla firma con chiave pubblica della CA
2. Verifica validità temporale
3. Controlla che non sia stato revocato (CRL o OCSP)

**Uso pratico:** HTTPS (il lucchetto nel browser verifica il certificato del sito)

### Multi-Factor Authentication (MFA)

Combina più fattori di autenticazione:

1. **Qualcosa che sai**: Password, PIN
2. **Qualcosa che hai**: Smartphone (app authenticator), token hardware
3. **Qualcosa che sei**: Impronta digitale, riconoscimento facciale

**TOTP (Time-based One-Time Password):**
- Codice di 6 cifre valido 30 secondi
- Generato da: `HMAC-SHA1(chiave_segreta, timestamp/30)`
- Usato in: Google Authenticator, Authy

**Vantaggio MFA:** Anche se la password viene rubata, l'attaccante non può accedere senza il secondo fattore

---

## 🛡️ TLS 1.3: Crittografia in Pratica

TLS (Transport Layer Security) è il protocollo che protegge le comunicazioni web (HTTPS).

### Handshake TLS 1.3

```
Client → Server: ClientHello (cipher supportati, chiave pubblica)
Server → Client: ServerHello (cipher scelto, chiave pubblica, certificato)
                 [Tutto cifrato da qui in poi]
Client → Server: Verifica certificato, genera chiave di sessione
═══════════════════════════════════════════════════════════════
        Comunicazione cifrata con chiave simmetrica
```

**Miglioramenti vs TLS 1.2:**
- Handshake più veloce (1-RTT invece di 2-RTT)
- Solo algoritmi sicuri (rimossi MD5, SHA-1, RC4, RSA key exchange)
- Forward secrecy obbligatoria (ECDHE)
- Handshake cifrato (più privacy)

**Cipher suite moderna:**
```
TLS_AES_256_GCM_SHA384
 │    │    │    │
 │    │    │    └─ Hash per HMAC
 │    │    └────── Modo autenticato (GCM)
 │    └─────────── Cifrario simmetrico
 └──────────────── Protocollo
```

---

## 📊 Confronto e Raccomandazioni

### Quando Usare Cosa

| Obiettivo | Soluzione | Algoritmo |
|-----------|-----------|-----------|
| Cifrare file/dati | Simmetrica | AES-256-GCM |
| Scambiare chiavi | Asimmetrica | ECDH (Curve25519) |
| Firmare documenti | Firma digitale | Ed25519 o RSA-PSS |
| Verificare integrità | Hash | SHA-256 |
| Autenticare API | HMAC | HMAC-SHA256 |
| Salvare password | Password hashing | Argon2id o bcrypt |

### Raccomandazioni 2024

**✅ Algoritmi sicuri:**
- Simmetrica: AES-256, ChaCha20
- Asimmetrica: ECC (P-256, Curve25519), RSA ≥3072 bit
- Firma: Ed25519, ECDSA, RSA-PSS
- Hash: SHA-256, SHA-3, BLAKE2

**❌ Algoritmi obsoleti (da evitare):**
- DES, 3DES (chiavi troppo corte)
- MD5, SHA-1 (collisioni trovate)
- RSA <2048 bit (fattorizzabile)
- RC4 (vulnerabilità note)

---

## 🎯 Domande Tipiche per Esame

### 1. Differenza tra cifratura e firma digitale

**Risposta:**
- **Cifratura**: Chiave pubblica cifra, privata decifra → garantisce **confidenzialità**
- **Firma**: Chiave privata firma, pubblica verifica → garantisce **autenticità e integrità**

Sono operazioni inverse che usano le stesse chiavi ma per scopi opposti.

### 2. Perché TLS usa sia crittografia simmetrica che asimmetrica?

**Risposta:**
TLS usa un approccio ibrido per combinare i vantaggi di entrambe:
- **Asimmetrica (ECDHE)**: Scambia in modo sicuro una chiave di sessione, senza doverla trasmettere
- **Simmetrica (AES)**: Cifra i dati effettivi, perché è molto più veloce

Questo permette di avere sia sicurezza (key exchange sicuro) che performance (cifratura veloce).

### 3. Cos'è la forward secrecy e perché è importante?

**Risposta:**
Forward secrecy garantisce che anche se la chiave privata del server viene compromessa in futuro, le comunicazioni passate rimangono sicure.

Si ottiene usando **chiavi di sessione effimere** (generate per ogni connessione e poi distrutte) invece di usare sempre la stessa chiave del certificato.

Algoritmo: ECDHE (Elliptic Curve Diffie-Hellman Ephemeral)

### 4. Differenza tra HMAC e firma digitale

**Risposta:**
- **HMAC**: Usa chiave simmetrica condivisa, più veloce, non offre non-ripudio
- **Firma digitale**: Usa chiave asimmetrica, più lenta, offre non-ripudio

HMAC è ideale quando mittente e destinatario si fidano (es. API interne).
Firma digitale quando serve provare a terzi chi ha firmato (es. contratti).

### 5. Perché non si usa SHA-256 per salvare password?

**Risposta:**
SHA-256 è troppo veloce: un attaccante può provare miliardi di password al secondo.

Si usano algoritmi **lenti di proposito** come bcrypt o Argon2, che:
- Richiedono molta memoria e CPU
- Includono salt automatico (previene rainbow tables)
- Sono configurabili (si può aumentare il costo nel tempo)

Esempio: bcrypt con 12 rounds richiede ~0.3 secondi per hash, rendendo impraticabile il brute force.

---

## 🔗 Collegamenti

- **Successivo:** [Architetture DLT e Blockchain](architetture-dlt.md)
- **Correlato:** [Smart Contracts](smart-contracts.md)
- **Correlato:** [Privacy e Anonimato](privacy-anonimato.md)
