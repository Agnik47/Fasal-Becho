# PROMPTS.md

# Fasal Becho — AI Prompt Library

Version: 1.0

Status: Active

---

# Purpose

This document contains reusable prompts for developing the Fasal Becho application.

It acts as the operating manual for AI coding assistants (Claude, ChatGPT, Gemini, etc.).

The prompts are organized according to the software development lifecycle.

Always follow the workflows defined here.

Never skip steps.

---

# AI Development Principles

Before generating any code:

- Read `idea.md` (honest product framing — do not overclaim AI forecasting)
- Read `Feature List.md` (Core/Important/Cut/Stretch priority tiers)
- Read `Sources Reference.md` (how each external data source is actually used)
- Read AGENTS.md
- Read PROJECT_RULES.md
- Read CURRENT_WORKFLOW.md
- Understand the requested feature
- Build only one major feature at a time
- Never break existing functionality
- Verify before completion

---

# Standard Development Workflow

Every feature should follow this workflow.

```
Understand Feature
        ↓
Read Documentation
        ↓
Plan
        ↓
Design
        ↓
Implement
        ↓
Review
        ↓
Fix Issues
        ↓
Test
        ↓
Complete
        ↓
Update Documentation
```

Never skip verification.

---

# Prompt 1 — Understand a Feature

Use before writing code.

```
Read idea.md, Feature List.md, Sources Reference.md, AGENTS.md, PROJECT_RULES.md, PRD.md, ARCHITECTURE.md, DATABASE.md, API.md, ROADMAP.md, TASKS.md, and CURRENT_WORKFLOW.md.

Understand the requested feature.

Explain:

• Why this feature exists.

• Which users will use it.

• Which existing modules it affects.

• Which files are likely to change.

Do not write code.

Only provide the implementation plan.
```

---

# Prompt 2 — Feature Planning

```
Create a complete implementation plan for the requested feature.

Include:

- Architecture impact
- Database impact
- API impact
- UI impact
- Business logic
- Validation
- Error handling
- Dependencies

Do not generate code.

Wait for approval.
```

---

# Prompt 3 — UI Development

```
Build only the frontend UI.

Rules:

- React
- Next.js
- TypeScript
- Tailwind
- shadcn/ui

Do NOT write backend code.

Do NOT use mock business logic.

Keep components reusable.

Keep accessibility in mind.
```

---

# Prompt 4 — Backend Development

```
Build only the backend.

Include:

- Route Handlers
- Validation
- Services
- Error Handling

Do not build UI.

Follow the architecture.

Never duplicate business logic.
```

---

# Prompt 5 — Database

```
Generate the Prisma schema.

Requirements:

- Follow DATABASE.md
- Add relationships
- Add enums
- Add indexes
- Add constraints

Do not generate unnecessary tables.
```

---

# Prompt 6 — API Development

```
Implement APIs following API.md.

Every endpoint must include:

- Validation
- Authorization
- Error Handling
- Typed Responses

Do not expose secrets.

Do not bypass services.
```

---

# Prompt 7 — Business Logic

```
Implement the business logic.

Rules:

Business logic belongs only inside the Business Layer.

Never place calculations inside React components.

Never duplicate calculations.

Keep functions modular.
```

---

# Prompt 8 — Prediction Engine

```
Implement the Prediction Engine.

Pipeline:

Financial Baseline

↓

Seasonality

↓

Commodity Prices

↓

Weather

↓

Scenario Generation

↓

Risk Score (deterministic arithmetic + the offline-exported risk-model weights — see ARCHITECTURE.md Section 3a)

↓

Explainability (built from the risk model's SHAP-style attributions)

Do not use Gemini or any generative AI to calculate risk.

The risk model itself is never trained or invoked live inside this module — it is a Python artifact trained offline and checked into the repo; this module only loads its exported weights and applies them as plain arithmetic.

Keep calculations deterministic.
```

---

# Prompt 9 — Gemini Integration

```
Integrate Gemini only for:

- Why Card
- Recommendation
- Plain-language summaries

Never use Gemini for:

- Risk score
- Financial calculations
- Scenario generation

Keep prompts deterministic.
```

