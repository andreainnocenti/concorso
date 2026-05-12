# Interoperabilità tra Piattaforme

## 📖 Introduzione

L'interoperabilità permette a blockchain diverse di comunicare e scambiare valore.

## 🌉 Cross-Chain Protocols

### Atomic Swaps

Scambio trustless tra blockchain diverse.

```
Alice (Bitcoin) ↔ Bob (Litecoin)

1. Alice crea HTLC su Bitcoin
2. Bob crea HTLC su Litecoin
3. Alice rivela secret e prende Litecoin
4. Bob usa secret per prendere Bitcoin
```

### Wrapped Tokens

Token rappresentanti asset di altre chain.

```
Bitcoin → Lock → Mint WBTC on Ethereum
```

**Esempi:**
- WBTC (Wrapped Bitcoin)
- renBTC
- WETH (Wrapped Ether)

## 🔗 Bridge Protocols

### Cosmos IBC (Inter-Blockchain Communication)

```
Zone A ←→ IBC ←→ Zone B
    ↓              ↓
  Cosmos Hub (relay)
```

### Polkadot Parachains

```
Parachain 1 ─┐
Parachain 2 ─┼→ Relay Chain
Parachain 3 ─┘
```

## 🔮 Oracles

Portano dati esterni on-chain.

### Chainlink

```
Smart Contract → Chainlink Node → External API
                      ↓
                 Aggregation
                      ↓
                 On-chain Data
```

**Decentralized Oracle Networks:**
- Multiple data sources
- Aggregazione
- Reputation system

## 📡 Standard di Interoperabilità

### ERC-20 (Ethereum)
Standard per token fungibili.

### ERC-721 (NFT)
Standard per token non-fungibili.

### ERC-1155 (Multi-Token)
Standard per token multipli.

## 🔗 Collegamenti
- **Precedente:** [Privacy-Enhancing Technologies](privacy-enhancing.md)
- **Prossimo:** [Crittografia Post-Quantum](post-quantum.md)
