# PROJECT_RULES.md

# Fasal Becho — Engineering Rules

This document defines how development should happen.

These rules apply to every implementation.

If a prompt conflicts with these rules, ask for clarification instead of guessing.

---

# Development Philosophy

Build the smallest working version first.

Do not overengineer.

Do not build future features.

Do not optimize before measuring.

Prefer shipping a complete MVP over partially building many features.

---

# One Feature Rule

Every implementation request must contain only ONE major feature.

Good examples:

- Login page
- Risk Queue
- Triple Chart
- Enterprise CRUD
- Weather Integration

Bad examples:

- Build dashboard + authentication + charts
- Create backend + frontend + database together

If a request is too large,
split it into smaller implementation tasks.

---

# Feature Workflow

Every feature follows this order.

1.

Understand the task

↓

2.

Identify affected files

↓

3.

Implement

↓

4.

Verify

↓

5.

Fix issues

↓

6.

Complete

Never skip verification.

---

# Before Writing Code

Always understand:

- Why this feature exists
- Which user uses it
- How it fits the MVP
- Which existing code it affects

Never start coding immediately.

---

# Existing Code Protection

Never break existing functionality.

Never remove working code unless explicitly requested.

Never refactor unrelated modules.

Never rename files without approval.

Keep changes focused.

---

# File Modification Rules

Modify only files related to the requested feature.

Avoid touching unrelated components.

If a file becomes too large,
recommend splitting it instead of creating unnecessary abstractions.

---

# Architecture Rules

Keep clear separation between:

UI

↓

Business Logic

↓

External Services

↓

Database

Business logic should never live inside UI components.

API integrations should be isolated.

Prediction logic should remain modular.

---

# Component Rules

Create components only when:

- reused multiple times
- improves readability
- represents a meaningful UI section

Avoid wrapper components with no value.

---

# State Management

Prefer:

Local State

↓

React Context (only if needed)

↓

Global State

Do not introduce global state unnecessarily.

---

# API Rules

All external APIs must be isolated.

Never call external APIs directly inside components.

Wrap integrations inside reusable service modules.

Always handle:

- loading
- empty state
- timeout
- failure

---

# Error Handling

Never silently ignore errors.

Display meaningful messages.

Log unexpected failures.

Fail gracefully.

---

# Forms

Every form must include:

- validation
- loading state
- disabled submit while processing
- success feedback
- error feedback

---

# UI Rules

Prioritize clarity over decoration.

Every screen should answer one question quickly.

Avoid visual clutter.

Use consistent spacing.

Use consistent typography.

Use consistent colors.

---

# Accessibility

Always include:

- labels
- keyboard navigation
- focus states
- accessible buttons

---

# Responsive Design

Every page must work on:

Desktop

Tablet

Mobile

Never build desktop-only layouts.

---

# Performance

Prefer:

Server Components where appropriate.

Lazy loading for heavy sections.

Optimized images.

Avoid unnecessary renders.

Do not optimize prematurely.

---

# TypeScript Rules

Strict typing.

Never use:

any

unless absolutely unavoidable.

Prefer explicit interfaces.

Keep types readable.

---

# Database Rules

Database changes must follow:

Schema

↓

Migration

↓

Validation

↓

API

↓

UI

Never build UI around undefined data.

---

# AI Rules

Gemini is used only for:

- recommendations
- summaries
- explainability

Do not use AI for deterministic calculations.

Prediction logic must remain transparent.

---

# External Libraries

Do not install new packages unless:

- there is a clear need
- built-in tools are insufficient

Always explain why a dependency is required.

Prefer lightweight libraries.

---

# Git Rules

One feature

↓

One commit

Commit messages should describe the feature clearly.

Example:

feat: add officer risk queue

fix: weather API timeout handling

refactor: split prediction service

---

# Code Style

Write code that another engineer can understand quickly.

Prefer:

small functions

descriptive names

early returns

simple logic

Avoid:

deep nesting

clever tricks

over-abstraction

---

# Verification Checklist

Before marking a feature complete:

✅ Builds successfully

✅ No TypeScript errors

✅ No console errors

✅ Existing features still work

✅ Responsive layout

✅ Handles loading

✅ Handles empty state

✅ Handles errors

Only after passing every check is the feature complete.

---

# When Blocked

If information is missing:

Do not guess.

Explain what is missing.

Recommend the smallest path forward.

---

# Definition of Done

A feature is complete only when:

- Works correctly
- Fits the MVP
- Matches AGENTS.md
- Does not break existing functionality
- Passes verification

Then move to the next feature.

---

# Final Principle

Every implementation should make the demo stronger.

If a feature does not improve the demo or MVP,
it probably should not be built during the hackathon.