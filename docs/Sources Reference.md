# Fasal Becho — Data Sources Reference

How to actually use each data source, mapped to the 4-step pipeline: **Baseline → Seasonality → Market Realities → Weather Deflator**. Read top to bottom in priority order — the first three are load-bearing for the demo; the rest are polish/credibility.

---

## 1. UPAg API — `upag.gov.in` (your `openapi.json`) ⭐ PRIMARY market data source(USING)

**What it is:** A real, authenticated government API (Unified Portal for Agricultural Statistics, launched by DA&FW/NITI Aayog) that unifies ~20 agri-data sources behind one endpoint pattern. This replaces the flaky data.gov.in sample-key approach — it's the single most valuable source you have.

**How to access it:**
1. Register for an account at `upag.gov.in` — **do this today**, government API approval can take 1–2 days and you don't want it blocking you mid-build.
2. Authenticate: `POST /v1/upag/api-data-share/login` (form-encoded username/password) → returns a bearer `access_token`.
3. Check what you're approved for: `GET /v1/upag/api-data-share/sources/user-allowed-sources` (send the bearer token).
4. Pull data: `POST /v1/upag/api-data-share/sources/{source_name}` with a JSON body shaped like:
   ```json
   { "source_input_object": { "limit": 10, "offset": 0, "source_name": "agmarknet", "year": ["2025"], "location_granularity": "state" } }
   ```
   (Exact required fields vary per source — check the live `/docs` page on the API for the full schema per source, since some fields in the spec were truncated when you shared it.)

**Which sources to actually pull, and what for:**

| source_name | What it gives you | Use it for |
|---|---|---|
| `agmarknet` | Mandi commodity prices (state/district/market/commodity/variety/grade/price) | **Step 3 (Market Realities)** — this is the core input. Pull your chosen sector's commodity (wheat, milk, poultry feed inputs, etc.), compute month-over-month % change, feed that directly into your revenue-curve adjustment. |
| `enam` | e-NAM trade data | Backup/cross-check for `agmarknet` prices — previously had no public API at all, so having it now is a genuine upgrade. Use if `agmarknet` coverage is thin for your chosen commodity. |
| `pmfby_ay` | Crop insurance claims by state/district/tehsil/village | **Step 4 (Weather Deflator), secondary signal.** Insurance claim spikes are a real-world proxy for crop distress — if you want to validate that your weather-deflator logic produces sensible outputs, cross-check against where PMFBY claims actually spiked historically. |
| `wpi` | Wholesale Price Index | Optional macro inflation feature — a general "cost pressure" input alongside your specific commodity price if you want the model to react to broader input-cost inflation, not just one crop. |
| `mncfc` / `dcs` / `dafw_state` / `dafw_district` | Area/Production/Yield at state/district/village level | Use only if your chosen sector is crop-dependent (e.g. dairy feed costs tied to fodder-crop yield) — otherwise skip, this is more for macro credibility than pipeline input. |
| `fci_procurement` / `fci_stock` | Government wheat/paddy procurement | Only relevant if your one deep sector is grain-trading rural retail — skip otherwise. |
| `ncdex` / `pink_sheet` | Commodity futures / World Bank global prices | Skip for MVP — nice-to-have if you want a "forward-looking price signal" slide, not core to the demo. |
| `farmers_survey` / `nsso` | Household-level agri survey data | Use only when calibrating your **Faker/Mimesis synthetic generator** — these give you realistic distributions (typical household income, savings patterns) to seed believable mock enterprise profiles, not a live pipeline input. |

**Bottom line for your team:** wire up `agmarknet` first — that's your Step 3 dependency and the one live-API call your pre-submission checklist requires. Everything else in this table is optional depth.

---

## 2. Meteostat — weather (Step 4: Weather Risk Deflator) ⭐ PRIMARY weather source(USING)

**What it is:** A weather data API/Python library giving historical and near-real-time data by station or lat/long — much faster to integrate than IMD's raw gridded NetCDF files.

**How to use it:**
```bash
pip install meteostat
```
```python
from meteostat import Point, Daily
from datetime import datetime

location = Point(lat, lon)  # enterprise's pin-code coordinates
data = Daily(location, start_date, end_date).fetch()
# data has columns: tavg, tmin, tmax, prcp (precip mm), wspd, etc.
```

**How to use it in your pipeline:**
- Pull the enterprise's pin-code coordinates (mock these too, or use a small lookup table of real district-centroid lat/longs for your chosen sector's region).
- Compare current/forecast-period rainfall against the historical monthly average for the same period → compute a **delay/deficit score**.
- If the deficit crosses your threshold (e.g. monsoon onset >X days late, or cumulative rainfall <Y% of normal), apply the "stress multiplier": shift the expected revenue hump forward by the delay period and flag increased short-term cash-drain risk — exactly as your teammate's Step 4 describes.

**Caveat to flag in the deck:** cite Meteostat as your engineering choice for speed, but mention **IMD's official gridded rainfall dataset (0.25°×0.25°, 1901–2024, Pai et al. 2014, MAUSAM)** as the citation for "what a production version would use" — this shows you know the authoritative source even though you didn't integrate its raw NetCDF format under hackathon time pressure.

---

## 3. Synthetic Data Generation — Faker / Mimesis (Python) ⭐ FOUNDATION for everything(USING)

**What they are:** Python libraries for generating realistic fake data — names, IDs, timestamps, addresses, financial-looking numbers — with sensible statistical shape out of the box.

```bash
pip install faker
# or
pip install mimesis
```

