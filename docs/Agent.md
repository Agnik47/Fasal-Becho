# AGENTS.md

# Fasal Becho — AI Engineering Guide

> This document is the **source of truth** for every engineering decision in this repository.
>
> Before implementing any feature, read this file completely.
>
> If another document conflicts with this file, **AGENTS.md always wins**.

---

# Project Overview

## Product

**Fasal Becho**

An AI-powered decision-support platform that helps NABARD field officers identify rural micro-enterprises likely to experience cash-flow stress before it becomes a financial crisis.

Fasal Becho is **NOT** a lending platform.

Fasal Becho is **NOT** a credit-scoring platform.

Fasal Becho is a **human-in-the-loop triage system**.

It combines synthetic financial data, commodity prices, weather signals and transaction proxies to generate explainable cash-flow scenarios and prioritize enterprises that require human attention.

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
- Real Account Aggregator integration

These are future features.

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

AI is used ONLY for:

- Recommendation generation
- Plain-language summaries
- Explainability

AI is NOT responsible for:

- Core prediction
- Risk scoring
- Scenario generation

Those should remain deterministic and transparent.

---

# Data Sources

Priority:

1.

Synthetic Financial Dataset

2.

UPAg Agmarknet

3.

Meteostat

Everything else is optional.

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

AI

- Gemini

Offline

- PWA
- IndexedDB

Deployment

- Vercel

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

If implementation time becomes limited:

Cut in this order:

1.

Second sector

2.

Additional APIs

3.

Advanced analytics

4.

Maps

5.

Notifications

Never cut:

- Risk Queue
- Triple Chart
- Why Card
- Prediction Pipeline

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