# DengueScope
Scroll-interactive dengue analysis using real Karnataka public reports. No live incidence feed is claimed.

## Run
Serve the dist folder via any static web host. For local Python: `python -m http.server 8000 --directory dist`, then open http://localhost:8000. Opening index.html directly as a file may block JSON loading. No build or API key is required. Google Fonts are optional; fallback fonts are supplied.

## What changed
Animated point-cloud opening, scroll-controlled BBMP history, dark interactive explorer, real district/reporting-area data, keyboard-accessible chart points, coverage-aware heatmap, source refresh, CSV import/export, print view, and separate synthetic scenarios. Reduced-motion support is included. The decorative point cloud is not a map or case visualization.

## Data and attribution
ARTPARK publicdata, Karnataka Dengue Daily Summary Data:
https://publicdata.artpark.ai/en/latest/datasets/0015.html
Publisher CSV:
https://raw.githubusercontent.com/dsih-artpark/publicdata/refs/heads/main/data/0015/ka-dengue-daily-summary.csv
Upstream: Karnataka Department of Health and Family Welfare. Publisher warns of discrepancies.
Bundled snapshot retrieved 2026-11-03. Report range: 2017-01-02 to 2024-08-28. 76,114 raw rows, 32 reporting areas.

The measure is daily.positive.total: reported positive tests, not necessarily unique cases. Sum valid daily reports into calendar months. Normalize region capitalization; keep BBMP separate from the Bengaluru Urban district series. Do not aggregate these into an official state total or claim non-overlapping catchments. Historical boundaries may change.

143 duplicate rows are deduplicated. 48 conflicting region/day keys are excluded. A month is complete only when every calendar day has a valid report. Completeness of report dates does not prove completeness of case ascertainment. Incomplete monthly totals are shown with diamonds; missing is never zero. No population denominators were supplied, so per-capita view is unavailable for the real snapshot.

## Source refresh
The button fetches the publisher CSV directly from the browser with a 45-second timeout, validates and aggregates it, then replaces the in-memory data. Failure retains the previous snapshot. Success reports the actual latest reporting date. In verification on 2026-11-03 the fetch succeeded but the latest date remained 2024-08-28. No claim of a real-time dengue feed. Refreshes do not modify the bundled deployment; reload restores that snapshot.

Official national context is a separate static reference, verified 2026-11-03:
https://ncvbdc.mohfw.gov.in/index4.php?Background=Light&lang=1&level=0&lid=3715&linkid=431
The table lists 6,927 India cases and 560 Karnataka cases provisionally through 28 February 2026. Its webpage update date differs from its data cutoff. Annual/YTD counts are never converted into invented monthly values.

## Analysis
Baseline: same-month arithmetic mean across earlier complete years. Flag only if the current month is complete, at least 3 earlier complete observations exist, and cases exceed BOTH multiplier × baseline and baseline + 20. No future observations enter baseline. A 3-month trailing average needs 3 consecutive complete months. Historical peak withheld unless all 12 months have at least 3 previous complete observations. Rules are exploratory, not calibrated clinical thresholds or official alerts. Historic trend, reporting artifacts and testing changes may affect signals.

## Import format
CSV columns: date,region,cases,days,population. Dates YYYY-MM, cases non-negative integer or blank, days valid reporting days in the month, population optional positive denominator. Missing days means unknown completeness and no signal eligibility. Duplicate region/month keys and invalid numeric fields are rejected. 2 MB maximum. Files processed locally in browser memory, no server upload. Export retains days and population.

## Presentation
1. Start with real reporting dates, scroll BBMP through 2019, 2022 and 2024.
2. Show partial-month diamonds and why they are not assessed as anomalies.
3. Check source: explain no newer reporting date is different from a failed request.
4. Switch to Sample scenarios. September 2025 has 890 versus baseline 387; raise multiplier to 3× and the flag disappears.
5. Mysuru demonstrates a missing month; Kodagu demonstrates insufficient history. Sample counts and populations are fictional and never merged into real data.

## Validation
`node verify.cjs` runs analytics, normalization and CSV edge-case checks. Browser checks cover source refresh, synthetic-mode switching, threshold change, and rendering. No clinical accuracy or predictive performance claim.
