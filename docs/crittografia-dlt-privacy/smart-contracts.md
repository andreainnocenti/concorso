# Smart Contracts

## 📖 Introduzione

Gli **smart contracts** sono programmi auto-eseguibili memorizzati su blockchain che implementano automaticamente accordi tra parti quando si verificano condizioni predefinite.

**Caratteristiche fondamentali:**
- **Deterministici**: Stesso input → stesso output sempre
- **Immutabili**: Una volta deployati, non possono essere modificati
- **Trasparenti**: Codice visibile a tutti
- **Trustless**: Non serve fiducia in terze parti
- **Auto-eseguibili**: Esecuzione automatica quando condizioni soddisfatte

**Analogia:** Come un distributore automatico - inserisci moneta (input), ottieni prodotto (output) automaticamente, senza intermediari.

---

## 🎯 Casi d'Uso

### 1. DeFi (Decentralized Finance)
- **Lending/Borrowing**: Aave, Compound
- **DEX**: Uniswap, SushiSwap
- **Stablecoin**: DAI, USDC

### 2. NFT (Non-Fungible Tokens)
- Arte digitale, collectibles
- Gaming items
- Real estate tokenization

### 3. Supply Chain
- Tracciabilità prodotti
- Certificazioni automatiche
- Pagamenti condizionali

### 4. DAO (Decentralized Autonomous Organization)
- Governance decentralizzata
- Treasury management
- Voting systems

---

## 📝 Solidity (Ethereum)

### Contratto Base

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleStorage {
    // State variable (memorizzata su blockchain)
    uint256 private storedData;
    
    // Event (log su blockchain)
    event DataStored(uint256 indexed data, address indexed setter);
    
    // Function per scrivere
    function set(uint256 x) public {
        storedData = x;
        emit DataStored(x, msg.sender);
    }
    
    // Function per leggere (view = non modifica stato)
    function get() public view returns (uint256) {
        return storedData;
    }
}
```

**Concetti chiave:**
- `uint256`: Intero senza segno 256 bit
- `public`: Accessibile da chiunque
- `view`: Funzione read-only (no gas per chiamate esterne)
- `msg.sender`: Indirizzo di chi chiama la funzione
- `event`: Log permanente su blockchain

### Token ERC-20 (Standard Fungible Token)

```solidity
pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract MyToken is IERC20 {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    
    uint256 private _totalSupply;
    string public name = "MyToken";
    string public symbol = "MTK";
    uint8 public decimals = 18;
    
    constructor(uint256 initialSupply) {
        _totalSupply = initialSupply * 10**decimals;
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }
    
    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }
    
    function transfer(address to, uint256 amount) public override returns (bool) {
        require(_balances[msg.sender] >= amount, "Insufficient balance");
        require(to != address(0), "Transfer to zero address");
        
        _balances[msg.sender] -= amount;
        _balances[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }
    
    function approve(address spender, uint256 amount) public override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }
    
    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }
    
    function transferFrom(address from, address to, uint256 amount) public override returns (bool) {
        require(_balances[from] >= amount, "Insufficient balance");
        require(_allowances[from][msg.sender] >= amount, "Allowance exceeded");
        require(to != address(0), "Transfer to zero address");
        
        _balances[from] -= amount;
        _balances[to] += amount;
        _allowances[from][msg.sender] -= amount;
        
        emit Transfer(from, to, amount);
        return true;
    }
}
```

**Pattern ERC-20:**
- `transfer`: Trasferimento diretto
- `approve + transferFrom`: Delega trasferimento (usato da DEX)
- `mapping`: Hash table on-chain

### NFT ERC-721 (Standard Non-Fungible Token)

```solidity
pragma solidity ^0.8.0;

contract SimpleNFT {
    mapping(uint256 => address) private _owners;
    mapping(address => uint256) private _balances;
    mapping(uint256 => string) private _tokenURIs;
    
    uint256 private _tokenIdCounter;
    
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
    
    function mint(address to, string memory uri) public returns (uint256) {
        uint256 tokenId = _tokenIdCounter++;
        _owners[tokenId] = to;
        _balances[to]++;
        _tokenURIs[tokenId] = uri;
        
        emit Transfer(address(0), to, tokenId);
        return tokenId;
    }
    
    function ownerOf(uint256 tokenId) public view returns (address) {
        return _owners[tokenId];
    }
    
    function balanceOf(address owner) public view returns (uint256) {
        return _balances[owner];
    }
    
    function tokenURI(uint256 tokenId) public view returns (string memory) {
        return _tokenURIs[tokenId];
    }
}
```

---

## ⚡ Gas e Ottimizzazione

### Cos'è il Gas?

**Gas** = unità di misura del costo computazionale su Ethereum.

```
Costo Transazione = Gas Used × Gas Price
```

**Esempio:**
- Gas Used: 50,000
- Gas Price: 50 Gwei (0.00000005 ETH)
- Costo: 50,000 × 50 = 2,500,000 Gwei = 0.0025 ETH

**Operazioni costose:**
- Storage (SSTORE): ~20,000 gas
- Creazione contratto: ~32,000 gas base
- Transfer ETH: 21,000 gas

### Tecniche di Ottimizzazione

**1. Usa `uint256` invece di tipi più piccoli**
```solidity
// ❌ Più costoso (packing/unpacking)
uint8 a;
uint16 b;

