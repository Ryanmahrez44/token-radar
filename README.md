# Token Launch Radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/Ryanmahrez44/token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/Ryanmahrez44/token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A web dashboard ranking 198 DeFi protocols by token-launch likelihood, built from a sealed DefiLlama snapshot. Built for the [make.fun "Build the best token launch radar" bounty](https://make.fun/bounty/token-radar-01).

## How it works

`main.ts` reads the sealed snapshot (`input/snapshot.json`, 758 protocols) and scores each protocol with the `radar-2` model in `lib/model.ts`: `implied = TVL × 0.25 × growth × tenure × chains`. Wrapped, staked and bridged products are excluded. The board ranks 198 protocols — 40 watchlist (TVL ≥ $50M) + 158 early.

## Run

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <kit>/input/snapshot.json
```

## Top 5

1. Steakhouse Financial — $560.5M implied
2. Sentora Curator — $514.8M
3. Gauntlet — $336.4M
4. Veda — $318.1M
5. USDD — $203.1M
