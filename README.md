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

1. **Run-up, shallow pullback, bounce.** A curve qualifies when its price ran up ≥10%
   from the window start, pulled back SHALLOWLY (6–9% off its window high —
   deep pullbacks are dead coins; capping the pullback at 9% is what turns the
   edge positive on the dev window), then bounced ≥5% off the low since the
   high, with net buy flow in the newest trades and the last trade a buy.
2. **Organic gate.** At least 3 distinct buyers in the window and no single buyer
   taking more than 60% of the buy SOL, so a bundled launch being dumped cannot
   qualify. Real SOL reserve must sit between 1 and 80 SOL.
3. **Size and exits.** 0.50 SOL per position, at most 5 open positions, at most 2
   entries per mint, never two buys in flight on the same mint. Sell whole on
   the first of: +12% take-profit, −4% stop-loss, or a max hold of 2000 slots.
   No fade / big-sell exits (on the dev window those fired on nearly every
   position and sold every dip into its own bounce) and no trailing stop — the
   bounce is a scalp, take the +12% and leave. Position value is computed by
   constant-product sell proceeds, not spot price, so exits price the sell's
   own impact.
4. **Breadth guard.** A pace guard opens tiny 0.02 SOL probe round trips when the
   coin count lags the slot pace, so the ≥20-coin / ≥0.01 SOL round-trip
   guardrail is met even on a quiet window — at most one probe per 2000 slots,
   only on a mint never traded or probed before, only while under 20 coins.

Local dev grading (kit level 40, `--driver process`): **+0.413 SOL net PnL**
(+4.1% on 10 SOL), 71 round trips over 65 coins, win rate 25%, max drawdown
2.6%, still +0.053 SOL at 2x fees. Guardrails met. Two executions reproduce
byte for byte (deterministic).

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