// ✅ Più economico
uint256 a;
uint256 b;
```

**2. Cache array length**
```solidity
// ❌ Legge length ad ogni iterazione
for (uint256 i = 0; i < arr.length; i++) {
    // ...
}

// ✅ Cache length
uint256 length = arr.length;
for (uint256 i = 0; i < length; i++) {
    // ...
}
```

**3. Usa `++i` invece di `i++`**
```solidity
// ❌ i++ crea variabile temporanea
for (uint256 i = 0; i < length; i++) { }

// ✅ ++i più efficiente
for (uint256 i = 0; i < length; ++i) { }
```

**4. Usa `calldata` per array read-only**
```solidity
// ❌ Copia in memory
function sum(uint256[] memory arr) public pure returns (uint256) { }

// ✅ Legge direttamente da calldata
function sum(uint256[] calldata arr) public pure returns (uint256) { }
```

**5. Batch operations**
```solidity
// ❌ N transazioni
for (uint i = 0; i < users.length; i++) {
    transfer(users[i], amounts[i]);
}

// ✅ 1 transazione
function batchTransfer(address[] calldata users, uint256[] calldata amounts) public {
    for (uint i = 0; i < users.length; ++i) {
        _transfer(users[i], amounts[i]);
    }
}
```

---

## 🔒 Security Patterns e Vulnerabilità

### 1. Reentrancy Attack

**Vulnerabilità più famosa** (The DAO hack, 2016, $60M rubati).

**Codice vulnerabile:**
```solidity
// ❌ VULNERABILE
function withdraw(uint256 amount) public {
    require(balances[msg.sender] >= amount);
    
    // Invia ETH PRIMA di aggiornare balance
    (bool success, ) = msg.sender.call{value: amount}("");
    require(success);
    
    balances[msg.sender] -= amount;  // Troppo tardi!
}
```

**Attacco:**
```solidity
contract Attacker {
    function attack() public {
        victim.withdraw(1 ether);
    }
    
    // Fallback chiamata quando riceve ETH
    receive() external payable {
        if (address(victim).balance >= 1 ether) {
            victim.withdraw(1 ether);  // Chiama di nuovo!
        }
    }
}
```

**Soluzioni:**

**A) Checks-Effects-Interactions Pattern**
```solidity
// ✅ SICURO
function withdraw(uint256 amount) public {
    // 1. Checks
    require(balances[msg.sender] >= amount);
    
    // 2. Effects (aggiorna stato PRIMA)
    balances[msg.sender] -= amount;
    
    // 3. Interactions (chiamate esterne DOPO)
    (bool success, ) = msg.sender.call{value: amount}("");
    require(success);
}
```

**B) ReentrancyGuard**
```solidity
contract ReentrancyGuard {
    bool private locked;
    
    modifier noReentrant() {
        require(!locked, "No reentrancy");
        locked = true;
        _;
        locked = false;
    }
    
    function withdraw(uint256 amount) public noReentrant {
        require(balances[msg.sender] >= amount);
        balances[msg.sender] -= amount;
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success);
    }
}
```

### 2. Integer Overflow/Underflow

**Problema (Solidity <0.8.0):**
```solidity
// ❌ Overflow
uint8 a = 255;
a = a + 1;  // a = 0 (overflow!)

// ❌ Underflow
uint8 b = 0;
b = b - 1;  // b = 255 (underflow!)
```

**Soluzione:**
- Solidity ≥0.8.0: Controlli automatici (revert su overflow)
- Solidity <0.8.0: Usa SafeMath library

### 3. Access Control

```solidity
contract Ownable {
    address private owner;
    
    constructor() {
        owner = msg.sender;
    }
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }
    
    function sensitiveFunction() public onlyOwner {
        // Solo owner può chiamare
    }
    
    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0));
        owner = newOwner;
    }
}
```

### 4. Front-Running

**Problema:** Attaccante vede transazione in mempool e invia transazione con gas price più alto.

**Mitigazioni:**
- Commit-Reveal scheme
- Batch auctions
- Flashbots (MEV protection)

---

## 🐍 Vyper: Alternativa a Solidity

Linguaggio Python-like più sicuro e semplice.

**Filosofia:**
- No inheritance (no complessità)
- No inline assembly
- No function overloading
- Bounds checking automatico

```python
# @version ^0.3.0

storedData: public(uint256)
owner: public(address)

@external
def __init__():
    self.owner = msg.sender

@external
def set(x: uint256):
    assert msg.sender == self.owner, "Not owner"
    self.storedData = x

@external
@view
def get() -> uint256:
    return self.storedData
```

**Vantaggi:**
- ✅ Più sicuro (meno features = meno bugs)
- ✅ Più leggibile
- ✅ Audit più facili

**Svantaggi:**
- ❌ Meno flessibile
- ❌ Ecosistema più piccolo

---

## 🦀 Smart Contracts su Solana (Rust)

Solana usa **Rust** per performance estreme.

```rust
use solana_program::{
    account_info::{next_account_info, AccountInfo},
    entrypoint,
    entrypoint::ProgramResult,
    msg,
    program_error::ProgramError,
    pubkey::Pubkey,
};

