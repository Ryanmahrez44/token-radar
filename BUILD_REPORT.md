# Token Radar — Build Report

Entry for make.fun `token-radar-01` — "Build the best token launch radar".
Prize $1,325 (1st $1,000, 2nd–5th $50, 6th–10th $25). Deadline Mon 14 Dec 2026 17:00.
Built 19 Nov 2026 in `~/workspace/makefun-token-radar/repo/` from the kit at
`~/workspace/makefun-token-radar/token-radar-01-kit/`.

## What the model scores on (radar-2, `lib/model.ts`)

One function scores every row, from the snapshot's own figures only — no protocol name is
ever special-cased, no row is hand-picked:

```
implied = round( tvl_usd x 0.25
  x (1 + clamp(change_7d, -50, 100) / 100 x 0.6)
  x (0.55 + min(age_days, 1460) / 1460 x 0.45)
  x (1 + 0.03 x min(chains - 1, 5)) )
```

- **TVL** is the anchor: locked value is the best public proxy for what a token would be worth.
- **Seven-day growth** moves the score +60% / −30%.
- **Tenure** is the time the protocol has run with no token on record (days since DefiLlama first
  listed it; unknown age uses 365 days). A protocol that ran 4 years with no token scores nearly
  twice a brand-new one.
- **Chain count** is a small footprint term (up to +15%).

**Eligibility.** A row must record no token (no market cap, no CoinGecko id, no symbol) AND must
not be a wrapped, staked or bridged product of a token that already trades — the same reading the
house's watchlist uses: category not in {Bridge, Canonical Bridge, Liquid Staking, Liquid
Restaking, Restaked BTC, CEX}, name with no whole word `wrapped`/`staked`/`staking`, no exchange
name prefix, no version marker. 144 of the snapshot's 342 pre-token protocols are excluded this
way (WBTC, Binance staked ETH, USDT0, ...). The board is the whole eligible universe: **198 rows
= 40 watchlist + 158 early**. All 40 watchlist slugs are covered; the 24 largest live-token
protocols are set aside in their own view with their market caps, never mixed into the ranking.

**Evidence.** Every row carries 3 reasons drawn only from the snapshot
(`No token on record`, size, strongest of 7-day growth / tenure / chain footprint), shown under
each row and in the detail view.

**Farm plans.** Every row carries 6 steps with a cost and a time, derived from the protocol's
category, chains, TVL and age (never from a list of names). A farm plan is how a reader works a
protocol before or after it issues a token; the board never claims a token is coming.

## Top 5

| Rank | Protocol | Implied value | Evidence |
| --- | --- | --- | --- |
| 1 | Steakhouse Financial | $560.5M | No token on record · $2.75B TVL · Running 1.5y with no token |
| 2 | Sentora Curator | $514.8M | No token on record · $2.76B TVL · Running 1.1y with no token |
| 3 | Gauntlet | $336.4M | No token on record · $1.58B TVL · TVL +5.6% in 7 days |
| 4 | Veda | $318.1M | No token on record · $1.56B TVL · TVL −8.3% in 7 days |
| 5 | USDD | $203.1M | No token on record · $1.06B TVL · Running 1.7y with no token |

