# TASKS.md

# Fasal Becho — Development Tasks

Version: 1.0

Status: Active

This document tracks the implementation progress of the Fasal Becho MVP.

Unlike the roadmap, this document contains actionable engineering tasks.

Tasks should be completed in order.

Do not begin lower-priority tasks before completing higher-priority ones unless explicitly approved.

---

# Task Status Legend

| Status | Meaning |
|---------|---------|
| ⬜ | Not Started |
| 🟨 | In Progress |
| ✅ | Completed |
| ⛔ | Blocked |
| 🔄 | Needs Revision |

---

# Sprint Goal

Current Goal:

> Build a complete, demo-ready MVP for the NABARD Hackathon.

Before Phase 4 starts, resolve the open questions in `idea.md`: which sector (dairy vs agri-input retail), `agmarknet` source access confirmation, and Meteostat pin-code coverage.

---

# Phase 0 — Project Setup

## Repository

- ⬜ Initialize Next.js project
- ⬜ Configure TypeScript
- ⬜ Configure Tailwind CSS
- ⬜ Configure shadcn/ui
- ⬜ Configure ESLint
- ⬜ Configure Prettier
- ⬜ Configure Husky (optional)
- ⬜ Configure environment variables

---

## Project Structure

- ⬜ Create app structure
- ⬜ Create shared components
- ⬜ Create service layer
- ⬜ Create business layer
- ⬜ Create utility functions
- ⬜ Configure aliases

---

## Database

- ⬜ Install Prisma
- ⬜ Configure PostgreSQL
- ⬜ Create initial schema
- ⬜ Run first migration
- ⬜ Seed development database

---

# Phase 1 — Authentication

## User System

- ⬜ User model
- ⬜ Officer model
- ⬜ Enterprise user model

---

## Authentication

- ⬜ Login
- ⬜ Logout
- ⬜ Session handling
- ⬜ Protected routes
- ⬜ Role-based access

---

# Phase 2 — Enterprise Management

## Enterprise

- ⬜ Create enterprise
- ⬜ View enterprise
- ⬜ Edit enterprise
- ⬜ Soft delete enterprise
- ⬜ Enterprise listing
- ⬜ Capture Pin Code (needed for Step 4 Meteostat weather lookup — see `Sources Reference.md` Section 2)

---

## Search

- ⬜ Search by enterprise name
- ⬜ Search by owner
- ⬜ Search by district

---

## Filters

- ⬜ Filter by district
- ⬜ Filter by business type

---

# Phase 3 — Financial Records

## Synthetic Data Generator

- ⬜ Faker/Mimesis transaction skeleton (IDs, timestamps, lognormal amounts)
- ⬜ P2M/P2P split logic (P2M received = income, P2M sent = expense, P2P = informal)
- ⬜ Weekend/temporal amplification pattern (adapted from the Kaggle UPI generator reference — `Sources Reference.md` Section 5)
- ⬜ Sector seasonality bias (crop/festival calendar)
- ⬜ Calibrate macro trend against NPCI aggregate stats
- ⬜ Calibrate household income/savings distributions against UPAg `farmers_survey`/`nsso`
- ⬜ Freeze with a fixed seed, generate once, check the dataset into the repo
- ⬜ Export dataset feature statistics (means/variance) needed for offline model training (Phase 4)

## CRUD

- ⬜ Add financial record
- ⬜ Edit financial record
- ⬜ Delete financial record
- ⬜ View financial history

---

## Financial Inputs

- ⬜ Revenue
- ⬜ Expenses
- ⬜ Savings
- ⬜ Outstanding loan
- ⬜ Shape mock financial-record JSON against the Sahamati `deposit.xsd` structure (balances, transaction entries, account summary) — for AA-integration credibility, not live validation (`Sources Reference.md` Section 6)

---

## Validation

- ⬜ Required fields
- ⬜ Numeric validation
- ⬜ Date validation

---

# Phase 4 — Prediction Engine

## Baseline

- ⬜ Revenue calculation
- ⬜ Expense calculation
- ⬜ Working capital calculation

---

## Scenario Engine

- ⬜ Optimistic scenario
- ⬜ Expected scenario
- ⬜ Pessimistic scenario

---

## External Adjustments

- ⬜ Seasonality adjustment (sector crop/festival calendar)
- ⬜ Commodity price adjustment (UPAg `agmarknet`)
- ⬜ Weather adjustment (Meteostat, via Enterprise Pin Code → lat/long lookup)

---

## Offline Risk Model & Explainability (Python — not part of the running app)

- ⬜ Train lightweight model (scikit-learn: linear/logistic regression or shallow gradient-boosted tree) on the frozen synthetic dataset
- ⬜ Hold out some generator parameters for train/test validation (Stretch rigor, see `Feature List.md`)
- ⬜ Compute SHAP feature attributions
- ⬜ Export model weights + feature baselines as static JSON, check into repo
- ⬜ Import exported weights into the TypeScript Prediction Engine and apply deterministically

