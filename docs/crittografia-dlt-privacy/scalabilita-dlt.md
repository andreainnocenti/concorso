# Scalabilità in DLT

## 📖 Introduzione

La scalabilità è una sfida chiave per le blockchain: aumentare throughput mantenendo decentralizzazione e sicurezza.

## 🔺 Trilemma della Blockchain

Impossibile ottimizzare contemporaneamente:
- **Decentralizzazione**
- **Sicurezza**
- **Scalabilità**

## 🔄 Layer 2 Solutions

### Lightning Network (Bitcoin)

Canali di pagamento off-chain.

```
Alice ←→ Payment Channel ←→ Bob
  ↓                           ↓
Bitcoin Blockchain (settlement)
```

### Rollups (Ethereum)

#### Optimistic Rollups
- Assume transazioni valide
- Periodo di challenge (7 giorni)
- Esempi: Optimism, Arbitrum

#### ZK-Rollups
- Prove crittografiche (zero-knowledge)
- Finalità immediata
- Esempi: zkSync, StarkNet

```
L2: 1000s TPS
  ↓ (batch)
L1: Proof + State Root
```

### State Channels

Transazioni off-chain tra partecipanti.

```
1. Open channel (on-chain)
2. Multiple transactions (off-chain)
3. Close channel (on-chain)
```

## 🔀 Sharding

Divisione della blockchain in shard paralleli.

```
Shard 1: Accounts A-F
Shard 2: Accounts G-M
Shard 3: Accounts N-Z
     ↓
Beacon Chain (coordination)
```

**Ethereum 2.0:** 64 shard chains

## 🌉 Sidechains

Blockchain separate connesse alla mainchain.

**Esempi:**
- Polygon (Ethereum)
- Liquid (Bitcoin)
- xDai

## 🔗 Cross-Chain Bridges

```
Ethereum → Bridge → Binance Smart Chain
  (Lock)            (Mint wrapped token)
```

**Rischi:**
- Centralizzazione
- Vulnerabilità smart contract

## 📊 Confronto Soluzioni

| Soluzione | TPS | Sicurezza | Complessità |
|-----------|-----|-----------|-------------|
| Layer 1 | 10-100 | Alta | Bassa |
| Optimistic Rollup | 1000s | Media | Media |
| ZK-Rollup | 1000s | Alta | Alta |
| Sidechain | 1000s | Variabile | Media |
| State Channel | Illimitato | Alta | Alta |

## 🔗 Collegamenti
- **Precedente:** [Smart Contracts](smart-contracts.md)
- **Prossimo:** [Privacy-Enhancing Technologies](privacy-enhancing.md)
