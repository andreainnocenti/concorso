# Privacy-Enhancing Technologies

## 📖 Introduzione

Tecnologie per proteggere la privacy degli utenti mantenendo funzionalità e verificabilità.

## 🔐 Zero-Knowledge Proofs (ZKP)

Dimostrare la verità di un'affermazione senza rivelare informazioni.

### zk-SNARKs

**Zero-Knowledge Succinct Non-Interactive Argument of Knowledge**

```python
# Esempio concettuale
def prove_age_over_18(age, secret):
    """Prova di avere >18 anni senza rivelare l'età esatta"""
    proof = generate_zksnark_proof(
        statement="age > 18",
        witness={"age": age, "secret": secret}
    )
    return proof

def verify_proof(proof):
    return verify_zksnark(proof, "age > 18")

# Alice prova di avere >18 anni
proof = prove_age_over_18(25, "secret_random")
is_valid = verify_proof(proof)  # True, ma età non rivelata
```

**Applicazioni:**
- Zcash (transazioni private)
- Tornado Cash (mixer)
- zkSync (rollup)

### zk-STARKs

**Scalable Transparent ARgument of Knowledge**

**Vantaggi vs SNARKs:**
- No trusted setup
- Quantum-resistant
- Più scalabili

## 🔒 Homomorphic Encryption

Computazione su dati cifrati.

```python
# Esempio concettuale
encrypted_a = encrypt(5)
encrypted_b = encrypt(3)

# Operazione su dati cifrati
encrypted_sum = homomorphic_add(encrypted_a, encrypted_b)

# Decifra risultato
result = decrypt(encrypted_sum)  # 8
```

**Tipi:**
- **Partially Homomorphic**: una operazione
- **Somewhat Homomorphic**: operazioni limitate
- **Fully Homomorphic**: operazioni illimitate

## 🤝 Secure Multi-Party Computation (MPC)

Computazione distribuita senza rivelare input.

```
Party A (input: x) ─┐
                    ├→ Compute f(x,y,z) → Output
Party B (input: y) ─┤
                    │
Party C (input: z) ─┘

Nessuno conosce gli input degli altri
```

**Applicazioni:**
- Aste private
- Votazioni elettroniche
- Wallet multi-sig

## 📊 Differential Privacy

Aggiunta di rumore per proteggere dati individuali.

```python
import numpy as np

def add_laplace_noise(data, epsilon):
    """Differential privacy con meccanismo Laplace"""
    sensitivity = 1.0
    scale = sensitivity / epsilon
    noise = np.random.laplace(0, scale, len(data))
    return data + noise

# Esempio
true_average = 50000  # Stipendio medio
private_average = add_laplace_noise([true_average], epsilon=0.1)[0]
# Output: ~50000 ma con rumore per privacy
```

**Parametro ε (epsilon):**
- Piccolo ε = più privacy, meno accuratezza
- Grande ε = meno privacy, più accuratezza

## 🔐 Trusted Execution Environments (TEE)

Hardware sicuro per computazione confidenziale.

**Esempi:**
- Intel SGX
- ARM TrustZone
- AMD SEV

```
┌─────────────────────┐
│   Untrusted OS      │
├─────────────────────┤
│   Secure Enclave    │ ← TEE
│   (encrypted memory)│
└─────────────────────┘
```

## 🎭 Anonymous Credentials

Credenziali che preservano l'anonimato.

**Esempio:** Provare di essere studente universitario senza rivelare quale università.

## 🔗 Collegamenti
- **Precedente:** [Scalabilità DLT](scalabilita-dlt.md)
- **Prossimo:** [Interoperabilità](interoperabilita.md)