**How to use them for Fasal Becho specifically — this is the part that needs your own logic layered on top, the libraries alone won't give you rural-enterprise-shaped data:**

1. **Generate the transaction skeleton** with Faker/Mimesis: transaction IDs, timestamps, amounts (use a realistic distribution, not uniform random — e.g. `numpy.random.lognormal` for amounts, since real transaction sizes are right-skewed).
2. **Layer in your P2M/P2P split logic**, adapted from the Kaggle UPI generator concept: P2M *received* = business income, P2M *sent* = business expense, P2P = informal/personal cash movement.
3. **Layer in seasonality (Step 2 of your pipeline):** don't generate transactions uniformly across the year — bias the generator's volume/amount toward your sector's crop/festival calendar (e.g. spike outflow Nov–Dec for an agri-input retailer, spike inflow Apr–May).
4. **Calibrate the macro trend against NPCI's real aggregate stats** (see #4 below) so your synthetic data's growth rate and P2M-vs-P2P ratio aren't arbitrary.
5. **Calibrate distributions against `farmers_survey`/`nsso` data from the UPAg API** if you want defensible-looking household income/savings numbers instead of made-up ranges.

**One team decision to make now:** generate all synthetic data **once**, save it as a fixed dataset (CSV/JSON) checked into your repo, rather than regenerating randomly on every demo run. A judge re-running your demo and getting different numbers than what's in your slides is an easy credibility hit — freeze your seed.

---

## 4. NPCI Product Statistics — calibration, not live pipeline input()

**What it is:** NPCI's own published aggregate UPI stats — monthly transaction volume/value, P2P vs P2M split, app-wise market share.

**How to access it:** `npci.org.in` blocks automated scraping (robots.txt disallows fetch tools) — pull figures manually from their monthly reports, or use a secondary aggregator like **Dataful.in**, which republishes NPCI's UPI volume/value data in an easier downloadable format.

**How to use it:** this is **not** a per-enterprise data source (no such public data exists, by design — that's why the hackathon brief mandates mock data). Use it purely to **calibrate your Faker/Mimesis generator's macro assumptions**: real P2M growth rate (currently outpacing P2P), overall transaction growth trend, so your synthetic "UPI velocity" baseline (Step 1 of the pipeline) rides on a realistic macro curve instead of an arbitrary one.

**Credibility line for the deck:** *"Synthetic UPI proxy calibrated against published NPCI aggregate statistics"* — cite the specific month/figure you used.

---

## 5. Kaggle "UPI Transactions Generator" (skullagos5246) — reference implementation, not a direct data source(POSSIBLE)

**What it is:** A notebook that generates a synthetic UPI dataset with fields: transaction ID, timestamp, transaction type (P2P/P2M/Bill Payment/Recharge), merchant category, amount (category-specific distribution, weekend-boosted), status.

**How to use it:** don't use its output dataset as-is (it's generic consumer transactions, not rural-enterprise-shaped). **Fork its generation *logic*** — the P2P/P2M split, the weekend/temporal amplification pattern — and rebuild it inside your own Faker/Mimesis generator with your sector-specific merchant categories swapped in. Treat it as a reference implementation pattern, not a plug-in dataset.

---

## 6. Sahamati `account-aggregator-standards` GitHub — schema shape reference only(possible)

**What it is:** The official RBI-sanctioned Account Aggregator ecosystem's API specs (`specs/*.yaml`) and FI (Financial Information) Type schemas (`schemas/**/*.xsd`) — the real JSON/XML structure banks use to share account data through the AA network.

**How to use it:**
```bash
git clone https://github.com/Sahamati/account-aggregator-standards.git
```
Open `schemas/deposit/deposit.xsd` — don't parse or validate against it in code, just **read the field names and structure** (balances, transaction entries with date/amount/narration/type, account summary) and shape your mock financial-record JSON to match.

**Why this matters for the grill-me critique:** the plan already commits to explicitly labeling AA integration as **mocked, not live** (real FIU onboarding is weeks of compliance work). Using this schema shape is what makes that honest disclosure land as "we know exactly what the real thing looks like and built a faithful stand-in" rather than "we made up a random format."

---

## 7. Academic citation — NABARD credit-distribution research(possible)

**Source:** "Credit Distribution and Refinance Activities of NABARD: A Detailed Review," *Asian Journal of Economics, Business and Accounting* (`journalajeba.com/index.php/AJEBA/article/view/1879`).

**What it is:** A peer-reviewed descriptive review (2021–2024 data, built from NABARD's own annual reports) finding a positive relationship between agricultural credit distribution and productivity improvement, and that expanded financial-inclusion infrastructure narrowed access gaps.

**How to use it:** this is a **citation, not a data source** — one line on your "why this matters" deck slide, e.g. *"Peer-reviewed research confirms the link between credit access and rural productivity outcomes (Asian Journal of Economics, Business and Accounting, 2025)"* — gives your core thesis independent, non-vendor backing.

---

## Priority order if time runs short

1. **UPAg `agmarknet`** — required for your live-API checklist item; wires Step 3.
2. **Meteostat** — fast, wires Step 4; skip IMD raw grids entirely for the hackathon.
3. **Faker/Mimesis generator with your own P2M/seasonality logic** — this is the foundation everything else calibrates against; get it frozen (fixed seed) early.
4. **NPCI stats + Sahamati schema** — calibration/shape references, do these once your core generator works, not before.
5. **UPAg secondary sources (`pmfby_ay`, `wpi`, `enam`, etc.), Kaggle notebook, journal citation** — polish and deck credibility, cut first if time-constrained.