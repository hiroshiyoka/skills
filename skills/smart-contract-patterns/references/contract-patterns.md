# Contract Patterns — Code Reference

## 1. Multi-token whitelist

```solidity
mapping(address => bool) public isWhitelistedToken;

event TokenWhitelisted(address indexed token, bool status);

function setTokenWhitelist(address token, bool status) external onlyOwner {
    isWhitelistedToken[token] = status;
    emit TokenWhitelisted(token, status);
}

modifier onlyWhitelistedToken(address token) {
    require(isWhitelistedToken[token], "Token not whitelisted");
    _;
}
```

Never accept an arbitrary `IERC20(tokenAddress)` passed in a call without checking the whitelist first — this is what prevents a malicious or malformed token contract from being used to attack balance accounting elsewhere in the contract.

## 2. Isolated per-currency balance mappings

```solidity
// user => token => balance
mapping(address => mapping(address => uint256)) public balances;

function deposit(address token, uint256 amount) external onlyWhitelistedToken(token) {
    IERC20(token).transferFrom(msg.sender, address(this), amount);
    balances[msg.sender][token] += amount;
}

function withdraw(address token, uint256 amount) external nonReentrant {
    require(balances[msg.sender][token] >= amount, "Insufficient balance");
    balances[msg.sender][token] -= amount;   // effects before interaction
    IERC20(token).transfer(msg.sender, amount);
}
```

The key property: a bug or exploit affecting one token's accounting cannot leak into another token's balance, because they live in fully separate mapping slots. Never use a single aggregated `uint256` balance across multiple token types.

## 3. Soulbound / non-transferable tokens (ERC-5192)

```solidity
error TransferLocked();

function transferFrom(address, address, uint256) public pure override {
    revert TransferLocked();
}

function approve(address, uint256) public pure override {
    revert TransferLocked();
}

function locked(uint256 /* tokenId */) external pure returns (bool) {
    return true; // ERC-5192 interface requirement
}
```

Block transfer at the contract level explicitly — don't rely on off-chain convention or a UI that simply doesn't expose a transfer button. The guarantee has to be on-chain to mean anything for a reputation/credential use case.

## 4. Escrow release pattern

```solidity
enum EscrowStatus { Pending, Released, Refunded }

struct Escrow {
    address payer;
    address payee;
    address token;
    uint256 amount;
    EscrowStatus status;
}

mapping(uint256 => Escrow) public escrows;

function release(uint256 escrowId) external nonReentrant {
    Escrow storage e = escrows[escrowId];
    require(e.status == EscrowStatus.Pending, "Not pending");
    require(msg.sender == e.payer || msg.sender == owner(), "Not authorized");

    e.status = EscrowStatus.Released;   // effects before interaction
    IERC20(e.token).transfer(e.payee, e.amount);
}
```

Keep release/refund logic as single, clearly-guarded functions — resist the temptation to add multiple partial-release code paths unless the spec genuinely requires partial releases (YAGNI applies here too).

## Test checklist template

For any contract using these patterns, Foundry tests should cover at minimum:

```solidity
function test_RevertWhen_TokenNotWhitelisted() public { /* ... */ }
function test_BalanceIsolation_BetweenTokens() public { /* ... */ }
function test_RevertWhen_TransferAttempted_OnSoulboundToken() public { /* only if applicable */ }
function test_Withdraw_UpdatesBalanceBeforeTransfer() public { /* checks-effects-interactions */ }
```