entrypoint!(process_instruction);

fn process_instruction(
    program_id: &Pubkey,
    accounts: &[AccountInfo],
    instruction_data: &[u8],
) -> ProgramResult {
    msg!("Hello Solana!");
    
    let accounts_iter = &mut accounts.iter();
    let account = next_account_info(accounts_iter)?;
    
    // Verifica owner
    if account.owner != program_id {
        return Err(ProgramError::IncorrectProgramId);
    }
    
    // Logic here
    
    Ok(())
}
```

**Differenze vs Ethereum:**
- Account model (non contract storage)
- Parallel execution
- Rent (account devono pagare per storage)
- Molto più veloce (50k+ tx/s)

---

## 🎯 Domande Tipiche per Esame

### 1. Cos'è uno smart contract?

**Risposta:**
Programma auto-eseguibile su blockchain che implementa automaticamente accordi quando si verificano condizioni predefinite.

**Caratteristiche:**
- **Deterministico**: Stesso input → stesso output
- **Immutabile**: Non modificabile dopo deploy
- **Trasparente**: Codice pubblico
- **Trustless**: No intermediari
- **Auto-eseguibile**: Esecuzione automatica

**Esempio:** Contratto di vendita casa - quando buyer invia pagamento, ownership trasferita automaticamente, senza notaio.

### 2. Cos'è il gas in Ethereum?

**Risposta:**
Unità di misura del costo computazionale. Ogni operazione costa gas.

**Formula:**
```
Costo = Gas Used × Gas Price (in Gwei)
```

**Perché serve:**
1. **Previene spam**: Attacchi DOS costano troppo
2. **Incentiva miner**: Pagamento per validazione
3. **Limita computazione**: Previene loop infiniti

**Gas limit:** Massimo gas che sei disposto a pagare. Se superi, transazione fallisce ma paghi comunque gas usato.

### 3. Cos'è un reentrancy attack?

**Risposta:**
Attacco dove contratto malevolo chiama ricorsivamente funzione vittima prima che questa aggiorni il suo stato.

**Esempio The DAO (2016):**
1. Attacker chiama `withdraw(1 ETH)`
2. Contratto invia 1 ETH ad attacker
3. Attacker riceve ETH, fallback chiamata
4. Fallback chiama di nuovo `withdraw(1 ETH)`
5. Balance non ancora aggiornato → preleva di nuovo
6. Ripeti fino a svuotare contratto

**Prevenzione:**
- Checks-Effects-Interactions pattern (aggiorna stato PRIMA di chiamate esterne)
- ReentrancyGuard modifier

### 4. Differenza tra ERC-20 e ERC-721

**Risposta:**

**ERC-20 (Fungible Token):**
- Token intercambiabili (1 token = 1 token)
- Esempio: USDT, LINK, UNI
- Uso: Criptovalute, governance token

**ERC-721 (Non-Fungible Token):**
- Ogni token è unico (tokenId)
- Esempio: CryptoPunks, Bored Apes
- Uso: Arte digitale, collectibles, gaming items

**Differenza chiave:**
- ERC-20: `balanceOf(address)` → uint256
- ERC-721: `ownerOf(tokenId)` → address

### 5. Perché gli smart contract sono immutabili?

**Risposta:**

**Motivi tecnici:**
- Codice memorizzato su blockchain
- Blockchain è immutabile per design
- Nessun meccanismo di "update" nel protocollo

**Vantaggi:**
- ✅ Trustless: Codice non può cambiare dopo deploy
- ✅ Sicurezza: Nessuno può modificare logic
- ✅ Trasparenza: Quello che vedi è quello che ottieni

**Svantaggi:**
- ❌ Bug non correggibili
- ❌ No upgrade per nuove features

**Workaround:**
- Proxy pattern (contratto proxy → implementation)
- Migrazioni (deploy nuovo contratto, migra dati)
- Governance (DAO vota upgrade)

---

## 📊 Confronto Piattaforme Smart Contract

| Piattaforma | Linguaggio | TPS | Gas | Finalità | Ecosistema |
|-------------|-----------|-----|-----|----------|-----------|
| Ethereum | Solidity, Vyper | ~15 | Alto | ~15 min | Enorme |
| Solana | Rust | 50,000+ | Basso | ~0.4 sec | Grande |
| Binance Smart Chain | Solidity | ~100 | Medio | ~3 sec | Grande |
| Polygon | Solidity | ~7,000 | Molto basso | ~2 sec | Grande |
| Avalanche | Solidity | ~4,500 | Basso | ~1 sec | Medio |
| Cardano | Plutus (Haskell) | ~250 | Basso | ~20 sec | Medio |

---

## 🔗 Collegamenti

- **Precedente:** [Architetture DLT](architetture-dlt.md)
- **Successivo:** [Scalabilità DLT](scalabilita-dlt.md)
- **Correlato:** [Interoperabilità](interoperabilita.md)
