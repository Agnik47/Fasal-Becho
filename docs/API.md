# API.md

# Fasal Becho API Specification

Version: 1.0

Status: MVP API Contract

This document defines the API specification for the Fasal Becho application.

It serves as the contract between:

- Frontend
- Backend
- Database
- External Services

All APIs implemented in the application must follow the standards defined here.

This document intentionally avoids implementation details.

---

# API Design Philosophy

The API should be:

- Predictable
- RESTful
- Consistent
- Versionable
- Secure
- Easy to consume
- Easy to extend

Every endpoint should have a single responsibility.

Never create endpoints that perform multiple unrelated actions.

---

# Base URL

Development

```
/api
```

Future

```
/api/v1
```

All APIs should be version-ready.

---

# Authentication

Every protected endpoint requires authentication.

Public APIs should be explicitly marked.

Authentication responsibilities include:

- User identity
- Session validation
- Role authorization

The frontend should never trust itself.

The server is always the source of truth.

---

# User Roles

The application supports two roles.

## Enterprise

Permissions

- View own profile
- Update own profile
- Manage financial records
- View predictions
- View recommendations

Cannot:

- View other enterprises
- View officer dashboard
- Modify officer actions

---

## Officer

Permissions

- View all enterprises
- View risk queue
- View predictions
- Record officer actions
- Override alerts

Cannot:

- Modify enterprise ownership
- Modify prediction history

---

# API Response Standard

Every endpoint should return the same response structure.

Success

```json
{
  "success": true,
  "message": "Operation completed successfully.",
  "data": {}
}
```

Failure

```json
{
  "success": false,
  "message": "Validation failed.",
  "errors": []
}
```

Never return inconsistent response formats.

---

# HTTP Status Codes

200

Success

---

201

Resource Created

---

400

Bad Request

---

401

Unauthorized

---

403

Forbidden

---

404

Resource Not Found

---

409

Conflict

---

422

Validation Failed

---

500

Internal Server Error

---

# Validation

Every endpoint should validate:

- Required fields
- Data types
- Enum values
- Numeric ranges
- User permissions

Never trust client input.

Validation occurs before business logic.

---

# API Modules

The application exposes APIs grouped by responsibility.

Enterprise APIs

↓

Financial APIs

↓

Prediction APIs

↓

Recommendation APIs

↓

Officer APIs

↓

System APIs

---

# Enterprise APIs

Purpose

Manage enterprise information.

---

## Create Enterprise

POST

```
/api/enterprises
```

Creates a new enterprise.

---

## Get Enterprise

GET

```
/api/enterprises/:id
```

Returns enterprise details.

---

## Update Enterprise

PUT

```
/api/enterprises/:id
```

Updates enterprise information.

---

## Delete Enterprise

DELETE

```
/api/enterprises/:id
```

Soft delete only.

Historical data must remain intact.

---

## List Enterprises

GET

```
/api/enterprises
```

Supports:

- pagination
- search
- filtering

---

# Financial APIs

Purpose

Manage financial records.

---

## Add Financial Record

POST

```
/api/financial-records
```

Creates a monthly financial record.

---

## Get Financial Records

GET

```
/api/financial-records
```

Supports:

- enterprise
- month
- year

---

## Update Financial Record

PUT

```
/api/financial-records/:id
```

---

## Delete Financial Record

DELETE

```
/api/financial-records/:id
```

---

# Prediction APIs

Purpose

Generate and retrieve predictions.

Prediction generation is deterministic.

---

## Generate Prediction

POST

```
/api/predictions/generate
```

Input

- Enterprise ID

Output

- Triple Forecast
- Working Capital Forecast
- Risk Score
- Risk Level

---

## Get Prediction

GET

```
/api/predictions/:id
```

Returns a prediction snapshot.

---

## Prediction History

GET

```
/api/predictions/history/:enterpriseId
```

Returns historical predictions.

---

# Recommendation APIs

Purpose

Generate AI explanations.

AI never calculates risk.

---

## Generate Recommendation

POST

```
/api/recommendations/generate
```

Consumes:

- Prediction

Returns:

- Why Card
- Recommendations
- Summary

---

## Get Recommendation

GET

```
/api/recommendations/:predictionId
```

---

# Officer APIs

Purpose

Support officer workflow.

---

## Risk Queue

GET

```
/api/officer/risk-queue
```

Returns prioritized enterprises.

Supports:

- filtering
- sorting
- pagination

---

## Enterprise Review

GET

```
/api/officer/enterprise/:id
```

Returns:

- Enterprise
- Prediction
- Recommendation
- Officer History

---

## Record Officer Action

POST

```
/api/officer/actions
```

Creates intervention history.

---

## Officer History

GET

```
/api/officer/actions/:enterpriseId
```

Returns intervention timeline.

---

# Dashboard APIs

Purpose

Dashboard statistics.

---

## Dashboard Summary

GET

```
/api/dashboard
```

Returns:

- Total Enterprises
- High Risk
- Medium Risk
- Low Risk
- Recent Predictions

---

# System APIs

Purpose

Health and diagnostics.

---

## Health Check

GET

```
/api/health
```

Returns application health.

---

## Version

GET

```
/api/version
```

Returns application version.

---

# External Service APIs

External services are never exposed directly.

All integrations must go through internal service layers.

Supported services:

- UPAg Agmarknet
- Meteostat
- Gemini

Client components must never call external APIs directly.

---

# Pagination Standard

Collection endpoints support:

```
?page=1
&limit=20
```

Future support

```
sort
order
filter
```

---

# Search Standard

Search should support:

- Enterprise Name
- Owner Name
- District

Search should be case-insensitive.

---

# Filtering

Endpoints may support:

- District
- Business Type
- Risk Level
- Date Range

---

# Sorting

Supported fields:

- Created Date
- Updated Date
- Risk Score
- Enterprise Name

Ascending

Descending

---

# Error Handling

Every endpoint should return meaningful errors.

Never expose:

- stack traces
- SQL errors
- API secrets

Unexpected errors should be logged internally.

---

# Rate Limiting

Future enhancement.

Not required for MVP.

---

# Caching

Cache only:

- Weather
- Commodity Prices
- Prediction Results

Never cache:

- Authentication
- Officer Actions
- Financial Updates

---

# Security Rules

Never expose:

- API Keys
- Environment Variables
- Database Credentials

Server validates every request.

The client never has direct database access.

---

# API Design Principles

Every endpoint should:

- Have one responsibility
- Be predictable
- Validate input
- Return consistent responses
- Use proper HTTP methods
- Respect user roles
- Be easy to document
- Be easy to test

---

# API Lifecycle

Request

↓

Authentication

↓

Validation

↓

Business Logic

↓

Database

↓

Response Formatting

↓

Client

Every endpoint follows this lifecycle.

---

# Definition of Done

An API is complete when it:

✓ Validates input

✓ Enforces permissions

✓ Returns consistent responses

✓ Handles errors

✓ Follows naming conventions

✓ Matches the database model

✓ Matches business rules

✓ Matches the PRD

---

# Guiding Principle

The API exists to expose business capabilities—not database tables.

Every endpoint should represent a meaningful business action and remain independent of implementation details.