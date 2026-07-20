# 🌾 Fasal Becho

> **AI-Powered Cash Flow Prediction & Risk Intelligence Platform for Rural Micro Enterprises**

<p align="center">

**Predict Tomorrow. Intervene Today.**

</p>

---

## 🚀 Overview

Fasal Becho is an AI-assisted decision support platform built for the **NABARD Hackathon**. It helps NABARD field officers proactively identify rural micro-enterprises that are likely to face cash-flow stress before financial distress becomes severe.

Instead of replacing human judgement, Fasal Becho provides explainable predictions, prioritized risk queues, and AI-generated recommendations that support faster, data-driven interventions.

---

## ❗ Problem

Rural micro-enterprises often lack continuous financial monitoring.

Field officers typically react **after** businesses experience financial stress.

Challenges include:

- Manual monitoring
- Seasonal income fluctuations
- Commodity price volatility
- Weather uncertainty
- Large number of enterprises per officer

---

## 💡 Solution

Fasal Becho combines:

- 📊 Financial records
- 🌦 Weather signals
- 🌾 Commodity prices
- 🤖 Explainable AI

to generate:

- Triple cash-flow forecasts
- Working capital alerts
- Risk prioritization
- Why Cards
- AI recommendations

---

## ✨ Key Features

| Enterprise | Officer | Intelligence |
|------------|----------|--------------|
| Business Profile | Risk Queue | Triple Forecast |
| Financial Records | Dashboard | Working Capital Alert |
| Income & Expenses | Enterprise Review | Explainable Why Card |
| Loans & Savings | Officer Actions | AI Recommendations |
| Offline Support | Search & Filters | Scenario Simulation |

---

## 🔄 Prediction Pipeline

```text
Financial Records
        │
        ▼
Seasonality
        │
        ▼
Commodity Prices
        │
        ▼
Weather Adjustment
        │
        ▼
Scenario Generation
        │
        ▼
Risk Score
        │
        ▼
Why Card
        │
        ▼
AI Recommendation
```

---

## 🏗 Architecture

```text
Enterprise Portal
          │
Officer Dashboard
          │
          ▼
 Next.js Application
          │
 Business Layer
          │
 Prediction Engine
          │
 ├── Weather
 ├── Commodity Prices
 ├── AI Recommendations
 └── PostgreSQL
```

---

## 🛠 Tech Stack

| Layer | Technology |
|------|------------|
| Frontend | Next.js + React + TypeScript |
| UI | Tailwind CSS + shadcn/ui |
| Backend | Next.js Route Handlers |
| ORM | Prisma |
| Database | PostgreSQL |
| AI | Gemini |
| Charts | Recharts |
| Offline | PWA + IndexedDB |
| Deployment | Vercel |

---

## 🌐 External Data Sources

- Agmarknet (UPAg)
- Meteostat
- Synthetic Financial Dataset
- Gemini

---

## 📁 Project Structure

```text
app/
components/
services/
business/
prisma/
public/
docs/
```

---

## 🎯 Demo Flow

1. Create an enterprise
2. Add monthly financial records
3. Generate prediction
4. View risk score
5. Read Why Card
6. View AI recommendations
7. Open Officer Dashboard
8. Record officer intervention

---

## 🚀 Local Setup

```bash
git clone <repo>

cd app

npm install

cp .env.example .env

npm run dev
```

---

## 🛣 Roadmap

- Multi-sector support
- Portfolio analytics
- Heatmaps
- Notifications
- Mobile application
- Account Aggregator integration

---

## 👥 Team

Built for the **NABARD Hackathon 2026**.

---

## 📄 License

MIT License
