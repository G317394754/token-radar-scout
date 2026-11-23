# Token Launch Radar — radar-2

A ranked board of protocols that **record no token yet**, each with an implied value derived from
the snapshot's own figures and a concrete farm plan.

## What makes radar-2 different

The house starter scores every protocol with one global multiple (`base_multiple = 0.25`). That number
is asserted, not measured. In this snapshot the ratio of a **live** protocol's market cap to its TVL
runs from **0.04** (Liquid Staking) to **1.22** (Cross Chain Bridge) by category — a 19x spread. One
global multiple therefore mis-ranks every protocol whose category's tokens trade at a different
multiple from the median.

`radar-2` measures the multiple instead of asserting it:

- For each category with at least 3 live peers, the multiple is the **median market-cap-to-TVL of that
  category's own live peers**, taken from this same snapshot.
- A category with fewer live peers falls back to the global median, and the row says so
  (`multiple_source`).
- On top of it: seven-day growth, maturity (time running with no token) and **reach** (a token that
  reaches more chains reaches more people).

No protocol name is ever special-cased. The same function scores every row.

## The dashboard

A static page with no network: the ranked board, sortable and filterable, a detail view for every
protocol, and a separate view of the protocols that already trade (their market cap is a fact, not a
model). The page **recomputes each row from the figures the board carries** and shows whether it
reproduces the board's number, so the page and the data cannot drift apart.

## Run it

```bash
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input path/to/snapshot.json
```

Node 22.18 or later. Standard library only. The program runs twice in grading and both runs must write
the same bytes.

## Model

```
implied = round( tvl_usd
               x category_multiple
               x (1 + clamp(change_7d, -60, 120) / 100 x 0.5)
               x (0.6 + min(age_days, 1095) / 1095 x 0.4)
               x (1 + (chains - 1) x 0.12) )
```

Built for make.fun's token-radar-01 bounty: https://make.fun/bounty/token-radar-01
