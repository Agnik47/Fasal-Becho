# ARCHITECTURE.md

# Fasal Becho — System Architecture

Version: 1.0

Status: MVP Architecture

This document defines the system architecture for Fasal Becho.

It is the engineering blueprint of the application.

It explains:

- system boundaries
- module responsibilities
- data flow
- business logic ownership
- service communication
- architectural constraints

If another document conflicts with this architecture,
AGENTS.md takes precedence.

---

# 1. Architecture Philosophy

Fasal Becho follows a modular service-oriented architecture.

Every responsibility should belong to exactly one module.

The architecture prioritizes:

- Simplicity
- Maintainability
- Explainability
- Offline-first experience
- Human-in-the-loop decision support
- Clear separation of concerns

Never optimize for unnecessary complexity.

Never introduce microservices.

Never introduce event buses.

Never split the application into multiple backends.

Everything runs inside one Next.js application.

**Exception, stated explicitly:** the offline Python data-science pipeline (Section 3a) is tooling, not a backend. It never runs as part of the deployed application, is never called over HTTP by it, and has no request/response lifecycle. It runs once, ahead of time, and produces static artifacts (the frozen synthetic dataset and the exported risk-model weights) that get checked into the repo and read by the Next.js application like any other static data file. This does not violate "one backend" — there is still exactly one thing that serves requests.

---

# 3a. Offline Data-Science Pipeline (Python, not part of the running application)

This is where the project's actual AI/ML development happens — and it is deliberately kept out of the request path.

Responsibilities (run manually / as one-off scripts, not deployed):

- Generate the synthetic financial/transaction dataset (Faker/Mimesis, `numpy.random.lognormal` for amounts, fixed seed)
- Calibrate the generator's macro assumptions against NPCI aggregate stats and UPAg `farmers_survey`/`nsso`
- Train a small, transparent risk model (`scikit-learn` — linear/logistic regression or a shallow gradient-boosted tree) on the frozen dataset
- Compute `shap` feature attributions for that model
- Export both the frozen dataset and the trained model's weights/baselines as static JSON, checked into the repo

