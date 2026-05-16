# Architetture DLT e Algoritmi di Consenso

## 📖 Introduzione

Le **Distributed Ledger Technologies (DLT)** sono sistemi di registro distribuito che permettono a più partecipanti di mantenere una copia sincronizzata di dati senza un'autorità centrale. La blockchain è il tipo più noto di DLT.

**Caratteristiche fondamentali:**
- **Decentralizzazione**: Nessun punto di controllo centrale
- **Immutabilità**: I dati scritti non possono essere modificati
- **Trasparenza**: Tutti i partecipanti vedono le stesse transazioni
- **Consenso**: Accordo distribuito sullo stato del sistema

---

## ⛓️ Blockchain: Struttura e Funzionamento

### Anatomia di un Blocco

Un blocco contiene:

```
Block #1234
├── Header
│   ├── Previous Block Hash: 0x7a3f2e...
│   ├── Timestamp: 2024-01-15 10:30:00
│   ├── Nonce: 4567890 (per PoW)
│   ├── Merkle Root: 0x9b4c1d...
│   └── Difficulty: 20
└── Body
    ├── Transaction 1
    ├── Transaction 2
    ├── ...
    └── Transaction N
```

**Componenti chiave:**

1. **Previous Block Hash**: Collega il blocco al precedente, creando la "catena"
2. **Merkle Root**: Hash di tutte le transazioni (permette verifica efficiente)
3. **Nonce**: Numero usato nel mining (Proof of Work)
4. **Timestamp**: Quando il blocco è stato creato

### Merkle Tree

Struttura per verificare efficientemente l'inclusione di transazioni:

```
        Root Hash
       /          \
    H(AB)        H(CD)
    /   \        /   \
  H(A) H(B)  H(C)  H(D)
   |    |     |     |
   A    B     C     D
```

**Vantaggio**: Per verificare che A è nel blocco, servono solo log(n) hash invece di n.

### Immutabilità

Modificare un blocco passato richiede:
1. Ricalcolare hash del blocco modificato
2. Ricalcolare hash di TUTTI i blocchi successivi
3. Convincere la maggioranza della rete ad accettare la nuova catena

**Costo computazionale**: Proibitivo per blockchain lunghe e sicure.

---

## 🔨 Proof of Work (PoW)

### Concetto

I miner competono per trovare un **nonce** tale che l'hash del blocco sia minore di un target:

```
SHA-256(block_header + nonce) < target
```

**Esempio:**
```
Target: 0000000000000000000f1a2b3c4d5e6f...
Hash valido: 00000000000000000001234567890abc... ✅
Hash invalido: 00000000000000000012345678901234... ❌ (troppo grande)
```

### Difficoltà (Difficulty)

Regola quanti zeri iniziali deve avere l'hash. Si aggiusta automaticamente per mantenere tempo di blocco costante.

**Bitcoin:**
- Target: ~10 minuti per blocco
- Difficoltà si aggiusta ogni 2016 blocchi (~2 settimane)

**Formula semplificata:**
```
Nuova Difficoltà = Vecchia Difficoltà × (2 settimane / Tempo Effettivo)
```

### Mining Process

```python
def mine_block(block_header, target):
    nonce = 0
    while True:
        hash_result = sha256(block_header + str(nonce))
        if hash_result < target:
            return nonce  # Trovato!
        nonce += 1
```

**Caratteristiche:**
- ✅ Sicurezza elevata (attacco richiede 51% hash power)
- ✅ Semplice da verificare (un solo hash)
- ❌ Enorme consumo energetico
- ❌ Lento (Bitcoin: 7 tx/sec)

**Esempi:** Bitcoin, Ethereum (fino a 2022), Litecoin

---

## 💰 Proof of Stake (PoS)

### Concetto

I validatori sono scelti in base alla quantità di criptovaluta che "mettono in stake" (bloccano come garanzia).

**Processo:**
1. Validatori depositano stake (es. 32 ETH per Ethereum)
2. Algoritmo seleziona validatore per proporre blocco (probabilità ∝ stake)
3. Altri validatori attestano la validità
4. Validatore riceve reward
5. Se validatore è disonesto, perde parte dello stake (slashing)

### Vantaggi vs PoW

| Aspetto | PoW | PoS |
|---------|-----|-----|
| Consumo energetico | Altissimo | Minimo (~99.95% in meno) |
| Hardware richiesto | ASIC costosi | Computer normale |
| Velocità | Lenta (7-15 tx/s) | Veloce (1000+ tx/s) |
| Finalità | Probabilistica | Deterministica |
| Attacco 51% | Richiede 51% hash power | Richiede 51% stake |

### Varianti PoS

