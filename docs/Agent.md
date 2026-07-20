# AGENTS.md

# Fasal Becho — AI Engineering Guide

> This document is the **source of truth** for every engineering decision in this repository.
>
> Before implementing any feature, read this file completely.
>
> If another document conflicts with this file, **AGENTS.md always wins** — except on product framing and feature priority, where `idea.md`, `Feature List.md`, and `Sources Reference.md` are authoritative. This file operationalizes their decisions; it should never contradict them.

---

# Project Overview

## Product

**Fasal Becho**

A **rule-based, explainable decision-support platform** — not an AI-forecasting product — that helps one NABARD field officer triage hundreds of rural micro-enterprises instead of dozens, flagging which ones are likely to experience cash-flow stress before it becomes a financial crisis.

Fasal Becho is **NOT** a lending platform.

Fasal Becho is **NOT** a credit-scoring platform.

Fasal Becho is **NOT** an AI model claiming proven predictive accuracy on real-world cash flow — that claim doesn't hold up on synthetic, rule-generated data.

Fasal Becho is a **human-in-the-loop triage system**: transparent, auditable rule logic that turns four signals into a priority + a plain-language reason, with AI used only for the explanation layer on top.

It combines a synthetic financial dataset (Faker/Mimesis, fixed seed), UPAg `agmarknet` commodity prices, and Meteostat weather signals to generate explainable cash-flow scenarios and prioritize enterprises that require human attention. See `idea.md` for the full honest framing and `Sources Reference.md` for how each data source is actually used.

---

# Core Philosophy

Always optimize for:

- Simplicity
- Explainability
- Reliability
- Demo quality
- Clean architecture

Never optimize for:

- Academic complexity
- Fancy AI buzzwords
- Unnecessary abstractions
- Features outside MVP
- Premature optimization

The goal is **to win a hackathon**, not build a production banking platform.

---

# Product Principles

Every feature should support this story:

Enterprise

↓

Financial Data

↓

Scenario Pipeline

↓

Risk Prediction

↓

Explainability

↓

Officer Action

If a feature does not strengthen this flow,
do not build it.

---

# Primary User

The primary user is NOT the enterprise.

The primary user is:

**NABARD Field Officer**

Every UX decision should prioritize helping a field officer answer one question:

> Which enterprise should I visit first?

---

# Product Scope

We are building ONE responsive web application (PWA).

The application has two roles.

## Enterprise

Can:

- Manage enterprise profile
- Enter income
- Enter expenses
- Enter loans
- View prediction
- View recommendations
- Work offline
- Sync later

## Field Officer

Can:

- View risk queue
- Search enterprises
- View enterprise profile
- View prediction
- View "Why" explanation
- Record action taken
- Override alert

---

# Product Constraints

Do NOT build:

- Loan underwriting
- Credit approval
- Lending workflows
- Payment gateway
- Multi-language support
- Heatmaps
- Portfolio analytics
- Multiple sectors
- Real Account Aggregator integration (mocked/schema-shape only — see `Sources Reference.md`)
- Prophet / ARIMA "rigor baseline" models
- Secondary UPAg sources beyond `agmarknet` (unless core is solid with time to spare)

Most of these are future features. Loan underwriting, credit approval, and lending workflows are a **permanent non-goal** — Fasal Becho is a triage layer, not a credit-scoring product, and that will not change in future versions either.

---

# MVP

The demo must successfully show:

✓ Enterprise data

↓

✓ Scenario generation

↓

✓ Triple-line chart

↓

✓ Working Capital Dip detection

↓

✓ Why Card

↓

✓ Risk Queue

↓

✓ Officer Action

If this works, the MVP is complete.

---

# Prediction Pipeline

Every prediction follows exactly four steps.

1.

Baseline

↓

UPI Velocity

2.

Seasonality

↓

Business Type

↓

Seasonal Curve

3.

Market Reality

↓

Agmarknet

↓

Commodity Price Adjustment

4.

Weather Deflator

↓

Meteostat

↓

Climate Stress

Never skip this order.

---

# Explainability

Every prediction must answer:

1.

Why was this flagged?

2.

Which factors contributed?

3.

What action should be taken?

Never return only:

"Risk = High"

Always explain the reasoning.

---

# AI Usage

Two things are both called "AI" in this project — do not conflate them:

- **Gemini** (generative, runtime) — used ONLY for recommendation generation, plain-language summaries, and turning risk-model attributions into Why Card sentences.
- **The risk model** (scikit-learn + SHAP, offline) — the project's actual AI/ML development. Trained once in Python, exported as static weights, applied deterministically by the running app. See `ARCHITECTURE.md` Section 3a.

Gemini is NOT responsible for, and must never be called to perform:

- Core prediction
- Risk scoring
- Scenario generation

