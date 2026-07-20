# Product Requirements Document (PRD)

# Fasal Becho
### A Rule-Based Cash-Flow Scenario Simulator with an Explainability Layer

> Version: 1.1
> Status: MVP
> Project: NABARD Hackathon 2026
> Honest-framing source: `idea.md` — read it before this document.

---

# 1. Overview

Fasal Becho is a decision-support platform designed to help one NABARD field officer triage hundreds of rural micro-enterprises instead of dozens — identifying which ones may experience future cash-flow stress before it becomes a crisis.

The core prediction is **deterministic and rule-based**, not an AI model claiming proven predictive accuracy on real-world cash flow — that claim does not hold up on synthetic, rule-generated data. AI is used strictly for the **explainability layer** on top: turning the deterministic output into plain-language reasons and recommendations. See Section 11 (AI Responsibilities).

Instead of replacing human judgement or making lending decisions, the platform provides explainable cash-flow scenarios, risk prioritization, and actionable recommendations using multiple data signals.

The application is designed as a responsive Progressive Web App (PWA) that works in both online and low-connectivity rural environments.

---

# 2. Problem Statement

Millions of rural micro-enterprises operate without reliable financial visibility.

Most institutions react only after financial distress has already occurred because:

- Financial monitoring is manual
- Cash-flow trends are difficult to identify
- Market conditions change rapidly
- Weather affects business performance
- Officers manage hundreds of enterprises simultaneously

As a result:

- Financial stress is detected too late
- Field visits are reactive
- Working capital shortages increase
- Credit risk increases

Field officers need a way to know **which enterprise requires attention first.**

---

# 3. Product Vision

Fasal Becho transforms scattered financial and environmental signals into a simple, explainable priority system.

Instead of asking:

> "Which enterprise has already failed?"

the platform answers:

> "Which enterprise is most likely to require intervention soon?"

---

# 4. Product Positioning

Fasal Becho is NOT:

- A lending platform
- A loan management system
- A credit approval engine
- A credit scoring product
- A banking application
- An AI model claiming proven predictive accuracy on real-world cash flow

Fasal Becho IS:

- A decision-support platform
- A risk prioritization system
- A rule-based cash-flow scenario simulator with an explainability layer
- A human-in-the-loop intelligence layer

Every prediction is advisory.

Human officers always make the final decision.

---

# 4a. Honest Constraints (do not overclaim these in the demo or deck)

