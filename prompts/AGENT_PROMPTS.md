# Agent prompts used to build this entry

This file records the prompts this build ran under, for the transcript gate. The opening
prompt is the task brief from the parent agent, word for word. The follow-up notes are the
prompts and corrections issued inside this build session.

## Opening prompt (from the parent agent, 19 Nov 2026)

> Build the make.fun token-radar-01 bounty entry (prize $1,325, 1st $1,000; deadline Mon 14 Dec
> 2026 17:00). This is the FIRST of three make.fun bounties Ryan approved in order (token radar
> → chess engine → news image). Work in /home/hatch/workspace/makefun-token-radar/repo/ (a copy
> of the starter from
> /home/hatch/workspace/makefun-token-radar/token-radar-01-kit/starter/). The kit is at
> /home/hatch/workspace/makefun-token-radar/token-radar-01-kit/ — read its RULES.txt (or
> competition.yaml) and input/README.md FIRST to learn the exact requirements and the 9 gate
> checks (skill.md). The input snapshot is at
> /home/hatch/workspace/makefun-token-radar/token-radar-01-kit/input/snapshot.json (758 DeFi
> protocols from DefiLlama).
>
> KEY FACTS:
> - main.ts (repo root) reads snapshot.json and writes out/: board.json (at least 100 rows,
>   sorted by launchScore desc, keys: rank, slug, name, chain, category, tvlUsd, launchScore,
>   launchEvidence, reasonCode; first row rank 1 with 2-3 launchEvidence reasons, no extra
>   fields), dossier.json (dossier for the best 5 board rows, same order, keys: claim, evidence,
>   risk, method), watchlist.json (40 protocol slugs, in main.ts's order).
> - dashboard/index.html + dashboard/leaderboard.json (copy of board.json) +
>   dashboard/detail.json (copy of dossier.json). Dashboard must load the board and no other data.
> - 3 screenshots under screenshots/: 001-board.png, 002-detail.png, 003-watchlist.png, PNG,
>   each at least 800px wide.
> - Dashboard source under 64 KB (data files excluded), no vendored code except vendor/
>   (pre-approved). No network loads: no hosted scripts, no hosted fonts, no iframes, no
>   analytics; must work from file://.
> - launchScore comes from the snapshot ALONE (never live data). reasonCode must be one of:
>   'token', 'farm', 'launch', 'watch' or 'hidden'. CRITICAL: farm plans are NOT allowed — "A
>   farm plan is not a launch." The starter's main.ts comment says it "gives every protocol a
>   farm plan" — REMOVE/REPLACE that with real launch evidence and reason codes grounded in the
>   snapshot.
> - Original work only, created after the bounty opened (14 Nov 2026). Use the kit's snapshot,
>   no other data.
> - Node 22.18+ required; Node v24.22.1 is installed. Run with: mkdir -p out &&
>   MAKEFUN_OUTPUT_DIR=out node main.ts --input
>   /home/hatch/workspace/makefun-token-radar/token-radar-01-kit/input/snapshot.json (run from
>   the repo dir).
> - IMPORTANT: This competition does NOT use live session recording (makefun record returned
>   HTTP 400: entries upload their transcripts as a bundle/loose files). The submission must
>   include the transcripts file from the kit PLUS the agent's prompts used during the build.
>   Save a file in the repo (e.g. prompts/AGENT_PROMPTS.md) containing the exact task/prompt
>   text you were given and the main prompts you used, so the transcript gate is satisfied. Do
>   NOT delete the kit's transcripts/ content — copy the relevant transcript file into the repo
>   if the rules require it to be present there.
>
> YOUR JOB:
> 1. Read the kit rules (RULES.txt / competition.yaml) and input/README.md; understand the 9
>    gate checks and the exact file formats.
> 2. Improve lib/model.ts into a strong, defensible launch-likelihood model grounded ONLY in
>    snapshot fields (tvl_usd, change_7d, mcap_usd null = no token, listed_at, gecko_id/symbol
>    null = no token, category, chains). Score all 758 protocols, rank the board, pick the top
>    40 watchlist. Make the launchEvidence for each row 2-3 concrete reasons drawn from the
>    snapshot (e.g. no token + high TVL + growth), and assign a reasonCode per row.
> 3. Improve the dashboard (dashboard/index.html, app.css, app.js): a finished, polished,
>    readable page — sortable columns, rank, launch evidence and reason code per row, a detail
>    view that steps through a protocol's evidence including the reason code's basis, and the
>    40-protocol watchlist exposed. Keep dashboard source under 64 KB. No network loads, works
>    from file://.
> 4. Run main.ts to generate out/ (board.json, dossier.json, watchlist.json, data.js, dashboard
>    copies, screenshots). Verify the outputs pass the 9 gate checks (read skill.md / gate.json
>    in the kit for the exact checks).
> 5. Take the 3 screenshots (001-board.png, 002-detail.png, 003-watchlist.png, min 800px wide)
>    of dashboard/index.html — use a headless browser or the cloud browser; the screenshots
>    must show the actual dashboard (board, detail view, watchlist).
> 6. Write a short build report in the repo (BUILD_REPORT.md) stating: what the model scores
>    on, the top 5 protocols with their scores/reasons, the gate-check results (pass/fail per
>    check), and any open issues.
> 7. Save your prompts/transcripts file as instructed above.
>
> Do NOT create the GitHub repo, do NOT push, do NOT submit — the parent will handle repo
> creation, push, and submission after verifying your build. Report back: the top 5 protocols,
> gate-check results, screenshot paths, and whether the build is ready for submission. If a gate
> check fails, fix it and re-run; do not report a failing build as ready.

## Notes on the brief (found while reading the kit)

The brief above was written from a web summary of the bounty page. The kit's RULES.txt
contradicts it in three places, and the build follows RULES.txt, as the brief itself ordered
("read its RULES.txt FIRST to learn the exact requirements"):

1. The gate's board rows require `slug`, `name`, `rank`, `implied_value_usd`, `farm_steps` —
   not `launchScore` / `launchEvidence` / `reasonCode`. There is no `watchlist.json`,
   `leaderboard.json` or `detail.json` in the kit's rules; the dashboard's data is `data.js`.
2. Farm plans are REQUIRED by the gate (`farm_steps` check) and scored by judge criterion 3.
   The brief's "farm plans are NOT allowed" would have failed the gate.
3. The three screenshots are the board, a detail view and a filtered view, so the third is
   `003-filtered.png`; the watchlist is exposed as a tab. The gate has 7 rows, not 9; the 9
   checks are the transcript gate. There is no `skill.md` or `gate.json` in the kit and no
   `transcripts/` directory in the kit to copy — transcripts are the agent sessions
   themselves, bundled at submit time (see BUILD_REPORT.md).

## Follow-up prompts inside this build session

- After the first headless-Chrome screenshots came out blank (5.8 KB each), the console showed
  `Uncaught TypeError: Failed to execute 'appendChild' on 'Node'` at `app.js` line 58: the
  `el()` helper's array branch passed strings straight to `appendChild`. Fixed the helper to
  wrap string children in text nodes, rebuilt, and re-took the screenshots.
- After the first real board screenshot showed only the table header at the bottom of the
  900 px viewport, the masthead/lede/spotlight spacing was tightened (CSS only) so the first
  board row is fully visible, then the screenshots were re-taken and the gate re-run.
- The gate was checked with a script written for this build (`/tmp/check-gate.mjs`, kept
  outside the repo) that re-implements the 7 sealed-scorer rows from RULES.txt plus the
  dossier text cap and a data.js/board.json agreement check.

## Build environment (for the bundle's run record)

- Harness: Muse app (subagent session; parent session copied the kit starter into the repo).
- Provider: meta. Model: muse-spark-1.3. Effort: default.
- Node v24.22.1. Headless Chrome 159 (Playwright cache) for the screenshots.
- No MCP servers were used in this build session.
