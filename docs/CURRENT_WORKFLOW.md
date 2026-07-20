# CURRENT_WORKFLOW.md

# Fasal Becho — Current Development Workflow

Version: 1.0

Status: Active Development

Last Updated: YYYY-MM-DD

---

# Purpose

This document represents the **current state of the project**.

Unlike the PRD or Roadmap, this file changes frequently.

It answers one question:

> **"If development starts right now, what should be worked on?"**

Claude should always consult this document before implementing any feature.

If this document conflicts with older documentation, this document represents the latest development state.

---

# Current Project Status

## Project Stage

🟨 Planning

Current Phase:

Phase 0 — Project Foundation

Overall Progress:

```
Planning          ██████████ 100%

Development       ░░░░░░░░░░   0%

Testing           ░░░░░░░░░░   0%

Deployment        ░░░░░░░░░░   0%
```

---

# Current Sprint

## Sprint Objective

Set up the complete application foundation.

The objective of this sprint is **not** to build business features.

The objective is to prepare a clean architecture for future development.

---

## Current Focus

Current Feature

> Project Setup

Current Module

> Foundation

Current Priority

> Highest

---

# Active Tasks

## In Progress

None

---

## Ready To Start

Highest Priority

- Project initialization
- Next.js setup
- TypeScript configuration
- Tailwind configuration
- shadcn/ui installation
- Prisma setup
- PostgreSQL connection
- Environment configuration
- Folder structure
- Git repository configuration

---

## Waiting

None

---

## Blocked

None

---

# Current Development Order

The following order must be respected.

```
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

Testing

↓

Deployment
```

No feature should skip this order unless explicitly approved.

---

# Current Module

## Project Foundation

### Goal

Create a production-ready application structure.

### Deliverables

- Next.js configured
- TypeScript configured
- Tailwind configured
- shadcn/ui installed
- Prisma installed
- PostgreSQL connected
- Environment variables configured
- Basic routing working

---

# Current Constraints

During the current phase:

Do NOT implement:

- Prediction logic
- Dashboard UI
- Financial calculations
- External API integrations
- Gemini integration
- Weather integration
- Commodity integration

Those belong to later phases.

---

# Current Architecture Decisions

Confirmed

- Next.js
- React
- TypeScript
- Tailwind CSS
- shadcn/ui
- Prisma
- PostgreSQL
- IndexedDB
- PWA
- Gemini
- UPAg (`agmarknet` source)
- Meteostat
- Faker/Mimesis (synthetic data generator, fixed seed)

Pending

- Authentication provider
- Deployment environment variables
- Demo seed dataset (generation is confirmed as Faker/Mimesis + fixed seed; the actual frozen dataset still needs to be produced and checked in)
- Business sector (dairy vs agri-input retail)

---

# Current Folder Ownership

Application Folder

Responsible for:

- Product
- Dashboard
- Business logic
- APIs
- Database
- Prediction Engine

Landing Page

Separate project

Currently out of scope.

---

# Current Success Criteria

The current sprint is complete when:

- Project runs locally
- No build errors
- No TypeScript errors
- Prisma connects successfully
- Database migration succeeds
- Basic application structure exists

Nothing else is required during this phase.

---

# Known Risks

Current risks:

- External API availability
- Synthetic dataset quality
- Prediction algorithm tuning

Open questions from `idea.md` — answer these this week, cheap now, expensive later:

- Is `agmarknet` confirmed inside the UPAg account's `user-allowed-sources`? (Not yet checked.)
- Does Meteostat have usable station coverage for the actual target pin codes? (Not yet checked.)
- Which one sector — dairy or agri-input retail? (Not yet decided. Note: choosing dairy also pulls in UPAg `mncfc`/`dcs`/`dafw_state`/`dafw_district` fodder-crop yield data per `Sources Reference.md`; agri-input retail doesn't need those sources at all.)

These should not block project setup, but should be resolved before Phase 4 (Prediction Engine) begins.

---

# Current Assumptions

The project currently assumes:

- One business sector — **Dairy or Agri-Input Retail, not yet decided** (see Known Risks below and `idea.md`)
- Synthetic financial data (Faker/Mimesis, fixed seed, generated once and checked into the repo — not regenerated per demo run)
- Single deployment
- One backend
- One database
- One frontend application

These assumptions may change after the MVP.

---

# Next Milestone

After completing the Project Foundation phase:

→ Begin Database Implementation

Deliverables:

- Database schema
- Prisma models
- Initial migration
- Seed script

---

# Definition of Done

The current sprint is complete only when:

- All foundation tasks are completed.
- The application builds successfully.
- Database connectivity is verified.
- Folder structure follows the architecture.
- Documentation is updated.

Only then should development move to the next phase.

---

# Update Rules

This document must be updated whenever:

- A sprint starts
- A sprint ends
- A phase changes
- A major decision changes
- A blocker is discovered
- A milestone is completed

It should always reflect the current reality of the project.

---

# Developer Notes

- Focus on one major feature at a time.
- Do not start future phases early.
- Keep commits small and focused.
- Update TASKS.md after completing work.
- Update CHANGELOG.md after each milestone.
- Update DECISIONS.md whenever a new architectural decision is made.

---

# Guiding Principle

This document is the project's working memory.

Every development session should begin by reviewing this file to understand the current state of the project before writing any code.