- **Synthetic data:** all financial/transaction data is synthetically generated (Faker/Mimesis, fixed seed), disclosed upfront — not real enterprise data.
- **Business model:** SaaS-to-institutions, grant funding, and embedded-lending fees are all unvalidated hypotheses, not a proven plan. No pricing, payer, or timeline exists yet. Near-term goal is a NABARD pilot conversation, nothing more.
- **Competition:** do not lead with a feature-gap table against lending/underwriting products (Kaleidofin, Avanti, KarmaLife) — different customer segment, reads as a reverse-engineered comparison. The differentiation that matters: *"We don't originate or price credit — we're the layer that decides which enterprise gets a human's attention first."* The real comparison (e-Shakti's roadmap, internal NABARD/RRB tooling) is unresearched and should be checked before the pitch.
- **Enterprise under-reporting risk:** an enterprise has an incentive to under-report if it believes a flag affects loan treatment. Mitigation: officer-assisted data entry for the pilot (not pure self-entry), framed as "this helps us support you," not "this decides your credit."

---

# 5. Primary Users

## 1. NABARD Field Officer (Primary User)

Responsibilities:

- Monitor enterprises
- Review financial health
- Prioritize visits
- Record interventions
- Understand risk factors

Success for this user means:

> "I immediately know which businesses require my attention."

---

## 2. Rural Micro Enterprise

Responsibilities:

- Maintain business profile
- Record financial information
- View business insights
- Understand financial risks
- Receive recommendations

The enterprise does NOT make lending decisions.

---

# 6. Product Goals

The application should enable users to:

- Predict cash-flow trends over the next 3–6 months
- Detect potential working capital shortages
- Explain why risk exists
- Prioritize enterprises requiring intervention
- Assist officers with actionable insights
- Work in low-network environments

---

# 7. Core Workflow

Enterprise Data

↓

Scenario Pipeline

↓

Cash Flow Prediction

↓

Risk Detection

↓

Explainability

↓

Officer Review

↓

Officer Action

---

# 8. MVP Scope

Priority tiers (Core / Important / Cut / Stretch) are defined in `Feature List.md` — that document is authoritative for what gets built first. In short: the **officer risk queue is the first screen and the priority tier that must not be cut**; the enterprise-side module below is "Important," not core-blocking.

One sector will be built fully real end-to-end. Which sector — **dairy or agri-input retail** — is not yet decided (open question in `idea.md`); decide this before implementing sector-specific seasonality/commodity logic.

## Enterprise Module

- Create enterprise profile
- Update enterprise details
- Record income
- Record expenses
- Record savings
- Record loans
- View prediction
- View recommendations

---

## Officer Module

- Dashboard
- Risk Queue
- Enterprise Search
- Enterprise Details
- Cash Flow Forecast
- Why Card
- Officer Notes
- Override Alert
- Mark Action Taken

---

## Prediction Module

Generate:

- Optimistic Scenario
- Expected Scenario
- Pessimistic Scenario

Detect:

- Working Capital Dip
- Risk Level

Generate:

- Risk Explanation
- Recommendations

---

## Offline Support

- Offline data entry
- Local storage
- Synchronization after reconnect

---

# 9. Prediction Pipeline

Every prediction follows the same pipeline.

### Step 1

Baseline

Input:

- Financial Records
- Synthetic UPI Velocity

---

### Step 2

Seasonality

Input:

- Business Sector
- Seasonal Patterns

---

### Step 3

Market Adjustment

Input:

- Agmarknet Commodity Prices

---

### Step 4

Weather Deflator

Input:

- Meteostat Weather Data

---

### Output

- Cash Flow Forecast
- Working Capital Prediction
- Risk Level
- Why Explanation
- AI Recommendation

---

# 10. Explainability

Every prediction must clearly answer:

Why was this enterprise flagged?

What contributed to the risk?

What should the officer do?

The system must never display only a risk score.

Every alert requires a human-readable explanation.

---

# 11. AI Responsibilities

Artificial Intelligence is responsible for:

- Plain-language summaries
- Recommendations
- Explainability

Artificial Intelligence is NOT responsible for:

- Risk calculation
- Financial simulation
- Scenario generation

Core prediction remains deterministic and transparent.

---

# 12. External Data Sources

Full detail and per-source usage guidance lives in `Sources Reference.md`. Summary:

Foundation:

- Synthetic Financial Dataset (Faker/Mimesis, fixed seed, calibrated against NPCI aggregate stats and UPAg `farmers_survey`/`nsso`) — generated once and checked into the repo, not regenerated per demo run.

Primary (wired into the live pipeline):

- UPAg `agmarknet` — mandi commodity prices, Step 3
- Meteostat — historical weather, Step 4

Secondary / optional (only if time permits, cut first under time pressure):

- Other UPAg sources (`enam`, `wpi`, `pmfby_ay`, `ncdex`, etc.)
- NPCI product statistics — calibration only, never a live per-enterprise input
- Sahamati Account Aggregator schema — shape reference only; AA integration itself is mocked, not live, for the hackathon
- Kaggle "UPI Transactions Generator" — reference implementation pattern only, not a plug-in dataset

---

# 13. Functional Requirements

The application must provide:

✓ Enterprise Management

✓ Financial Record Management

✓ Risk Queue

✓ Cash Flow Forecast

✓ Triple Scenario Chart

✓ Working Capital Alert

✓ Why Card

✓ AI Recommendation

✓ Officer Actions

✓ Offline Support

---

# 14. Non-Functional Requirements

The application should:

- Be responsive
- Support desktop and tablet
- Support offline usage
- Load quickly
- Handle API failures gracefully
- Provide clear loading and error states

---

# 15. Out of Scope (Hackathon)

The following features will NOT be implemented (full list with rationale in `Feature List.md`):

- Loan approval / credit underwriting / lending decisioning of any kind — Fasal Becho is explicitly a triage layer, not a credit-scoring product, and this will not become in-scope in a future version either
- Payment gateway
- Live Account Aggregator / Sahamati sandbox integration — mocked only, schema-shape reference used instead
- Multilingual / vernacular interface
- Prophet + ARIMA "rigor baselines" — one deterministic model plus the honest circularity caveat is enough
- Secondary UPAg sources (`enam`, `wpi`, `pmfby_ay`, `ncdex`, `pink_sheet`)
- Monitoring dashboard / sector heat map
- Portfolio-level analytics beyond the officer's own risk queue
- Multiple business sectors (one sector, fully real, is the MVP scope)
- Notification system

Most of these are future roadmap items (Section 17); credit underwriting/lending decisioning is a permanent non-goal, not a deferred feature.

---

# 16. Success Criteria

The MVP is successful when a judge can complete the following flow:

1. Open Officer Dashboard

2. View Risk Queue

3. Open a High-Risk Enterprise

4. View Cash Flow Prediction

5. Understand the "Why" explanation

6. Read AI Recommendations

7. Record an Officer Action

without requiring any explanation from the development team.

---

# 17. Future Roadmap

Future versions may include:

- Multiple enterprise sectors
- Live banking integrations
- Account Aggregator connectivity
- Multilingual support
- Portfolio analytics
- District heatmaps
- Mobile application
- Notification engine
- Advanced ML models

---

# 18. Guiding Principle

Fasal Becho does not replace field officers.

It enables them to make faster, more informed, and more explainable decisions.

Every feature added to the platform should reinforce this principle.