---

## Risk Analysis

- ⬜ Risk score (deterministic arithmetic + exported model weights)
- ⬜ Risk level
- ⬜ Working capital dip detection
- ⬜ PMFBY claims cross-check for weather-deflator sanity (optional validation, Important tier — UPAg `pmfby_ay`, `Sources Reference.md` Section 1)

---

# Phase 5 — Explainability

## Why Card

- ⬜ Build explanation generator
- ⬜ Consume SHAP-style attribution values from the Prediction Engine
- ⬜ Display contributing factors

---

## AI Recommendations

- ⬜ Gemini integration
- ⬜ Recommendation generation
- ⬜ Plain-language summaries

---

# Phase 6 — Officer Dashboard

## Dashboard

- ⬜ Dashboard layout
- ⬜ Summary cards
- ⬜ Statistics

---

## Risk Queue

- ⬜ High-risk enterprises
- ⬜ Medium-risk enterprises
- ⬜ Low-risk enterprises
- ⬜ Sorting
- ⬜ Filtering

---

## Enterprise Review

- ⬜ Enterprise profile
- ⬜ Financial overview
- ⬜ Prediction overview
- ⬜ Recommendation view

---

## Officer Actions

- ⬜ Record action
- ⬜ View action history
- ⬜ Override alert

---

# Phase 7 — Offline Support

## Offline Storage

- ⬜ IndexedDB setup
- ⬜ Local cache
- ⬜ Pending queue

---

## Synchronization

- ⬜ Background sync
- ⬜ Retry failed sync
- ⬜ Conflict handling

---

# Phase 8 — UI & UX

## Components

- ⬜ Loading states
- ⬜ Error states
- ⬜ Empty states
- ⬜ Skeleton loaders

---

## Responsive Design

- ⬜ Mobile
- ⬜ Tablet
- ⬜ Desktop

---

## Accessibility

- ⬜ Keyboard navigation
- ⬜ Labels
- ⬜ Focus states

---

# Phase 9 — Testing

## Functional Testing

- ⬜ Authentication
- ⬜ Enterprise management
- ⬜ Financial records
- ⬜ Prediction engine
- ⬜ Dashboard

---

## API Testing

- ⬜ Enterprise APIs
- ⬜ Prediction APIs
- ⬜ Officer APIs

---

## Edge Cases

- ⬜ Empty data
- ⬜ Invalid input
- ⬜ API failures
- ⬜ Offline mode

---

# Phase 10 — Deployment

## Production

- ⬜ Configure production database
- ⬜ Configure environment variables
- ⬜ Deploy to Vercel
- ⬜ Verify production build

---

## Demo Preparation

- ⬜ Seed demo data
- ⬜ Verify complete demo flow
- ⬜ Fix UI issues
- ⬜ Performance review
- ⬜ Final bug fixes

---

# Stretch Goals

Only begin these after the MVP Core tier is complete — per `Feature List.md`, unlikely to be reached in hackathon time.

- ⬜ Train/test split holding back some generator parameters (show the model recovers *some* signal it wasn't directly given)
- ⬜ Real Meteostat coverage check + fallback logic for sparse-station pin codes
- ⬜ Second fully-built sector model

Cut for the hackathon entirely (not stretch goals — see `Feature List.md`):

- Multiple business sectors as a shipped feature (a config stub for a second sector is Important tier, not this)
- Portfolio analytics beyond the officer's own queue
- Heatmap dashboard
- Notification system
- Mobile optimization
- Prophet/ARIMA "rigor baseline" models
- Multi-language support
- Live Account Aggregator integration

---

# Known Issues

Use this section to record bugs that are discovered during development.

Example:

- None

---

# Technical Debt

Track shortcuts taken during the hackathon that should be revisited later.

Example:

- Replace synthetic data with production-ready sources.
- Improve prediction algorithm.
- Add automated testing.

---

# Completion Criteria

The MVP is considered complete when:

- ✅ All core features are implemented.
- ✅ All APIs are functional.
- ✅ Prediction engine works end-to-end.
- ✅ Officer dashboard is fully operational.
- ✅ Offline mode functions correctly.
- ✅ Demo flow is stable.
- ✅ No critical bugs remain.

---

# Current Focus

Only one major feature should be actively developed at a time.

Current Task:

> Define before starting development.

Next Task:

> Update after completing the current task.

Blocked Tasks:

> Record any blockers here.

---

# Guiding Principle

Progress is measured by working features, not lines of code.

Every completed task should move the product closer to helping a NABARD field officer identify which enterprise needs attention first.