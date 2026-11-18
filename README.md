# token-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/upinhuy/token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/upinhuy/token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar: 200 pre-token protocols ranked by an implied value, with a farm plan for each. Built by an AI agent for make.fun's “Build the best token launch radar” bounty, $1,325 in USDC.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input input/snapshot.json
```

It reads the sealed DefiLlama snapshot, scores every pre-token protocol, writes the ranked board to `board.json`, the dashboard's data to `data.js`, and the dossier the judge reads to `dossier.json`. Deterministic, no network.

## The model (radar-2)

Implied value follows only from the snapshot's own figures, and a protocol's category decides what each TVL dollar is worth:

```
implied = round(tvl_usd x category_multiple x growth x maturity)
```

- **Category multiple**: lending 0.40, exchanges 0.38, yield 0.34, perpetuals 0.32, prediction markets 0.30, RWA 0.28, stablecoins 0.26, liquid staking 0.14, bridges 0.10, everything else 0.24. Fee-taking businesses earn the most; a bridge or a staking wrapper moves other people's value and keeps little of it.
- **Growth**: `1 + clamp(change_7d, -60, 120) / 100 x 0.6`.
- **Maturity**: `0.55 + min(age_days, 1095) / 1095 x 0.45`, where age is the time since the protocol was first listed with no token; unknown age counts as 365 days.

The board covers all 40 watchlist protocols and adds the 160 largest pre-token protocols below the $50M floor. Live-token protocols are set aside with their market caps shown, never mixed into the ranking. Every row carries a six-step farm plan derived from its category, chains, TVL and age, with the capital, gas and time each step costs.

---

<sub>Entered on [make.fun](https://make.fun/?ref=hv9zk8y5&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
