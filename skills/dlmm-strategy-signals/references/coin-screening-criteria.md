# Coin Screening Criteria — Consolidated

Each strategy profile uses its own screening thresholds. Consolidated here so a screening tool can support multiple strategy profiles without duplicating filter logic per bot. Use the column matching whichever strategy profile you're screening for — don't average or mix thresholds across strategies.

## Dexscreener-stage filters (before deeper token analysis)

| Filter | Blitzkrieg | Evil Panda | Combine Bid-Ask & Spot |
|---|---|---|---|
| Market cap minimum | $350,000 | $250,000 | $250,000 |
| 24h volume minimum | $1,000,000 | $1,000,000 | $100,000 |
| Token age | Prefer < 7 days | Sorted by age, no hard cutoff stated | ≥ 6 hours |
| Chart signal required | 15m: uptrend (HH/HL), price closed above SuperTrend | 15m: SuperTrend breakout (Bootcamp variant only) | None specified — screening is fundamentals-only |
| Volume shape check | Reject erratic/zig-zag ("sisir") volume | Not specified | Not specified |
| Requires profile/socials | Yes | Yes | Not specified |

## Risk-checker stage (e.g. RugCheck or equivalent)

| Filter | Blitzkrieg | Evil Panda | Combine Bid-Ask & Spot |
|---|---|---|---|
| Overall risk result | Must return "GOOD" | Not explicitly stated (assume same check applies) | Not explicitly stated |

## On-chain analytics stage (e.g. GMGN or equivalent)

| Filter | Blitzkrieg | Evil Panda (original) | Evil Panda (Bootcamp) | Combine Bid-Ask & Spot |
|---|---|---|---|---|
| Total fees generated | ≥ 30 SOL | — | > 30 SOL | ≥ 15 SOL |
| Top 10 holders | < 20% | — | < 30% | Not specified |
| Bundling | < 40% | — | < 60% | < 30% |
| Insiders | < 4% | — | < 10% | Not specified |
| Phishing | < 15% | — | < 30% | Not specified |
| Single-wallet holding cap | No wallet > 5% (except Pump AMM) | — | Not specified | Not specified |

Note the Bootcamp variant of Evil Panda uses noticeably looser thresholds than Blitzkrieg across every category — reflects a deliberately more permissive filter for a strategy that's betting on a dump anyway, versus Blitzkrieg's stricter filter for a strategy staying in a token through continued upward momentum.

## Pool-level checks (after a token passes the above)

| Filter | Blitzkrieg | Evil Panda | Combine Bid-Ask & Spot |
|---|---|---|---|
| Bin step | ~80–125 | 80 / 100 / 125 | Not fixed — varies by token |
| Pool TVL minimum | ≥ $2,000 | Not specified | Not specified |
| Total pool TVL vs. market cap | ≤ 15% | Not specified | Not specified |

## Implementation note

A screening tool should tag each token check with which strategy profile's thresholds were applied, and store the raw metric values (not just pass/fail) — this is what lets thresholds be revisited later per strategy without re-fetching historical data. Log alongside the six-checkpoint monitoring data from `checkpoint-criteria.md` once a position is actually opened, so screening and monitoring history live in the same place per token.
