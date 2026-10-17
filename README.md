# RimRunnerKentucky

Unofficial Kentucky HS boys basketball ratings. Not affiliated with the KHSAA, the Kentucky All "A" Classic or MaxPreps.
Live site: https://rimrunnerkentucky.pages.dev/ (backup copy: GitHub Pages).

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project `rimrunnerkentucky`.

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 04:30 and 12:00 UTC
(11:30 pm / 7:00 am Eastern standard time) and on manual dispatch. Before the Kentucky boys season starts
(2026-11-30, KHSAA first allowable contest; `SEASON_START` in index.html) the script makes no requests. From then on it reads boys
basketball finals for the last 3 days from the MaxPreps Kentucky scores-by-date page, matches them to the
schedule by MaxPreps team id + date, records them in `data/results.json` and bakes scores and W-L records
into `index.html`. The page recomputes Elo from those finals. It commits only when data changed, then
deploys (index.html + logos/) to Cloudflare Pages if the `CLOUDFLARE_API_TOKEN` repository secret is set.
