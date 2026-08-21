# Strategy Profiles

Three named, concrete strategies with distinct entry/exit logic — not interchangeable. Pick one deliberately per trade based on market conditions, don't blend their exit rules together. Each profile below is a paraphrased summary of the source strategy notes; refer back to your own original notes for full nuance before automating any of this.

**Not financial advice.** These are risk-management frameworks documented from your own research, not a recommendation to use them. Every strategy below carries real capital risk, including scenarios where the position never activates or a token rugs to zero.

---

## 1. Blitzkrieg (momentum scalping)

High-frequency, tiny-edge scalping on tokens already in a confirmed uptrend. Built as an evolution of Evil Panda's dump-fee concept, but applied to the opposite market condition (uptrend, not anticipated dump).

**Coin screening:** market cap ≥ $350k, 24h volume ≥ $1M, prefer age < 7 days, reject erratic/zig-zag volume, 15m chart must show an uptrend with price closed above a SuperTrend indicator. Risk-checker result must come back clean. Additional wallet-concentration and fee-history checks apply (see `coin-screening-criteria.md`).

**Position setup:**
- Pool: bin step ~80–125, pool TVL ≥ $2k, total pool TVL ≤ 15% of market cap.
- Chart overlays: SuperTrend + Bollinger Bands (or MA20).
- Auto-fill off.
- Position A: 50% of intended capital, **Spot** shape, min price -90% (bin 100) or -80% (bin 80), max price 0%.
- Position B: remaining capital, **Bid-Ask** shape, added on top of Position A.

**Exit — any one of:**
- Position goes out of range (price kept pumping past it).
- Take profit: +0.3% to +0.5%.
- Stop loss: -10%.

**Risk profile:** very small per-trade edge, relies on high win rate and volume of trades rather than large individual wins. Needs frequent, active monitoring — not suited to a "set and forget" schedule. A single missed stop-loss check can erase many small wins.

---

## 2. Evil Panda (contrarian dump-fee strategy)

Anticipates a coin nearing its local top and positions to earn fees specifically *from the dump*, rather than trying to avoid it. Two documented variants exist (original post vs. "Bootcamp #7"); the Bootcamp version adds a SuperTrend entry confirmation and psychology/risk-management rules on top of the original.

**Coin screening:** market cap ≥ $250k, 24h volume ≥ $1M, sorted by age, must have a profile. Fee/holder-concentration filters apply (see `coin-screening-criteria.md`) — the Bootcamp variant uses looser bundling/insider thresholds than the original post.

**Position setup:**
- One-sided SOL position, opened near a perceived local top (not necessarily the all-time high).
- Bootcamp variant: wait for a 15m SuperTrend breakout confirmation before entering. Original post: no confirmation required, just judgment of proximity to top.
- Shape: Spot or Bid-Ask (Bootcamp variant treats these as interchangeable; original post doesn't specify).
- Bin step 80/100/125, range roughly -86% to -95% (wide range functions as the "insurance" zone that only activates once the dump actually reaches it).

**Exit — confluence of at least 2 of the following, on the 5m or 15m chart:**
- RSI(2) closes above 90, **and**
- Price closes above the Bollinger Band upper line, **or**
- MACD histogram prints its first green bar.

**Core mechanic:** the position only earns fees once price actually drops into the wide range — panic sellers pay the pool's fee (5–10%) to exit through it. If the anticipated dump never happens, the position earns nothing. If the token rugs straight to zero past the range, IL exposure remains despite fees collected.

**Risk profile:** contrarian and timing-dependent — explicitly described by the original author as advanced/"final exam" level, not beginner-friendly. The Bootcamp variant adds explicit psychological discipline rules (position sizing across ≥6 concurrent positions, no new positions after a set cutoff time, no size increase without a clean prior day, mandatory cooldown after a loss) — treat these as part of the strategy, not optional extras.

---

## 3. Combine Bid-Ask & Spot (cyclical rebalancing)

A single position that's repeatedly rebalanced as the token appreciates, mixing Spot (fee-focused) and Bid-Ask (IL-reducing) liquidity within the same position rather than opening new positions.

**Coin screening:** market cap ≥ $250k, age ≥ 6 hours, 24h volume ≥ $100k, total fees generated ≥ 15 SOL, bundler concentration < 30%. Notably looser volume/age requirements than Blitzkrieg or Evil Panda — this strategy targets earlier-stage, lower-volume tokens than the other two.

**Position setup and rebalance cycle:**
1. Open with 100% Spot, single-sided SOL, wide range (commonly ~-70%, though the exact bin/percentage varies by token).
2. Once the SOL side has converted roughly 30% into the token, withdraw *only* the SOL portion (~98–100% of the SOL side) — leave the already-converted token side untouched.
3. Re-add the withdrawn SOL as **Bid-Ask** (commonly ~60% of the withdrawn amount).
4. As the token continues appreciating and SOL converts again, repeat: withdraw the SOL portion, re-add as Bid-Ask or Spot depending on which side needs rebalancing.
5. This all happens **within the same position** — no new position is created during the cycle.

**Rationale:** the Bid-Ask portion reduces impermanent loss if the token reverses, while still generating strong fees on the way; the Spot portion maximizes fee capture during strong upward volume. Cycling between them lets profit come from both fee accrual *and* the token's price appreciation, rather than fees alone.

**Risk profile:** moderate — requires active attention tied to price-based thresholds (the ~30%-converted trigger) rather than a fixed schedule. Best suited to tokens in a sustained, gradual uptrend rather than sharp, fast pumps or dumps.

---

## Choosing between the three

| Market condition | Fitting strategy |
|---|---|
| Confirmed uptrend, want frequent small wins | Blitzkrieg |
| Token near a local top, expecting a dump | Evil Panda |
| Steady, gradual appreciation over time | Combine Bid-Ask & Spot |

Don't apply Evil Panda's wide dump-insurance range logic to a Blitzkrieg-style momentum entry, or vice versa — the position shape, range, and exit signal are tuned to opposite market expectations.