Notes: Sentora Curator (#2) is not on the house watchlist — the early radar found it. USDD is
ranked because the snapshot records no token for it; the dashboard says "no token on record",
never "has no token". All 198 implied values are distinct; watchlist ranks span 1–66.

## Gate checks (all pass)

Checked with `/tmp/check-gate.mjs` against `out/` (re-implements the 7 sealed-scorer rows from
the kit's RULES.txt, plus the dossier text cap):

| Check | Result |
| --- | --- |
| dashboard_files | PASS — 3 files, 35.2 KB, index.html + CSS + JS, no URLs in source |
| dashboard_source_under_64k | PASS — 36,052 bytes (dossier text 36,316 bytes, no cut line: the judge reads the whole source) |
| board_file | PASS — 198 rows, 284 KB |
| sorted_by_value | PASS — ranks 1..198, implied non-increasing, 198 distinct values |
| coverage | PASS — watchlist 40/40 |
| farm_steps | PASS — 6 steps per row, each 99–175 chars (gate: 3–12 steps, 20–600 chars) |
| evidence | PASS — 3 reasons per row |
| screenshots | PASS — 1440×900 PNG, 173 / 224 / 173 KB (gate: ≤512 KiB, long ≤1440, short ≥800) |
| dossier_matches | PASS — 797 KB, byte-identical to `buildDossier` (gate cap 1.5 MiB) |
| data_matches_board | PASS — data.js slugs and implied values match board.json |

Determinism: two runs of `main.ts` produce byte-identical `board.json`, `data.js`,
`dossier.json`, `dashboard/` and `screenshots/` (the scorer runs the program twice).

Run: `mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input
/home/hatch/workspace/makefun-token-radar/token-radar-01-kit/input/snapshot.json`

## Screenshots

- `screenshots/001-board.png` — board view (masthead, top-5 spotlight, controls, table)
- `screenshots/002-detail.png` — detail view, rank #1 Steakhouse Financial (evidence, model
  breakdown with recompute check, farm plan)
- `screenshots/003-filtered.png` — filtered view (category = Risk Curators, 34 rows shown)

Taken with headless Chrome 159 from `file://` URLs at 1440×900. The page works from `file://`
with no network loads (verified: no console errors, full DOM renders).

## How this differs from the starter (house dashboard #2)

The kit's starter page IS the house's second fixed dashboard, so submitting it unchanged would
play as that dashboard and never enter the playoff. This entry changes all three things the
rules name: the model (radar-2: tenure + chain terms, wrapped-product exclusion, whole eligible
universe instead of top-160-by-TVL), the page (dark masthead, top-5 spotlight, watchlist tab,
per-row evidence, rewritten CSS/JS), and the plan (category-aware weekly actions).

## Deviations from the parent brief (kit RULES.txt is authoritative)

The parent brief was written from a web summary of the bounty page; the kit's RULES.txt
contradicts it in three places and the build follows RULES.txt:

1. **Board row schema.** The brief said rows carry `launchScore`, `launchEvidence`,
   `reasonCode`. The gate requires `slug`, `name`, `rank`, `implied_value_usd`, `farm_steps`
   (extra keys allowed). Rows carry `implied_value_usd` + `farm_steps` (required) plus
   `evidence` (3 reasons), `watchlist`, and the model breakdown as extra keys. There is no
   `reasonCode` in the gate or the judge criteria.
2. **Farm plans are REQUIRED, not forbidden.** The gate has a `farm_steps` check (3–12 steps
   per row, 20–600 chars) and judge criterion 3 scores farm plans with concrete costs and
   times. The brief's "farm plans are not allowed" would have failed the gate.
3. **Screenshots are board / detail / filtered** (RULES.txt judge section), so the third is
   `003-filtered.png`, not `003-watchlist.png`. The watchlist is exposed as a tab in the
   dashboard instead. The gate itself only requires 3–4 images.
4. **No `dashboard/leaderboard.json` / `dashboard/detail.json`.** The starter (and RULES.txt)
   write the dashboard's data as `data.js` beside the page; the dashboard loads it and no
   other data. Those two files are not in the kit's rules.
5. **Gate rows are 7**, not 9 (dashboard_files, board_file, sorted_by_value, coverage,
   farm_steps, screenshots, dossier_matches). The 9 checks are the *transcript* gate.

## Open issues / for the parent (repo creation, push, submit)

- **NOT done here:** GitHub repo creation, push, README badges, repo About, transcript bundle,
  submit. The parent handles those.
- **Transcripts:** this competition requires the transcripts of the agent sessions that built
  the entry (transcript gate: 9 checks). This build session is a Muse app session; bundle it
  with `makefun bundle --muse-usage <file>` (Muse app sessions carry no token usage on their
  own). The opening prompt is saved verbatim in `prompts/AGENT_PROMPTS.md`. Bundle flags from
  the GTA 6 entry: `--harness muse-app --harness-version 1.3 --provider meta --model
  muse-spark-1.3 --effort default`. The parent session that copied the starter into the repo
  must be bundled too (it is the top-level session; this build ran as its subagent).
- **Transcript-gate authorship:** `main.ts`, `lib/model.ts` and all three dashboard files were
  rewritten in full by this session (not copied), and revised again in later messages
  (CSS tightening, the `el()` array-child fix), so authored coverage and the ≥2-revision rule
  should hold. `vendor/makefun-board.ts` is the kit's pre-approved vendored library, left
  untouched. `LICENSE` is the kit starter's MIT license.
- **Repo requirements at submit:** README must carry the three make.fun badges
  (`https://make.fun/badge/token-radar-01.svg` plus the repo's own rank/reward badges) with no
  `{owner}` placeholders, and the GitHub About must have a make.fun website + the `makefun`
  topic (see RULES.txt "Enter" step 4). The repo must be public, created after the bounty
  opened (14 Nov 2026), and the entry signed with the agent wallet (as for GTA 6; payout
  defaults to the signing wallet — redirect to Ryan's Phantom SOL at claim time).
- **No live session recording** was used: `makefun record` returned HTTP 400 for this
  competition (entries upload transcripts as a bundle/loose files instead).
