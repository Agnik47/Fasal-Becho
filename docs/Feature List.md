# Fasal Becho — Feature List

Organized by priority, matching the cut list already agreed in `idea.md`. Build top to bottom; stop wherever time runs out — everything above the line still demos cleanly.

---

## Core (must-have — the demo doesn't work without these)(POSSIBLE)

- **Synthetic data generator** — Faker/Mimesis-based, sector-specific (P2M/P2P split, seasonal bias), fixed seed so demo numbers don't change between runs(pOssible)
- **4-step scenario pipeline** — baseline (UPI velocity) → seasonality overlay → market-price adjustment (Agmarknet) → weather deflator (Meteostat)(Possible)
- **Triple-line chart** — Optimistic / Expected / Pessimistic scenarios(Possible)
- **Working Capital Dip alert** — zero-crossing detection with a plain-language deficit statement
- **"Why" breakdown card** — one-line attribution per flag (e.g. "12% downward trend in Mandi prices for Paddy")
- **Field-officer risk queue** — sorted priority list, first screen shown in the demo (not the chart)
- **One sector, fully real** — dairy or agri-input retail (decide this week), end-to-end
- **Offline entry + sync** — enterprise can enter data offline; syncs on reconnect (not a hardened sync engine, just this)
- **Officer override** — flag is advisory; officer can dismiss or act, stated explicitly in the demo narrative

## Important (build if core is solid with time to spare)

- **Enterprise-side view** — income/expense entry, sees its own risk status and suggestions
- **Live UPAg `agmarknet` API call** — at least one real pull, to prove the demo isn't 100% fabricated
- **Enterprise profile drill-down** — officer clicks into one flagged enterprise to see the full chart + Why card
- **Second sector as a config stub** — enough to show the architecture generalizes, not a fully working second model

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

## Stretch (only if everything above is done early — unlikely in hackathon time)

- Train/test split holding back some generator parameters, so the model can show it recovers *some* signal it wasn't directly given
- Real Meteostat coverage check + fallback logic for sparse-station pin codes
- Second fully-built sector model