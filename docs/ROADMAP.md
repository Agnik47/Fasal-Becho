# ROADMAP.md

# Fasal Becho — Development Roadmap

Version: 1.0

Status: Active

This document defines the development roadmap for the Fasal Becho MVP.

Its purpose is to provide a clear implementation sequence for both developers and AI coding assistants.

Every feature should be built in dependency order.

Do not skip phases.

Do not implement future roadmap items before completing the MVP.

---

# Development Philosophy

The roadmap follows one guiding principle:

> Build a complete product, not a collection of incomplete features.

Every phase should produce a working, testable application.

Each completed phase should leave the project in a deployable state.

---

# MVP Goals

The MVP should allow a judge to:

- Create and manage enterprises
- Record financial data
- Generate cash flow predictions
- View enterprise risk
- Understand prediction reasoning
- Receive AI recommendations
- Review enterprise risk from an officer dashboard
- Record officer interventions
- Experience offline-first functionality

Everything else is optional.

---

# Development Phases

## Phase 0 — Project Foundation

Status: Highest Priority

Objective:

Prepare the project structure and development environment.

Deliverables:

- Initialize Next.js project
- Configure TypeScript
- Configure Tailwind CSS
- Configure shadcn/ui
- Configure ESLint & Prettier
- Configure environment variables
- Configure Prisma
- Connect PostgreSQL
- Configure PWA
- Configure project folders

Outcome:

A clean project ready for development.

---

## Phase 1 — Database & Authentication

Objective:

Build the application's foundation.

Deliverables:

- User model
- Enterprise model
- Officer model
- Authentication
- Session management
- Role-based authorization
- Database migrations
- Seed data

Outcome:

Users can securely access the application.

---

## Phase 2 — Enterprise Management

Objective:

Allow enterprises to manage their business information.

Deliverables:

- Enterprise registration
- Enterprise profile
- Enterprise editing
- Enterprise listing
- Enterprise details
- Search
- Filtering

Outcome:

Enterprises exist inside the system.

---

## Phase 3 — Financial Records

Objective:

Capture business financial information.

Deliverables:

- Monthly income
- Monthly expenses
- Savings
- Loans
- Financial history
- Edit records
- Delete records

Outcome:

The application contains enough data for prediction.

---

## Phase 4 — Prediction Engine

Objective:

Generate cash flow forecasts.

Deliverables:

- Financial baseline (synthetic UPI velocity, from the Faker/Mimesis generator)
- Seasonality adjustment (sector crop/festival calendar)
- Market adjustment (UPAg `agmarknet` commodity prices)
- Weather adjustment (Meteostat)
- Offline risk model + SHAP attributions, trained once in Python and exported as static weights (`ARCHITECTURE.md` Section 3a) — this is the project's AI/ML development; the running app only ever applies the exported weights deterministically, it never trains or calls a live model
- Triple scenario generation
- Working capital calculation
- Risk score generation

Outcome:

Every enterprise receives predictions. Resolve the open questions in `idea.md`/`CURRENT_WORKFLOW.md` (agmarknet source access, Meteostat pin-code coverage, sector choice) before starting this phase.

---

## Phase 5 — Explainability Engine

Objective:

Explain predictions.

Deliverables:

- Why Card (built from the Phase 4 risk model's SHAP attributions, turned into sentences by Gemini)
- Plain-language summaries
- AI recommendations
- Business insights

Outcome:

Predictions become understandable.

---

## Phase 6 — Officer Dashboard

Objective:

Provide decision-support tools.

Deliverables:

- Dashboard
- Risk Queue
- Enterprise review page
- Prediction viewer
- Recommendation viewer
- Officer notes
- Intervention history

Outcome:

Officers can prioritize field visits.

---

## Phase 7 — Offline Support

Objective:

Support low-connectivity environments.

Deliverables:

- IndexedDB storage
- Offline form submission
- Sync queue
- Background synchronization
- Conflict handling

Outcome:

The application works without continuous internet.

---

## Phase 8 — Polish & Demo

Objective:

Prepare for hackathon presentation.

Deliverables:

- Loading states
- Error states
- Empty states
- Responsive design
- UI polish
- Demo data
- Performance improvements
- Bug fixes

Outcome:

Demo-ready MVP.

---

# Dependency Graph

Project Setup

↓

Database

↓

Authentication

↓

Enterprise Management

↓

Financial Records

↓

Prediction Engine

↓

Explainability

↓

Officer Dashboard

↓

Offline Support

↓

Polish

No phase should begin before its dependencies are complete.

---

# MVP Feature Checklist

## Core Features

- Enterprise Management
- Financial Records
- Prediction Engine
- Triple Forecast
- Working Capital Dip Detection
- Risk Score
- Why Card
- AI Recommendations
- Officer Dashboard
- Risk Queue
- Officer Actions
- Offline Support

All of these must be completed before considering the MVP finished.

---

# Stretch Goals

Only begin these after the MVP Core tier is complete — per `Feature List.md`, unlikely to be reached in hackathon time:

- Train/test split holding back some generator parameters, so the model can show it recovers *some* signal it wasn't directly given
- Real Meteostat coverage check + fallback logic for sparse-station pin codes
- Second fully-built sector model (beyond the config-stub version in the Important tier)

Stretch goals should never delay MVP completion.

Everything else that sounds like a stretch goal — multiple business sectors as a *shipped* feature, notifications, portfolio analytics, heatmaps, multi-language support, real Account Aggregator integration, mobile app — is explicitly **cut for the hackathon**, not a stretch goal to reach for. See "Future Roadmap" below and `Feature List.md`'s "Cut" section.

---

# Definition of Done for Each Phase

A phase is considered complete when:

- Features are implemented
- Code builds successfully
- No TypeScript errors
- No linting errors
- Responsive on supported devices
- Proper loading states
- Proper error handling
- Documentation updated

Only then should development move to the next phase.

---

# Development Rules

During implementation:

- Complete one phase at a time.
- Do not work on multiple unrelated phases simultaneously.
- Keep commits focused on a single feature or milestone.
- Verify functionality before marking a task complete.

---

# Success Criteria

The roadmap is complete when a judge can:

1. Log in.
2. Create or access an enterprise.
3. Enter financial records.
4. Generate a prediction.
5. Understand the "Why" explanation.
6. Review AI recommendations.
7. View the enterprise in the Officer Dashboard.
8. Record an officer action.
9. Experience offline functionality without issues.

---

# Future Roadmap

Beyond the hackathon, future releases may include:

- Live banking integrations
- Additional business sectors
- Portfolio analytics
- District-level dashboards
- Advanced forecasting models
- Notification engine
- Mobile applications
- Account Aggregator integration
- Multi-language support

These features are intentionally excluded from the MVP roadmap.

---

# Guiding Principle

Every completed phase should move the product closer to answering one question:

**"Which rural enterprise needs attention first, and why?"**

If a task does not contribute to that objective, it should not be prioritized during the hackathon.