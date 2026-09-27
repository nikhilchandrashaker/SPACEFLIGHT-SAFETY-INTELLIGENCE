# Spaceflight Safety Intelligence

Historical Shuttle Risk & Decision Analytics — two independent case studies built from small NASA/UCI datasets.

## Contents

| File | What it is |
|---|---|
| `spaceflight-safety-dashboard.html` | Interactive dashboard: landing-control rule matcher + O-ring temperature/risk explorer |
| `spaceflight-safety-report.html` | Written analysis with charts, covering both datasets and the extrapolation problem |
| `README.md` | This file |

Both HTML files are self-contained — no build step, no dependencies. Open either one directly in a browser.

## Source data

- `shuttle-landing-control.data` / `.names` — 15 cases, 6 conditions (stability, error, sign, wind, magnitude, visibility), used by a 1988 NASA autolander team to decide automatic vs. manual control.
- `o-ring-erosion-only.data` / `o-ring-erosion-or-blowby.data` / `.names` — 23 pre-Challenger shuttle flights: launch temperature, leak-check pressure, number of O-rings (of 6) showing thermal distress.

## Case study 1 — Landing control

Rather than fitting a model to 15 rows, the dashboard encodes the 15 historical cases as explicit rules (some attributes wildcarded, matching the original "don't care" values in the dataset). Given a new set of conditions, it returns the **most specific matching rule** and shows which conditions drove the recommendation — an interpretable approach, in keeping with how the original data was used (rule induction, not black-box classification).

## Case study 2 — O-ring thermal distress

A simple least-squares regression of distress count on launch temperature, fit on all 23 flights. The dashboard's temperature slider covers 31–81°F; the report's chart plots the same fit statically.

**Important caveat, repeated in both deliverables:** every observed flight launched at 53°F or warmer. The Challenger forecast of 31°F is 22 degrees below the coldest flight on record. Any estimate at that temperature is an extrapolation, not a measurement — the regression line can produce a number, but the number is not validated by data at that range. This is the same point later academic re-analyses (Dalal, Fowlkes & Hoadley 1989; Lavine 1991; Draper 1993) make: different reasonable extrapolation methods diverge sharply at 31°F, which itself undercuts the assumption that temperature had no effect on failure risk.

## What's deliberately left out

- No decision-tree / logistic-regression / polynomial / robust-regression model comparisons — the current build uses one interpretable method per case study. These could be added if useful.
- The two datasets are **not** combined into one table or model — they don't share entities, variables, or a process, so they're kept as two independent studies.
- No pressure variable in the O-ring model yet — temperature is the primary driver examined here.

## Possible next steps

- Add the additional model comparisons (decision tree, logistic regression, rule learner for landing; polynomial/robust regression for O-rings) as a second layer, with accuracy/interpretability trade-offs called out.
- Add leak-check pressure as a second O-ring input.
- Add flight-order (temporal trend) as an additional O-ring variable.