**1. Delegated Proof of Stake (DPoS):**
- Token holder votano per delegati
- Delegati validano blocchi
- Più veloce ma meno decentralizzato
- **Esempi:** EOS, Tron

**2. Nominated Proof of Stake (NPoS):**
- Nominatori scelgono validatori
- Validatori condividono reward con nominatori
- **Esempio:** Polkadot

**3. Liquid Proof of Stake (LPoS):**
- Stake può essere delegato senza perdere controllo
- **Esempio:** Tezos

### Ethereum 2.0 (The Merge)

Transizione da PoW a PoS completata nel 2022:

**Beacon Chain:**
- Coordina validatori
- Gestisce stake e reward
- Finalizza blocchi

**Casper FFG (Finality Gadget):**
- Fornisce finalità economica
- Checkpoint ogni 32 slot (~6.4 minuti)
- Blocchi finalizzati non possono essere revertiti

---

## 🛡️ Byzantine Fault Tolerance (BFT)

### Problema dei Generali Bizantini

Scenario: Generali bizantini devono coordinarsi per attaccare, ma alcuni potrebbero essere traditori.

**Obiettivo:** Raggiungere consenso anche con nodi malevoli.

**Teorema:** Serve almeno 3f+1 nodi per tollerare f nodi bizantini (falliti o malevoli).

### PBFT (Practical Byzantine Fault Tolerance)

Algoritmo classico per sistemi permissioned.

**Fasi:**
1. **Pre-prepare**: Leader propone blocco
2. **Prepare**: Nodi validano e broadcast
3. **Commit**: Se 2f+1 prepare ricevuti, commit
4. **Reply**: Esegui transazione

**Caratteristiche:**
- ✅ Finalità immediata
- ✅ Veloce (1000+ tx/s)
- ❌ Richiede nodi conosciuti (permissioned)
- ❌ Scala male (O(n²) messaggi)

**Esempio:** Hyperledger Fabric

### Tendermint

BFT moderno per blockchain pubbliche.

**Processo:**
1. **Propose**: Validatore propone blocco
2. **Prevote**: Validatori votano
3. **Precommit**: Se >2/3 prevote, precommit
4. **Commit**: Se >2/3 precommit, commit blocco

**Caratteristiche:**
- ✅ Finalità immediata (no fork)
- ✅ Veloce (~1000 tx/s)
- ✅ Funziona con validatori dinamici
- ❌ Richiede >2/3 validatori onesti

**Esempi:** Cosmos, Binance Chain

---

## 🔄 Altri Meccanismi di Consenso

### Proof of Authority (PoA)

Validatori pre-approvati basati su reputazione/identità.

**Caratteristiche:**
- ✅ Velocissimo
- ✅ Efficiente energeticamente
- ❌ Centralizzato
- **Uso:** Reti private, testnet

**Esempi:** VeChain, xDai

### Proof of Space (PoSpace)

Usa spazio disco invece di computazione.

**Processo:**
1. Genera "plot" (dati random su disco)
2. Cerca soluzioni nei plot
3. Chi trova soluzione più veloce vince

**Caratteristiche:**
- ✅ Meno energivoro di PoW
- ❌ Spreco di storage
- **Esempio:** Chia

### Proof of History (PoH)

Crea timestamp crittografico verificabile (Solana).

**Idea:** Sequenza di hash che prova il passaggio del tempo.

```
Hash₀ = SHA-256(data)
Hash₁ = SHA-256(Hash₀)
Hash₂ = SHA-256(Hash₁)
...
```

**Vantaggio:** Ordina eventi senza comunicazione tra nodi → velocità estrema (50k+ tx/s)

---

## 📊 Confronto Architetture DLT

### Blockchain vs DAG

**Blockchain:**
- Blocchi in sequenza lineare
- Un blocco alla volta
- Esempi: Bitcoin, Ethereum

**DAG (Directed Acyclic Graph):**
- Transazioni si riferiscono a transazioni precedenti
- Parallelismo
- Esempi: IOTA (Tangle), Nano

**Confronto:**

| Aspetto | Blockchain | DAG |
|---------|-----------|-----|
| Struttura | Lineare | Grafo |
| Throughput | Limitato | Alto |
| Latenza | Alta | Bassa |
| Maturità | Alta | Media |

### Permissioned vs Permissionless

**Permissionless (pubbliche):**
- Chiunque può partecipare
- Consenso trustless (PoW, PoS)
- Esempi: Bitcoin, Ethereum
- **Uso:** Criptovalute, DeFi

**Permissioned (private):**
- Partecipanti autorizzati
- Consenso BFT
- Esempi: Hyperledger, Corda
- **Uso:** Supply chain, finanza enterprise

---

## 🎯 Domande Tipiche per Esame

