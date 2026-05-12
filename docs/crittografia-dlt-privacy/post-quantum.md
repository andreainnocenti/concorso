# Crittografia Post-Quantum

## 📖 Introduzione

I computer quantistici minacciano la crittografia attuale. La crittografia post-quantum prepara la transizione.

## ⚛️ Minaccia Quantistica

### Algoritmo di Shor
Fattorizza numeri grandi in tempo polinomiale.

**Minaccia:**
- RSA ❌
- ECC ❌
- DSA ❌

### Algoritmo di Grover
Ricerca in database non ordinato.

**Impatto:**
- AES-128 → AES-256 (raddoppia chiave)
- SHA-256 → SHA-512

## 🛡️ Algoritmi Post-Quantum

### Lattice-Based Cryptography

Basata su problemi difficili nei reticoli.

**Algoritmi:**
- CRYSTALS-Kyber (key exchange)
- CRYSTALS-Dilithium (firma digitale)

### Hash-Based Signatures

Basate su funzioni hash.

**Algoritmi:**
- SPHINCS+
- XMSS

### Code-Based Cryptography

Basata su codici correttori di errori.

**Algoritmi:**
- Classic McEliece

### Multivariate Cryptography

Sistemi di equazioni polinomiali.

## 📊 NIST Post-Quantum Standards

**Selezionati (2022):**
1. CRYSTALS-Kyber (encryption)
2. CRYSTALS-Dilithium (signature)
3. FALCON (signature)
4. SPHINCS+ (signature)

## 🔄 Transizione

### Hybrid Approach

Combinare algoritmi classici e post-quantum.

```
TLS 1.3 + Kyber = Hybrid Key Exchange
```

### Timeline

- **2024-2025**: Standard finali
- **2025-2030**: Adozione graduale
- **2030+**: Deprecazione algoritmi classici

## 🔗 Collegamenti
- **Precedente:** [Interoperabilità](interoperabilita.md)
- **Sezione:** [Crittografia, DLT, Privacy](index.md)
