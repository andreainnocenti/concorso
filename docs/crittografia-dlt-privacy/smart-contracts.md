# Smart Contracts

## 📖 Introduzione

Gli smart contracts sono programmi auto-eseguibili su blockchain che implementano accordi tra parti.

## 📝 Solidity (Ethereum)

### Contratto Base

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleStorage {
    uint256 private storedData;
    
    event DataStored(uint256 data);
    
    function set(uint256 x) public {
        storedData = x;
        emit DataStored(x);
    }
    
    function get() public view returns (uint256) {
        return storedData;
    }
}
```

### Token ERC-20

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
    }
    
    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }
    
    function transfer(address to, uint256 amount) public override returns (bool) {
        require(_balances[msg.sender] >= amount, "Insufficient balance");
        _balances[msg.sender] -= amount;
        _balances[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }
    
    // ... altre funzioni
}
```

## ⚡ Gas e Ottimizzazione

```solidity
// ❌ Costoso
function sumArray(uint256[] memory arr) public pure returns (uint256) {
    uint256 sum = 0;
    for (uint256 i = 0; i < arr.length; i++) {
        sum += arr[i];
    }
    return sum;
}

// ✅ Ottimizzato
function sumArrayOptimized(uint256[] memory arr) public pure returns (uint256) {
    uint256 sum = 0;
    uint256 length = arr.length; // Cache length
    for (uint256 i = 0; i < length; ++i) { // ++i invece di i++
        sum += arr[i];
    }
    return sum;
}
```

## 🔒 Security Patterns

### Reentrancy Guard

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

### Checks-Effects-Interactions

```solidity
function withdraw(uint256 amount) public {
    // Checks
    require(balances[msg.sender] >= amount);
    
    // Effects
    balances[msg.sender] -= amount;
    
    // Interactions
    (bool success, ) = msg.sender.call{value: amount}("");
    require(success);
}
```

## 🐍 Vyper

Linguaggio alternativo più sicuro e semplice.

```python
# @version ^0.3.0

storedData: public(uint256)

@external
def set(x: uint256):
    self.storedData = x

@external
@view
def get() -> uint256:
    return self.storedData
```

## 🦀 Rust (Solana, Near)

### Solana Program

```rust
use solana_program::{
    account_info::AccountInfo,
    entrypoint,
    entrypoint::ProgramResult,
    pubkey::Pubkey,
};

entrypoint!(process_instruction);

fn process_instruction(
    program_id: &Pubkey,
    accounts: &[AccountInfo],
    instruction_data: &[u8],
) -> ProgramResult {
    // Logic here
    Ok(())
}
```

## 🔗 Collegamenti
- **Precedente:** [Architetture DLT](architetture-dlt.md)
- **Prossimo:** [Scalabilità DLT](scalabilita-dlt.md)
