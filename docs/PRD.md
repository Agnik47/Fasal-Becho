# Product Requirements Document (PRD)

# Fasal Becho
### AI-Powered Cash Flow Prediction & Risk Intelligence Platform

> Version: 1.0
> Status: MVP
> Project: NABARD Hackathon 2026

---

# 1. Overview

Fasal Becho is an AI-assisted decision support platform designed to help NABARD field officers identify rural micro-enterprises that may experience future cash-flow stress.

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

Fasal Becho IS:

- A decision-support platform
- A risk prioritization system
- A cash-flow scenario simulator
- A human-in-the-loop intelligence layer

Every prediction is advisory.

Human officers always make the final decision.

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

Primary:

- Synthetic Financial Dataset
- UPAg Agmarknet
- Meteostat

Secondary sources are optional and only used if time permits.

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

The following features will NOT be implemented:

- Loan approval
- Credit underwriting
- Payment gateway
- Real Account Aggregator integration
- Multilingual interface
- Heat maps
- Portfolio analytics
- Multiple business sectors
- Advanced forecasting models
- Notification system

These are future roadmap items.

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