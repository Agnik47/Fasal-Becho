# Fasal Becho — Ideation Brief

**One-liner:** A triage layer that helps one NABARD field officer prioritize hundreds of rural micro-enterprises instead of dozens — by simulating cash-flow scenarios from market, weather, and transaction signals, and explaining every flag in plain language.

---

## Problem
Field officers can only maintain deep relationships with a handful of enterprises. Everyone else gets checked on infrequently, so financial stress builds unnoticed until it's a crisis. *(Caseload figure is illustrative, not a cited stat — validate with a real officer/e-Shakti data if possible before the pitch.)*

## What it is (honest framing)
**Not** an AI model that predicts real-world cash flow with proven accuracy — that claim doesn't hold up on synthetic, rule-generated data, and a sharp judge will catch it.
**Is** a **rule-based scenario simulator with an explainability layer**: transparent, auditable logic that turns four signals into a triage priority + a human-readable reason. That's still a genuinely useful tool — it just doesn't overclaim "AI forecasting skill."

## Who it's for
- **Field officer (primary user):** wants a sorted priority queue, not a dashboard to babysit.
- **Enterprise (data source + beneficiary):** has an incentive problem — may under-report if they know a flag affects loan treatment. **Mitigation to design in, not ignore:** officer-assisted entry (not pure self-entry) for the pilot, framed as "this helps us support you," not "this decides your credit."
- **NABARD (buyer, and also the judge — a dependency risk, not a strength):** wants proof before funding. No other institution has expressed interest yet — say so.

## The logic (work backwards from output)
1. **Baseline** — UPI velocity → quiet-day revenue + operating costs
2. **Seasonality** — sector → crop/festival calendar (e.g. agri-input retailer: outflow spike Nov–Dec, inflow spike Apr–May)
3. **Market reality** — Agmarknet mandi price shift → revenue curve adjustment
4. **Weather deflator** — monsoon delay → stress multiplier, shifted revenue timing

## What it outputs
- **Triple-line chart** — Optimistic / Expected / Pessimistic scenarios (not one unfalsifiable number)
- **Working Capital Dip alert** — flags the zero-crossing point, plainly stated
- **"Why" card** — one-line plain-language attribution per flag

## Competition — honest version
Don't lead with a feature-gap table against Kaleidofin/Avanti/KarmaLife — they're lending/underwriting products in different customer segments; the comparison is a category mismatch and reads as reverse-engineered. **The real comparison that matters:** e-Shakti's existing roadmap and any internal NABARD/RRB tooling — unresearched, needs checking before the pitch. **The actual differentiation, stated plainly:** *"We don't originate or price credit — we're the layer that decides which enterprise gets a human's attention first."*

## Business model — stated as hypothesis, not plan
No pricing, no payer, no timeline yet for any of: SaaS to institutions, grant-funded public good, embedded lending fees. Say this openly. Near-term goal is a NABARD pilot conversation, nothing more.

## Scope for the build (cut list, in order)
1. Drop `enam`/`wpi`/`pmfby_ay`/`ncdex` — polish only, no live use
2. Drop Prophet + ARIMA "rigor baselines" — one model + the honest circularity caveat beats rigor theatre
3. Drop the monitoring dashboard / sector heat map — not in the demo path
4. Keep: one sector, fully real; SHAP/Why-card explainability; officer risk queue as first screen; offline entry + sync

## Open questions before building (answer this week, cheap now, expensive later)
- Is `agmarknet` confirmed inside the UPAg account's `user-allowed-sources`? (Not yet checked.)
- Does Meteostat have usable station coverage for the actual target pin codes? (Not yet checked.)
- Which one sector — dairy or agri-input retail? (Not yet decided.)
- Train/test split idea: hold back some generator rule-parameters from the model, so there's *some* defensible claim it recovers signal it wasn't directly given — worth doing even minimally.
