---
name: smart-contract-patterns
description: Apply proven Solidity contract patterns (multi-token whitelisting, isolated per-currency balance mappings, soulbound/non-transferable tokens, escrow with stablecoin support) instead of designing contract structure from scratch each time. Use this whenever the user is building a new Solidity/EVM smart contract on Base, adding token support to an existing contract, or designing on-chain data structures for payments, tipping, splitting, or reputation systems. Also use it to review an existing contract's structure against these patterns before deployment. Requires Foundry as the development framework (see `references/foundry-conventions.md`).
---

# Smart Contract Patterns

A skill for applying contract structure patterns already proven across multiple Base/EVM projects, instead of re-deriving token handling, balance isolation, or access control from scratch on every new contract.

## Why this exists

Payment-adjacent contracts (tipping, splitting, escrow) share the same handful of hard problems — which tokens to accept, how to keep balances from different currencies from bleeding into each other, how to prevent reentrancy on withdrawals. Solving these fresh each time risks re-introducing bugs that were already fixed in a prior project. This skill encodes the patterns that already work.

## Core patterns

See `references/contract-patterns.md` for full code-level detail. Summary:

1. **Multi-token whitelist** — accept a defined set of ERC-20 tokens (e.g. IDRX, USDC, USDT, PYUSD) via an explicit allowlist mapping, never an open "any ERC-20" acceptance. New tokens are added by an admin function, not inferred at call time.
2. **Isolated per-currency balance mappings** — track balances per user *per token*, never a single pooled balance across currencies. Use a nested mapping (`mapping(address => mapping(address => uint256))` — user → token → amount) so currencies can never cross-contaminate.
3. **Soulbound / non-transferable tokens** — for reputation or credential-style tokens, use ERC-5192 and explicitly override/block `transferFrom` and `approve` at the contract level, don't just discourage transfer via convention.
4. **Escrow with stablecoin support** — hold funds in contract until a release condition is met, using the same whitelist + isolated-balance patterns above; release logic should be a single, clearly-guarded function, not scattered across multiple entry points.

## Standard project setup

- **Framework:** Foundry (`forge`, not Hardhat/Truffle) — see `references/foundry-conventions.md` for test/deploy script conventions.
- **Network:** Base (mainnet for production, Base Sepolia for testing).
- **Frontend integration:** wagmi v2 + viem, with Privy for wallet auth (see the `pick-stack` skill for the full frontend defaults).

## Security checklist (apply before considering a contract done)

- Reentrancy guards on any function that transfers funds out (`nonReentrant` or checks-effects-interactions ordering).
- Explicit access control (`onlyOwner`/role-based) on any admin function (adding tokens to whitelist, pausing, upgrading).
- Integer operations use Solidity ^0.8.x's built-in overflow checks — don't add unchecked blocks without a specific, stated reason.
- Every external call that moves value is the *last* thing a function does (checks-effects-interactions), not followed by further state changes.
- Test coverage includes at least one test per pattern above: a whitelist rejection, a balance-isolation test (token A withdrawal doesn't touch token B balance), and a transfer-block test for soulbound tokens if applicable.

## When NOT to reach for these patterns

- Single-token contracts with no multi-currency requirement don't need the whitelist/isolated-mapping pattern — a single balance mapping is simpler and correct (KISS/YAGNI — see the `local-first-architecture` and `principles-check` skills for the same reasoning applied elsewhere).
- Don't add soulbound restrictions to a token that's supposed to be tradeable — confirm the actual requirement before applying ERC-5192.