### 1. Come funziona la Proof of Work?

**Risposta:**
PoW richiede ai miner di trovare un nonce tale che l'hash del blocco sia sotto un target:

```
SHA-256(block_header + nonce) < target
```

**Processo:**
1. Miner raccoglie transazioni
2. Prova nonce diversi (brute force)
3. Primo che trova nonce valido broadcast blocco
4. Altri nodi verificano (un solo hash) e accettano

**Sicurezza:** Attaccare richiede >50% hash power globale (costo proibitivo).

**Problema:** Consumo energetico enorme (Bitcoin ~150 TWh/anno, come Argentina).

### 2. Differenza tra PoW e PoS

**Risposta:**

**PoW (Proof of Work):**
- Sicurezza basata su computazione
- Miner competono risolvendo puzzle
- Consumo energetico altissimo
- Finalità probabilistica (fork possibili)

**PoS (Proof of Stake):**
- Sicurezza basata su stake economico
- Validatori scelti in base a stake
- Efficiente energeticamente (~99.95% in meno)
- Finalità deterministica (no fork dopo checkpoint)

**Attacco:**
- PoW: Serve 51% hash power (hardware)
- PoS: Serve 51% stake (token), ma perdi stake se attacchi (disincentivo economico)

### 3. Cos'è il problema dei Generali Bizantini?

**Risposta:**
Problema di consenso distribuito con nodi potenzialmente malevoli.

**Scenario:** Generali devono decidere se attaccare, ma alcuni potrebbero essere traditori che inviano messaggi contraddittori.

**Soluzione:** Algoritmi BFT (Byzantine Fault Tolerant) che garantiscono consenso se <1/3 nodi sono bizantini.

**Teorema:** Servono almeno 3f+1 nodi per tollerare f nodi falliti/malevoli.

**Applicazione blockchain:** Garantire che tutti i nodi onesti concordino sullo stesso stato anche con nodi malevoli.

### 4. Perché Ethereum è passato da PoW a PoS?

**Risposta:**

**Motivazioni:**
1. **Sostenibilità**: Riduzione 99.95% consumo energetico
2. **Scalabilità**: PoS permette sharding (parallelizzazione)
3. **Sicurezza**: Attacco più costoso (devi comprare 51% ETH, non solo hardware)
4. **Decentralizzazione**: Non serve hardware specializzato

**The Merge (2022):**
- Beacon Chain (PoS) si fonde con mainnet (PoW)
- 32 ETH per diventare validatore
- Reward ~4-5% annuo
- Slashing per comportamenti disonesti

### 5. Cosa garantisce l'immutabilità della blockchain?

**Risposta:**

**Meccanismi:**

1. **Hash linking**: Ogni blocco contiene hash del precedente
   - Modificare blocco N invalida tutti i blocchi N+1, N+2, ...

2. **Proof of Work/Stake**: Ricalcolare catena richiede:
   - PoW: Rifare tutto il mining (costo computazionale)
   - PoS: Controllare >2/3 stake (costo economico)

3. **Consenso distribuito**: Maggioranza nodi deve accettare modifica
   - Attacco 51% necessario ma costoso

**Esempio pratico:**
- Bitcoin: Modificare blocco di 6 conferme fa (1 ora) richiederebbe ~$500M di hardware + elettricità
- Ethereum PoS: Richiederebbe comprare >51% di tutti gli ETH (~$100B+)

---

## 📊 Tabella Riassuntiva Consenso

| Algoritmo | Tipo | Velocità | Energia | Finalità | Decentralizzazione | Esempi |
|-----------|------|----------|---------|----------|-------------------|---------|
| PoW | Permissionless | Bassa (7-15 tx/s) | Altissima | Probabilistica | Alta | Bitcoin, Litecoin |
| PoS | Permissionless | Media (1000+ tx/s) | Bassissima | Deterministica | Alta | Ethereum 2.0, Cardano |
| DPoS | Permissionless | Alta (3000+ tx/s) | Bassa | Deterministica | Media | EOS, Tron |
| PBFT | Permissioned | Alta (1000+ tx/s) | Bassa | Immediata | Bassa | Hyperledger Fabric |
| Tendermint | Permissionless | Alta (1000+ tx/s) | Bassa | Immediata | Media | Cosmos, Binance Chain |
| PoA | Permissioned | Altissima (5000+ tx/s) | Bassissima | Immediata | Molto bassa | VeChain, xDai |

---

## 🔗 Collegamenti

- **Precedente:** [Cifratura e Firma Digitale](cifratura-firma-digitale.md)
- **Successivo:** [Smart Contracts](smart-contracts.md)
- **Correlato:** [Scalabilità DLT](scalabilita-dlt.md)
