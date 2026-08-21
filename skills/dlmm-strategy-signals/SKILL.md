---
name: dlmm-strategy-signals
description: Apply a consistent set of checkpoint criteria and rebalancing rules when building or reviewing Meteora DLMM position monitoring tools, alert bots, or rebalancing logic — instead of re-deriving liquidity-management thresholds from scratch each time. Use this whenever the user is building a DLMM monitoring/alert system, reviewing an existing position's health, or deciding whether to rebalance, exit, or hold a DLMM liquidity position. This skill is a decision framework — the exact numeric thresholds are placeholders the user fills in from their own research (e.g. Evil Panda, Blitzkrieg-style strategies), not universal financial advice.
---

# DLMM Strategy Signals

A skill for applying a consistent, repeatable set of checkpoints when monitoring or rebalancing Meteora DLMM (Dynamic Liquidity Market Maker) positions — so a new bot or dashboard doesn't need the rebalancing logic re-derived from scratch, and so criteria stay consistent across every tool built on top of it.

## Important framing

This skill encodes a **decision framework**, not investment advice. The actual numeric thresholds (bin-drift %, volume drop-off %, time windows) are placeholders in `references/checkpoint-criteria.md` — fill them in from your own strategy notes and update them as the strategy evolves. Don't treat the placeholder numbers as fixed truth; they exist so every tool references the *same* source of truth instead of each one hardcoding its own copy.

## Named strategy profiles

Three concrete, named strategies are documented in `references/strategy-profiles.md`: **Blitzkrieg** (momentum scalping on confirmed uptrends), **Evil Panda** (contrarian dump-fee collection near a local top), and **Combine Bid-Ask & Spot** (cyclical rebalancing through a single position as a token appreciates). These are not interchangeable — each has its own coin-selection filters, position shape/range, and exit signal logic. Pick one deliberately based on the market condition being targeted; don't blend their exit rules together.

Coin-selection thresholds differ per strategy and are consolidated for comparison in `references/coin-screening-criteria.md` — use the column matching whichever strategy profile is active, don't average thresholds across strategies.

## How this relates to the six-checkpoint monitoring model

The strategy profiles above cover **pre-entry screening** and **exit signal logic** (often indicator-based: SuperTrend, RSI(2), Bollinger Bands, MACD). The six-checkpoint model below covers **ongoing position health monitoring** after entry — useful for positions held longer than a single scalp cycle, or for a dashboard tracking multiple open positions across different strategies at once. When a strategy profile's exit signal fires, that should map directly to an "Action" severity in the checkpoint model (see Alert Severity Tiers below) rather than running as a separate, disconnected check.

## The six-checkpoint model

Every position check walks the same six checkpoints, in order, so results are comparable across positions and across time:

1. **Price position relative to bin range** — is the current price still inside the active liquidity range, or has it drifted toward/past an edge bin?
2. **Bin distribution / concentration** — how is liquidity actually distributed across bins right now (concentrated vs. spread), and has that shape drifted from the original setup?
3. **Volume/fee accrual rate** — is the position still earning fees at a rate consistent with when it was opened, or has volume through this pool dropped off?
4. **Impermanent loss exposure** — how far has the pool's price moved from the position's entry price, and what does that imply about IL versus fees earned so far?
5. **Time-in-range vs. time-out-of-range** — over the monitoring window, what fraction of the time has price actually been inside the earning range?
6. **External volatility signal** — is there a broader market volatility spike (e.g. a sharp move in the underlying pair) that suggests a rebalance is about to be needed reactively rather than proactively?

See `references/checkpoint-criteria.md` for the concrete threshold table to fill in and reference from every monitoring tool.

## Alert severity tiers

Map checkpoint results to a consistent severity so a Telegram bot (or any other alert channel) doesn't need its own ad-hoc severity logic:

- **Info** — one checkpoint drifting mildly, no action needed yet, logged for trend visibility.
- **Warning** — price nearing a bin-range edge, or time-out-of-range crossing a set threshold — worth a human look soon.
- **Action** — multiple checkpoints triggered simultaneously, or a single checkpoint has crossed a hard threshold (e.g. price fully out of range) — rebalance or exit decision needed now.

## Rebalance vs. exit decision

When checkpoints indicate action is needed, don't default to "always rebalance" — check:

- **Rebalance** when the pool's underlying volume/fee generation is still healthy and the price move looks likely to stay in a new range for a reasonable duration (avoid rebalancing into a range that will immediately be crossed again — churn from over-frequent rebalancing erodes returns via gas/slippage).
- **Exit** when volume has genuinely dried up, or volatility suggests the pair is entering a regime the position wasn't designed for.

## Building a monitoring tool on this framework

1. Implement the six checkpoints as independent, testable functions — each should take a position snapshot and return a status, not directly send an alert (separation of concerns; see the `principles-check` skill's SOLID guidance).
2. Implement each active strategy profile's exit signal (from `references/strategy-profiles.md`) as its own testable function, separate from the six checkpoints — a strategy exit signal (e.g. RSI(2) + Bollinger Band confluence) is a different kind of check than position-health monitoring, and keeping them separate makes each easier to test and to swap independently.
4. Aggregate checkpoint results *and* any fired strategy exit signal into the severity tiers above before deciding whether/what to alert, then hand off to whatever alert/delivery channel the tool uses (Telegram, dashboard, email) — keep that delivery layer separate from the checkpoint/signal logic itself.
5. Log every checkpoint evaluation and every screening decision (not just alerts) somewhere queryable — this is what lets strategy thresholds in `checkpoint-criteria.md` and `coin-screening-criteria.md` actually get refined over time from real data, instead of staying static guesses.

## When NOT to apply this framework

- One-off manual position checks, or a single position being watched casually, don't need the full six-checkpoint automation — that's for when there are multiple positions or the checking needs to happen unattended.
- Don't apply Meteora-specific bin logic to a different AMM/DEX design without re-deriving which checkpoints actually translate — bin-based concentrated liquidity mechanics don't generalize automatically to constant-product pools.