Tooling: Python, Faker, Mimesis, NumPy, Pandas, scikit-learn, SHAP, the Meteostat Python client (for exploring/validating weather coverage — the running app calls Meteostat's HTTP API directly, it does not shell out to Python).

What the running Next.js application does with the output: loads the exported dataset and model weights as static data, and applies the model **deterministically** — plain arithmetic over fixed weights, not a live inference call. The Prediction Module (Section 4) and Prediction Pipeline (Section 5) below own this application logic. Gemini is a separate, unrelated integration used only for turning the resulting numbers into language (Section 8) — it never touches the weights or the arithmetic.

Full guidance and exact library versions/commands: `Sources Reference.md`.

---

# 2. High Level Architecture

                          Users
                             │
            ┌────────────────┴────────────────┐
            │                                 │
     Enterprise Portal              Officer Dashboard
            │                                 │
            └────────────────┬────────────────┘
                             │
                   Next.js Application
                             │
      ┌──────────────────────┼──────────────────────┐
      │                      │                      │
 UI Layer            Business Layer          Service Layer
      │                      │                      │
      └──────────────────────┼──────────────────────┘
                             │
                     Prediction Engine
                             │
      ┌───────────────┬──────────────┬──────────────┐
      │               │              │
 Financial Data   Weather API   Market API
      │               │              │
      └───────────────┴──────────────┘
                     │
               PostgreSQL Database

---

# 3. Application Layers

The application consists of five logical layers.

Layer separation must always be respected.

──────────────────────────────

Layer 1

Presentation Layer

──────────────────────────────

Responsible for:

- Pages
- Components
- Forms
- Charts
- Tables
- Dashboards

Must NEVER contain:

- Prediction logic
- API integration logic
- Database queries
- Financial calculations

UI only displays data.

UI never creates business rules.

---

Layer 2

Application Layer

Responsible for:

- Request handling
- Validation
- Authentication
- Authorization
- Session handling

Coordinates the request.

Does not perform prediction.

Does not perform calculations.

Simply orchestrates services.

---

Layer 3

Business Layer

This is the heart of Fasal Becho.

Owns:

- Risk calculation
- Scenario generation
- Working capital logic
- Explainability generation
- Recommendation generation
- Officer workflow

Business rules belong ONLY here.

No business logic inside UI.

No business logic inside database.

---

Layer 4

Service Layer

Responsible for:

External systems.

Examples:

UPAg

Meteostat

Gemini

Synthetic Dataset + Risk Model artifacts (static, produced by the offline Python pipeline in Section 3a — read, not called)

Every external integration must have its own service.

Never call external APIs directly inside pages.

Never duplicate API requests.

---

Layer 5

Persistence Layer

Responsible for:

Database

Caching

Offline storage

Synchronization

Nothing else.

---

# 4. Core System Modules

The system is divided into independent modules.

────────────────────────

Authentication Module

────────────────────────

Responsibilities

- Login
- Session
- User role
- Access control

Should never know prediction logic.

────────────────────────

Enterprise Module

────────────────────────

Responsibilities

- Enterprise profile
- Business details
- Sector
- Financial records

Owns enterprise lifecycle.

────────────────────────

Financial Module

────────────────────────

Responsibilities

- Income
- Expenses
- Loans
- Savings

Only stores facts.

Never predicts anything.

────────────────────────

Prediction Module

────────────────────────

Responsible for:

3–6 month simulation

Working Capital Dip

Scenario generation

Cash Flow Forecast

Risk Level

Applying the offline-trained risk model's exported weights (Section 3a) and computing the matching SHAP-style attribution values

This module owns all prediction logic. It reads the exported model artifact as static data — it does not train models, and it never calls a live Python process.

────────────────────────

Risk Module

────────────────────────

Consumes prediction output.

Produces:

Low

Medium

High

Priority ordering

Officer queue

────────────────────────

Recommendation Module

────────────────────────

Consumes:

Prediction

Risk

Weather

Market

Generates:

Plain-language advice

Never calculates risk.

────────────────────────

Officer Module

────────────────────────

Responsible for:

Risk Queue

Enterprise Review

Officer Notes

Override

Action History

Officer workflow only.

────────────────────────

Offline Module

────────────────────────

Responsible for:

Offline storage

Pending requests

Background synchronization

Conflict handling

No UI logic.

---

# 5. Prediction Pipeline

Every prediction MUST follow this exact order.

Step 1

Financial Baseline

↓

Income

Expenses

Loans

Savings

↓

Synthetic UPI Velocity

↓

Step 2

Seasonality

↓

Business Type

↓

Seasonal Curve

↓

Step 3

Market Adjustment

↓

Agmarknet

↓

Commodity Prices

↓

Revenue Adjustment

↓

Step 4

Weather Deflator

↓

Meteostat

↓

Climate Stress

↓

Step 5

Scenario Generation

↓

Optimistic

Expected

Pessimistic

↓

Step 6

Risk Detection

↓

Working Capital Dip

↓

Apply exported risk-model weights (Section 3a) — deterministic arithmetic, not a live inference call

↓

Risk Score

↓

Compute matching SHAP-style feature attributions

↓

Step 7

Explainability

↓

Why Card (built from the Step 6 attributions)

↓

Recommendations (Gemini — language only, never recalculates the score)

Never change this order.

---

# 6. Request Flow

Example:

Officer opens Enterprise

↓

Application validates request

↓

Fetch enterprise

↓

Fetch financial records

↓

Fetch prediction

↓

If prediction missing

↓

Run Prediction Engine

↓

Store prediction

↓

Return response

↓

Render UI

Every page follows similar flow.

---

# 7. Data Ownership

Enterprise Module

owns

Enterprise

Financial Module

owns

Financial Records

Prediction Module

owns

Prediction

Officer Module

owns

Actions

Recommendation Module

owns

Recommendations

Modules should never modify another module's data directly.

---

# 8. External Integrations

UPAg

Purpose

Commodity prices — `agmarknet` source only for MVP. Other UPAg sources (`enam`, `wpi`, `pmfby_ay`, `ncdex`, etc.) are optional/cut; see `Sources Reference.md` before wiring any of them up.

Only Market Service may access it.

------------------------------------------------

Meteostat

Purpose

Weather history, by station/lat-long resolved from the Enterprise's Pin Code field (`DATABASE.md`).

IMD's official gridded rainfall dataset is cited in the deck as "what a production version would use" — it is not integrated; Meteostat is the engineering choice for speed.

Only Weather Service may access it.

------------------------------------------------

Synthetic Data Generator + Risk Model

Purpose

Produces the transaction/financial baseline and the risk-model weights the Prediction Module applies. This is the Offline Data-Science Pipeline defined in **Section 3a** — not a live external API and not a service the running application calls; the application reads its exported artifacts as static data.

Only the Financial/Prediction service layer may read the exported artifacts.

------------------------------------------------

Gemini

Purpose

Recommendations

Explainability

Summaries

Gemini must NEVER calculate risk.

---

# 9. Offline Architecture

Internet Available

↓

Server

↓

Database

↓

Response

Offline

↓

IndexedDB

↓

Pending Queue

↓

Reconnect

↓

Sync Engine

↓

Server

Offline mode must always prioritize preserving user input.

Never discard unsynced data.

---

# 10. Caching Strategy

Cache:

Commodity prices

Weather

Predictions

Do NOT cache:

Officer actions

Enterprise edits

Authentication

---

# 11. Error Handling

Every service returns predictable responses.

Success

Failure

Loading

Empty

Timeout

No service should throw raw errors into the UI.

---

# 12. Security Principles

Never expose:

API Keys

Secrets

Tokens

Database credentials

Prediction APIs should validate all input.

Never trust client data.

---

# 13. Scalability

Future modules should plug into the Business Layer.

Examples:

SMS

Notifications

Bank Integration

Heatmaps

Portfolio Analytics

Additional sectors

Never rewrite existing modules.

Extend them.

---

# 14. Architecture Constraints

Never place:

Prediction logic inside React components.

Never place:

Database queries inside UI.

Never place:

Business rules inside API routes.

Never call:

External APIs from client components.

Never duplicate:

Prediction calculations.

Never calculate:

Risk inside Gemini.

Never bypass:

Business Layer.

---

# 15. Definition of Good Architecture

Good architecture means:

Small modules

Single responsibility

Predictable flow

Minimal coupling

Maximum readability

Business logic separated

Easy testing

Easy extension

Hackathon-friendly

---

# Final Principle

Every engineering decision should reinforce one goal:

Help a field officer identify which enterprise needs attention first.

If a new feature does not strengthen that workflow, it does not belong in the MVP.