The risk model contributes to the risk score, but only via its offline-exported weights applied as plain arithmetic — never a live inference call inside a request. Core prediction, including the risk model's contribution, remains deterministic, reproducible, and transparent.

---

# Data Sources

Full guidance per source: `Sources Reference.md`.

Foundation (everything else calibrates against this):

- Synthetic Financial Dataset — Faker/Mimesis, fixed seed, generated once and checked into the repo
- Risk Model + SHAP attributions — trained once offline in Python (scikit-learn + shap) on the dataset above, exported as static weights; this is the project's actual AI/ML development, and it is never a live service (see `ARCHITECTURE.md` Section 3a)

Primary (wired into the live pipeline):

1.

UPAg `agmarknet` — Step 3, Market Reality (this is the one live-API call the pre-submission checklist requires)

2.

Meteostat — Step 4, Weather Deflator

Secondary / optional / cut first under time pressure:

- Other UPAg sources (`enam`, `wpi`, `pmfby_ay`, `ncdex`, etc.)
- NPCI aggregate stats — calibration only, never a live per-enterprise input
- Sahamati AA schema — shape reference only; AA integration itself is mocked
- Kaggle UPI generator notebook — reference implementation pattern only

---

# Tech Stack

Frontend

- Next.js
- React
- TypeScript
- Tailwind CSS
- shadcn/ui
- Recharts
- Framer Motion

Backend

- Next.js Route Handlers
- Prisma
- PostgreSQL

Data Science (offline only — see ARCHITECTURE.md Section 3a; never part of the deployed application, never called at request time)

- Python
- Faker / Mimesis
- NumPy / Pandas
- scikit-learn (the risk model)
- SHAP (feature attribution)
- Meteostat Python client (exploration/validation only — the running app calls Meteostat's HTTP API directly)

Generative AI (runtime, language only)

- Gemini

Offline (client, PWA)

- PWA
- IndexedDB

Deployment

- Vercel

Two different "offline"s appear above — don't confuse them: the **Data Science** row is offline in the sense of "runs before deployment, not at request time." The **Offline (client, PWA)** row is offline in the sense of "the mobile/tablet client works without a network connection." They are unrelated.

---

# Development Rules

Always:

Build one feature at a time.

Never build multiple major features in one prompt.

Every feature must be:

Plan

↓

Implement

↓

Verify

↓

Complete

↓

Commit

before moving forward.

---

# Architecture Rules

Keep business logic separate from UI.

Prediction logic must remain modular.

External APIs must remain isolated.

Avoid tightly coupled components.

Never duplicate logic.

---

# Coding Rules

Prefer:

- TypeScript strict mode
- Readable code
- Small functions
- Reusable components

Avoid:

- any
- Massive files
- Deep nesting
- Unnecessary libraries
- Premature abstractions

---

# UI Principles

Prioritize:

- Clean dashboards
- Fast readability
- Large charts
- Clear risk indicators

Use:

🟢 Low Risk

🟡 Medium Risk

🔴 High Risk

Keep the UI minimal.

This is a fintech decision-support tool.

---

# Performance Rules

Keep:

- Fast initial load
- Lazy loading where useful
- Lightweight bundle

Never optimize prematurely.

Correctness > optimization.

---

# Demo Rules

The demo should begin from:

Field Officer Dashboard

NOT Landing Page.

The first thing judges should see is:

Risk Queue

↓

Enterprise

↓

Prediction

↓

Why Card

↓

Recommendation

This is the strongest storytelling flow.

---

# Out of Scope

`Feature List.md` is the authoritative priority tiering (Core / Important / Cut / Pitch-only / Stretch) — read it before deciding what to cut. Summary:

If implementation time becomes limited, cut in this order (Important tier, build only if Core is solid):

1. Second sector (config stub, not a full model)
2. Enterprise-side self-entry view
3. Enterprise profile drill-down
4. Live `agmarknet` API call (fall back to a frozen pull if time runs out — but try hard not to cut this one, it's the pre-submission checklist item)

Never cut (Core tier — the demo doesn't work without these):

- Risk Queue (first screen)
- Triple Chart
- Working Capital Dip alert
- Why Card
- Prediction Pipeline
- Officer override
- Offline entry + sync
- One sector, fully real

These define the product.

---

# Before Every Task

Claude must:

1.

Understand the requested feature.

2.

Read AGENTS.md.

3.

Identify affected files.

4.

Protect existing functionality.

5.

Implement only the requested feature.

6.

Verify nothing else broke.

7.

Avoid unrelated refactoring.

---

# Definition of Done

A feature is complete only when:

✓ Builds successfully

✓ No TypeScript errors

✓ No console errors

✓ Existing features still work

✓ Responsive UI

✓ Matches product philosophy

Only then move to the next feature.

---

# Final Reminder

Fasal Becho is not trying to replace human judgment.

It helps field officers decide:

**"Who needs my attention first?"**

Every engineering decision should reinforce that single idea.