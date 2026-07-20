# Fasal Becho — Feature List

Organized by priority, matching the cut list already agreed in `idea.md`. Build top to bottom; stop wherever time runs out — everything above the line still demos cleanly.

---

## Core (must-have — the demo doesn't work without these)(POSSIBLE)

- **Synthetic data generator** — Faker/Mimesis-based, sector-specific (P2M/P2P split, seasonal bias), fixed seed so demo numbers don't change between runs(pOssible)
- **4-step scenario pipeline** — baseline (UPI velocity) → seasonality overlay → market-price adjustment (Agmarknet) → weather deflator (Meteostat)(Possible)
- **Triple-line chart** — Optimistic / Expected / Pessimistic scenarios(Possible)
- **Working Capital Dip alert** — zero-crossing detection with a plain-language deficit statement
- **Lightweight risk model + SHAP attribution** — a small, transparent model (e.g. linear/logistic regression) trained offline in Python on the synthetic dataset, explained with SHAP feature attributions; this is the actual "AI/ML development" of the project — restores `idea.md`'s original "Keep: SHAP/Why-card explainability" line, which the Core tier had previously simplified away. Trained and exported **offline only** — the running app never calls a live Python/ML service, it applies the exported weights deterministically (see `ARCHITECTURE.md`).
- **"Why" breakdown card** — one-line attribution per flag, generated from the SHAP values above (e.g. "12% downward trend in Mandi prices for Paddy")
- **Field-officer risk queue** — sorted priority list, first screen shown in the demo (not the chart)
- **One sector, fully real** — dairy or agri-input retail (decide this week), end-to-end
- **Offline entry + sync** — enterprise can enter data offline; syncs on reconnect (not a hardened sync engine, just this)
- **Officer override** — flag is advisory; officer can dismiss or act, stated explicitly in the demo narrative

## Important (build if core is solid with time to spare)

- **Enterprise-side view** — income/expense entry, sees its own risk status and suggestions
- **Live UPAg `agmarknet` API call** — at least one real pull, to prove the demo isn't 100% fabricated
- **Enterprise profile drill-down** — officer clicks into one flagged enterprise to see the full chart + Why card
- **Second sector as a config stub** — enough to show the architecture generalizes, not a fully working second model
- **PMFBY claims cross-check** — sanity-check the weather-deflator logic against real historical PMFBY claim spikes (UPAg `pmfby_ay` source); validation only, never a live pipeline input

## Cut for the hackathon (explicitly out of scope, say so if asked)

- Live Account Aggregator / Sahamati sandbox integration — mocked only, schema-shape reference used instead(M)
- Multilingual / vernacular UI(M)
- Prophet + ARIMA "rigor baselines" — one model plus the honest circularity caveat is enough
- Secondary UPAg sources (`enam`, `wpi`, `pmfby_ay`, `ncdex`, `pink_sheet`)(M)
- Monitoring dashboard / sector heat map — not in the demo path(M)
- Portfolio-level analytics beyond the officer's own queue(M)
- Any real lending/underwriting decisioning — Fasal Becho is explicitly a triage layer, not a credit-scoring product(M - Future)

## Pitch-only (not code — goes in the deck/talk track instead)(REWRITE)

- Synthetic-data disclosure, stated upfront
- Rule-based-simulator framing (not "AI forecasting") — the honest answer to the circularity question
- Business model as funding-path hypothesis, not a proven plan
- "We're not competing with Kaleidofin/Avanti/KarmaLife — we're the layer that decides who gets a human's attention first"
- Officer-assisted entry as the answer to the enterprise under-reporting risk
- NPCI credibility line: *"Synthetic UPI proxy calibrated against published NPCI aggregate statistics"* — cite the specific month/figure used
- Academic citation for independent, non-vendor backing: *"Credit Distribution and Refinance Activities of NABARD: A Detailed Review,"* Asian Journal of Economics, Business and Accounting — one line on the "why this matters" slide, not a data source

## Stretch (only if everything above is done early — unlikely in hackathon time)

- Train/test split rigor on top of the Core risk model — hold back some generator parameters so there's a defensible claim it recovers *some* signal it wasn't directly given, beyond just having a trained model at all
- Real Meteostat coverage check + fallback logic for sparse-station pin codes
- Second fully-built sector model