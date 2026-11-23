# token-radar-scout

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/G317394754/token-radar-scout/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/G317394754/token-radar-scout/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar that **measures** its multiple instead of asserting it.

Every protocol that records no token yet is scored by one function of the snapshot's own figures, ranked by
implied value, and given a farm plan with costs and times. The board covers the whole watchlist and adds
160 pre-token protocols below the floor, the early ones the radar exists to find.

## Why this is not the house's radar-1

The starter scores every protocol with one global multiple (`base_multiple = 0.25`). That number is asserted,
not measured. In this snapshot the ratio of a **live** protocol's market cap to its TVL runs from **0.04**
(Liquid Staking) to **1.22** (Cross Chain Bridge) by category — a 19x spread. One global multiple therefore
mis-ranks every category whose tokens trade at a different multiple from the median.

`radar-2` measures it instead:

- For each category with at least 3 live peers, the multiple is the **median market-cap-to-TVL of that
  category's own live peers**, taken from this same snapshot.
- A category with fewer live peers falls back to the global median, and the row says so.
- On top: seven-day growth, maturity (time running with no token), and **reach** — a token on more chains
  reaches more people, a factor the starter never uses.

No protocol name is ever special-cased. The same function scores every row.

## The dashboard

A static page with no network: the ranked board, sortable and filterable, a detail view for every protocol,
and a separate view of the protocols that already trade — their market cap is a fact, not a model, so the
board never mixes the two. The page **recomputes each row from the figures the board carries** and says
whether it reproduces the board's number, and it renders the board's farm plans rather than re-deriving
them, so the page and the data cannot drift apart.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input path/to/snapshot.json
```

Node 22.18 or later, standard library only, nothing installed before it runs. The program runs twice in
grading and both runs must write the same bytes.

## Model

```
implied = round( tvl_usd
               x category_multiple
               x (1 + clamp(change_7d, -60, 120) / 100 x 0.5)
               x (0.6 + min(age_days, 1095) / 1095 x 0.4)
               x (1 + (chains - 1) x 0.12) )
```

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
