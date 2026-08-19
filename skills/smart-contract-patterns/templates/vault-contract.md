# Vault Contract Template

A ready-to-copy scaffold combining the whitelist + isolated-balance patterns from `references/contract-patterns.md`. Copy this into `src/`, rename the contract, and adapt the deposit/withdraw logic to the project's actual requirements (this is a starting skeleton, not a drop-in final contract — still needs project-specific review before deployment).

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

/// @title Vault
/// @notice Scaffold combining multi-token whitelist + isolated per-currency balances.
/// @dev Rename and extend per project. See references/contract-patterns.md for pattern rationale.
contract Vault is Ownable, ReentrancyGuard {
    // user => token => balance
    mapping(address => mapping(address => uint256)) public balances;
    mapping(address => bool) public isWhitelistedToken;

    event TokenWhitelisted(address indexed token, bool status);
    event Deposited(address indexed user, address indexed token, uint256 amount);
    event Withdrawn(address indexed user, address indexed token, uint256 amount);

    modifier onlyWhitelistedToken(address token) {
        require(isWhitelistedToken[token], "Token not whitelisted");
        _;
    }

    constructor(address initialOwner) Ownable(initialOwner) {}

    function setTokenWhitelist(address token, bool status) external onlyOwner {
        isWhitelistedToken[token] = status;
        emit TokenWhitelisted(token, status);
    }

    function deposit(address token, uint256 amount) external onlyWhitelistedToken(token) {
        require(amount > 0, "Amount must be > 0");
        IERC20(token).transferFrom(msg.sender, address(this), amount);
        balances[msg.sender][token] += amount;
        emit Deposited(msg.sender, token, amount);
    }

    function withdraw(address token, uint256 amount) external nonReentrant {
        require(balances[msg.sender][token] >= amount, "Insufficient balance");
        balances[msg.sender][token] -= amount; // effects before interaction
        IERC20(token).transfer(msg.sender, amount);
        emit Withdrawn(msg.sender, token, amount);
    }
}
```

## What to customize per project

- **Deposit/withdraw triggers.** This scaffold is a plain vault — for a tipping (Drip), splitting (Owe), or escrow (Proven-style) contract, the deposit/withdraw functions need project-specific logic layered on top (e.g. splitting a deposit across multiple recipients, or gating withdrawal behind an escrow release condition — see the escrow pattern in `references/contract-patterns.md`).
- **Access control.** `Ownable` is the simplest option; swap for role-based access control (`AccessControl`) if multiple admin roles are needed.
- **Events.** Add project-specific events beyond the generic ones here (e.g. a `Tipped` event with sender/recipient/message for Drip-style use cases).

## Matching test file

Pair this with a Foundry test file covering the checklist in `references/foundry-conventions.md` — at minimum a whitelist-rejection test and a balance-isolation test between two different tokens.
