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

Synthetic Dataset

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

This module owns all prediction logic.

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

Risk Score

↓

Step 7

Explainability

↓

Why Card

↓

Recommendations

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

Commodity prices

Only Market Service may access it.

------------------------------------------------

Meteostat

Purpose

Weather history

Only Weather Service may access it.

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