---

# Prompt 10 — External APIs

```
Integrate:

UPAg

Meteostat

Requirements:

- Service Layer only
- Retry mechanism
- Timeout handling
- Error handling
- Typed responses

Never call APIs directly inside React components.
```

---

# Prompt 11 — Code Review

```
Review the entire implementation.

Check:

Architecture

Performance

Readability

TypeScript

Security

Maintainability

Naming

Folder Structure

Return:

Critical Issues

Warnings

Suggestions
```

---

# Prompt 12 — Bug Finding

```
Act as a senior software engineer.

Find:

Logic bugs

Edge cases

Race conditions

Validation issues

Performance problems

Memory leaks

Security issues

Do not fix them.

Only report them.
```

---

# Prompt 13 — Bug Fixing

```
Fix every reported issue.

Do not refactor unrelated files.

Keep changes minimal.

Explain every fix.
```

---

# Prompt 14 — Refactoring

```
Refactor only the requested module.

Goals:

Improve readability.

Reduce duplication.

Keep behavior identical.

Do not change APIs.

Do not break tests.
```

---

# Prompt 15 — Performance Review

```
Review performance.

Check:

Rendering

Database

Queries

Caching

Bundle Size

API Calls

Suggest optimizations.

Only optimize if measurable.
```

---

# Prompt 16 — Security Review

```
Review security.

Check:

Authentication

Authorization

Input Validation

Secrets

Environment Variables

API Security

SQL Injection

XSS

CSRF

Rate Limiting

Return findings.
```

---

# Prompt 17 — Accessibility Review

```
Review accessibility.

Check:

Labels

Keyboard Navigation

Focus States

ARIA

Contrast

Responsive Design

Return issues.
```

---

# Prompt 18 — Deployment

```
Prepare the application for production.

Check:

Environment Variables

Database

Build

Lint

TypeScript

Assets

Performance

Deployment Readiness
```

---

# Prompt 19 — Documentation

```
Update:

CURRENT_WORKFLOW.md

TASKS.md

CHANGELOG.md

DECISIONS.md

Only update affected sections.

Do not rewrite unrelated documentation.
```

---

# Prompt 20 — Complete Feature Workflow

This is the prompt that should be used most often.

```
You are the lead software engineer for Fasal Becho.

Before writing code:

Read:

- idea.md
- Feature List.md
- Sources Reference.md
- AGENTS.md
- PROJECT_RULES.md
- PRD.md
- ARCHITECTURE.md
- DATABASE.md
- API.md
- ROADMAP.md
- TASKS.md
- CURRENT_WORKFLOW.md

Then:

1. Understand the requested feature.

2. Explain the implementation plan.

3. Identify affected files.

4. Wait for approval.

5. Implement only one feature.

6. Verify functionality.

7. Review code quality.

8. Fix issues.

9. Update documentation.

Never skip any step.

Never modify unrelated files.

Never break existing functionality.
```

---

# Recommended AI Workflow

Every development session should follow this order.

```
1. Read Documentation

↓

2. Read CURRENT_WORKFLOW.md

↓

3. Read TASKS.md

↓

4. Understand Feature

↓

5. Create Plan

↓

6. Wait for Approval

↓

7. Implement

↓

8. Self Review

↓

9. Bug Review

↓

10. Fix Issues

↓

11. Test

↓

12. Update Docs

↓

13. Commit
```

---

# AI Rules

Always:

- Think before coding.
- Build one feature at a time.
- Follow the architecture.
- Respect module boundaries.
- Keep business logic separate.
- Use strict TypeScript.
- Keep code readable.
- Verify before completion.

Never:

- Guess requirements.
- Skip documentation.
- Overengineer.
- Introduce unnecessary dependencies.
- Break existing functionality.
- Mix UI with business logic.
- Use AI for deterministic calculations.

---

# Guiding Principle

Every prompt in this document exists to ensure that development remains structured, maintainable, and aligned with the project's architecture.

The objective is not to generate code quickly—it is to generate correct, maintainable, and production-quality code that helps field officers identify which rural enterprises need attention first.