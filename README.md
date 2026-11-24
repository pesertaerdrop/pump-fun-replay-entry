# First-Pullback Organic Dip-Rider — Pump.fun Replay Bot

[![make.fun](https://make.fun/badge/u-tidy-owl-94-9499-winner-takes-all-5-pump-fun-b82c82d3.svg)](https://make.fun/bounty/u-tidy-owl-94-9499-winner-takes-all-5-pump-fun-b82c82d3)
[![make.fun score](https://make.fun/badge/u-tidy-owl-94-9499-winner-takes-all-5-pump-fun-b82c82d3/score.svg)](https://make.fun/bounty/u-tidy-owl-94-9499-winner-takes-all-5-pump-fun-b82c82d3)
[![make.fun rank](https://make.fun/badge/u-tidy-owl-94-9499-winner-takes-all-5-pump-fun-b82c82d3/rank.svg)](https://make.fun/bounty/u-tidy-owl-94-9499-winner-takes-all-5-pump-fun-b82c82d3)

Entry for [Winner Takes All — $5 Pump.fun Replay Sprint](https://make.fun/bounty/u-tidy-owl-94-9499-winner-takes-all-5-pump-fun-b82c82d3) on make.fun.
The bot trades 10 SOL of paper money on a 6-hour replay of pump.fun bonding-curve
events; best net PnL wins the whole pot.

SPDX-License-Identifier: MIT

## Strategy

A first-pullback dip-rider with an organic-flow filter — the pattern the dev
leaderboard's whole top 4 shares (buy a dip only once it bounces, never the raw
launch):

1. **Run-up, pullback, bounce.** A curve qualifies when its price ran up ≥20%
   from the window start, pulled back ≥6% off its window high, then bounced
   ≥2% off the pullback low, with net buy flow in the newest trades.
2. **Organic gate.** At least 4 distinct buyers in the window and no single buyer
   taking more than half of the buy SOL, so a bundled launch being dumped cannot
   qualify. Real SOL reserve must sit between 1 and 80 SOL.
3. **Small stakes, quick exits.** 0.10 SOL per position, at most 5 open positions,
   at most 2 entries per mint. Sell whole on the first of: +30% take-profit,
   −15% stop-loss, a trailing stop once up 12%, a max hold, or a fade (net sell
   flow / one big sell).
4. **Breadth guard.** A pace guard opens tiny 0.02 SOL probe round trips when the
   coin count lags the slot pace, so the ≥20-coin / ≥0.01 SOL round-trip
   guardrail is met even on a quiet window.

Deterministic by construction: no clock, no environment, no `Math.random`;
iteration only over insertion-ordered maps fed by the batch stream, so two runs
produce byte-identical action logs.

## Layout

- `main.ts` — the whole strategy; the grader runs `node /bot/main.ts` (Node 22).
- `vendor/makefun-bot.ts` — the kit's SDK shim, byte-identical to
  `sdk/ts/makefun-bot.ts` in the competition kit (kept out of authorship counts).

## Local grading

With the competition kit unpacked:

```sh
node cli/makefun.mjs grade-local --kit <kit-dir> --bot <this-repo> --driver process
```
