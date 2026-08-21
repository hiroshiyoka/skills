# Checkpoint Criteria — Fill In From Your Own Strategy Notes

The values below are **placeholders**, not researched thresholds. Replace each `<...>` with the actual number/rule from your own strategy work (Evil Panda, Blitzkrieg, or whatever the current live approach is), and keep this file as the single source of truth every monitoring tool reads from — don't let individual bots hardcode their own copies that can drift out of sync with each other.

## 1. Price position relative to bin range

| Signal | Threshold (fill in) | Severity |
|---|---|---|
| Price within central bins | — | Info (healthy) |
| Price within `<N>` bins of range edge | `<N>` | Warning |
| Price outside the active range entirely | — | Action |

## 2. Bin distribution / concentration

| Signal | Threshold (fill in) | Severity |
|---|---|---|
| Distribution shape matches original setup | — | Info |
| Concentration has drifted by more than `<X>%` from original | `<X>%` | Warning |

## 3. Volume / fee accrual rate

| Signal | Threshold (fill in) | Severity |
|---|---|---|
| Fee accrual rate within `<Y>%` of the rate at position open | `<Y>%` | Info |
| Fee accrual dropped more than `<Z>%` over the last `<time window>` | `<Z>%`, `<time window>` | Warning |
| Fee accrual near zero for `<time window>` | `<time window>` | Action |

## 4. Impermanent loss exposure

| Signal | Threshold (fill in) | Severity |
|---|---|---|
| Price move from entry within `<A>%` | `<A>%` | Info |
| Price move exceeds `<B>%`, IL starting to outweigh fees earned so far | `<B>%` | Warning |

## 5. Time-in-range vs. time-out-of-range

| Signal | Threshold (fill in) | Severity |
|---|---|---|
| Time-in-range over monitoring window ≥ `<C>%` | `<C>%` | Info |
| Time-in-range drops below `<D>%` | `<D>%` | Warning |

## 6. External volatility signal

| Signal | Threshold (fill in) | Severity |
|---|---|---|
| Underlying pair volatility (e.g. `<metric>`) within normal range | `<metric>` | Info |
| Volatility spike exceeds `<E>` over `<time window>` | `<E>`, `<time window>` | Warning/Action depending on magnitude |

---

## Revision log

Keep a short log here of when thresholds were changed and why (e.g. "tightened bin-edge warning from 3 bins to 2 after missing an early rebalance signal on <date>") — this is what turns this file into a living strategy document instead of a one-time guess.

| Date | Change | Reason |
|---|---|---|
| | | |
