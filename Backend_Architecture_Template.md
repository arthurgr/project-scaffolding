# [Project Name] — Backend Architecture & API Design

**Version:** 1.0
**Date:** [YYYY-MM-DD]
**Owner:** [Team / Person]
**Status:** Draft | In Review | Approved

| Field | Value |
|---|---|
| Project | [Project Name] |
| Owner | [Team / Person] |
| Status | Draft / In Review / Approved |
| Last Updated | [YYYY-MM-DD] |
| Primary Goal | [One sentence: what the backend must achieve for the user] |

---

## 1. Project Setup & Stack

### 1.1 Framework & Language

| Setting | Value | Notes |
|---|---|---|
| Framework | [e.g. Spring Boot 4.x] | |
| Language | [e.g. Kotlin / Java 21] | |
| Build Tool | [e.g. Gradle / Maven] | |
| Runtime | [e.g. JVM 21 / Node 22] | |

### 1.2 Core Dependencies

| Dependency | Purpose | Version |
|---|---|---|
| [library] | [e.g. REST API] | |
| [library] | [e.g. ORM / Persistence] | |
| [library] | [e.g. Auth] | |
| [library] | [e.g. Validation] | |
| [library] | [e.g. DB Migrations] | |
| [library] | [e.g. Rate Limiting] | |

---

## 2. Environment & Infrastructure

### 2.1 Database

| Setting | Value | Notes |
|---|---|---|
| Database | [e.g. PostgreSQL 16] | |
| Local Setup | [e.g. Docker / Podman Compose] | |
| Migration Tool | [e.g. Liquibase / Flyway] | |
| Migration Format | [e.g. SQL changelogs / Java] | |

### 2.2 Environment Variables

Store all secrets in a `.env` file (excluded from version control). Required variables:

- `[VAR_NAME]` — [description]
- `[VAR_NAME]` — [description]
- `[VAR_NAME]` — [description]

### 2.3 Local Development

- Container orchestration: [Docker Compose / Podman — file location]
- Start command: [e.g. `./gradlew bootRun` or `docker compose up`]
- `.gitignore` must exclude: `.env`, `build/`, `target/`, `*.class`

---

## 3. Core Principles

Non-negotiable architectural decisions that all contributors must follow.

- **[Principle 1]** — e.g. All financial calculations computed in the service layer, never stored as columns
- **[Principle 2]** — e.g. DTO-first API: clients always receive computed fields in responses
- **[Principle 3]** — e.g. Multi-tenant authorization enforced at API and DB constraint level
- **[Principle 4]** — e.g. Deterministic unit conversion via defined constants
- **[Add more as needed]**

---

## 4. Security & Authentication

### 4.1 Authentication Provider

[Describe the auth provider, e.g. Firebase, Auth0, Cognito — JWT-based]

- Verify [provider] ID token on every request (except health/public endpoints)
- Extract from JWT: `uid` / `sub`, `email`, `display_name` (note which are optional)

### 4.2 User Bootstrap Flow

1. Receive authenticated request
2. Look up internal user record by provider UID
3. If missing: create user record with new internal UUID
4. Optionally update stored profile fields if they changed

### 4.3 Multi-Tenant Authorization

Tenant is resolved from: [Header / Path param — choose one and document it]

- Every domain request must be scoped to a `tenant_id`
- Backend must verify: user is a member of the requested tenant
- Role-based access for restricted operations: [list roles, e.g. owner / member / viewer]

### 4.4 Security Controls

| Control | Implementation | Config |
|---|---|---|
| Rate Limiting | [e.g. Bucket4j — per UID / per IP] | [e.g. 100 req/min] |
| Security Headers | X-Content-Type-Options, X-Frame-Options, HSTS | Set in SecurityConfig |
| Input Validation | Bean Validation (`@Valid`, `@NotBlank`, etc.) | 400 on failure |
| Error Handling | Global exception handler — no stack traces in prod | `server.error.*=never` |
| Dependency Scanning | [e.g. OWASP dependency-check] | Run in CI |

---

## 5. High-Level Architecture

### 5.1 Layer Responsibilities

| Layer | Responsibilities |
|---|---|
| Controllers (HTTP) | Parse & validate input · Enforce auth · Delegate to service · Return DTOs |
| Services (Business Logic) | CRUD · Calculations · Authorization decisions · Unit conversion |
| Repositories (Data Access) | Tenant-scoped queries · Composite key usage · Prevent cross-tenant leaks |
| Calculation Engine | Pure functions · Stateless · Unit-testable independently |

### 5.2 Key Design Patterns

- **[Pattern]** — [rationale] — e.g. Repository pattern for all DB access
- **[Pattern]** — [rationale] — e.g. Service layer owns all business logic
- **[Pattern]** — [rationale] — e.g. Pure calculation engine (no side effects)

---

## 6. Domain Model

### 6.1 Entities

Reference canonical schema: [link to database-schema.md or equivalent]

| Entity | Purpose | Tenant-Scoped? |
|---|---|---|
| [entity_name] | [brief description] | Yes / No |
| [entity_name] | [brief description] | Yes / No |
| [entity_name] | [brief description] | Yes / No |
| [entity_name] | [brief description] | Yes / No |

### 6.2 Relationships

[Add ERD reference or key relationship notes here]

---

## 7. Calculation Engine

Implement as a dedicated, side-effect-free module. All functions must be independently unit-testable.

### 7.1 Unit Conversion

Canonical unit: [e.g. grams for weight]. All conversion constants must be defined once and referenced everywhere.

| Input Unit | Conversion to Canonical |
|---|---|
| [unit] | [formula or constant] |
| [unit] | [formula or constant] |
| [unit] | [formula or constant] |

### 7.2 Calculation Steps

Document each calculation in order. Name variables consistently between code and docs.

#### Step 1: [Name]

- **Input:** [list inputs]
- **Formula:** `[formula]`
- **Output:** `[variable_name]`

#### Step 2: [Name]

- **Input:** [list inputs]
- **Formula:** `[formula]`
- **Output:** `[variable_name]`

#### Step N: [Add as needed]

---

## 8. API Surface

### 8.1 Conventions

- Base path: [e.g. `/api/v1`]
- Auth: [e.g. Bearer token in `Authorization` header]
- Tenant scoping: [e.g. `X-Tenant-Id` header or `/tenants/{id}/...` path]
- Response format: JSON
- Error format: `{ code, message, fieldErrors[] }`
- Pagination: [e.g. `?page=0&size=20` — define here if used]

### 8.2 Endpoints

#### System

| Method | Path | Description |
|---|---|---|
| GET | `/health` | Health check — no auth required |
| GET | `/me` | Auth bootstrap — returns user + memberships |

#### [Resource Group 1]

| Method | Path | Description |
|---|---|---|
| POST | `/[resource]` | Create |
| GET | `/[resource]` | List (with optional `?query=` filters) |
| GET | `/[resource]/{id}` | Get by ID |
| PATCH | `/[resource]/{id}` | Update |
| DELETE | `/[resource]/{id}` | Delete (soft or hard — specify which) |

#### [Resource Group 2 — add as needed]

[Add endpoint table for each resource group]

---

## 9. DTO Shapes

### 9.1 Request DTOs

#### [EntityName]CreateRequest

- `field` — [type] — [validation rules, e.g. required, min/max]
- `field` — [type] — [validation rules]

### 9.2 Response DTOs

#### [EntityName]DTO

- Stored fields: `id`, `[field1]`, `[field2]`, ...
- Computed fields: `[computed1]` (display-only, not persisted)

#### [EntityName]ComputedDTO (for enriched views)

- `[computed_field_1]`
- `[computed_field_2]`
- `[breakdown_array][]`

---

## 10. Error Handling & Validation

### 10.1 Validation Rules

- Reject negative values for [money / quantity / etc.]
- Reject zero [yield / quantity / etc.]
- Reject unsupported [units / types / etc.]
- [Add domain-specific rules]

### 10.2 HTTP Status Map

| Scenario | HTTP Status |
|---|---|
| Validation failure | 400 Bad Request |
| Auth required | 401 Unauthorized |
| Access denied / wrong tenant | 403 Forbidden |
| Resource not found (or cross-tenant) | 404 Not Found |
| Rate limit exceeded | 429 Too Many Requests |
| Server error | 500 Internal Server Error |

### 10.3 Error Envelope

All errors return a consistent JSON shape:

```json
{
  "code": "VALIDATION_ERROR",
  "message": "Request validation failed",
  "fieldErrors": [
    { "field": "name", "message": "must not be blank" }
  ]
}
```

---

## 11. Testing Strategy

| Test Type | What to Cover |
|---|---|
| Unit tests (pure functions) | Unit conversion · Calculation math · Edge cases (zero, negatives) |
| Service tests | Tenant enforcement · CRUD correctness · Computed result accuracy |
| Integration tests | Auth middleware with mocked tokens · End-to-end flows · Error scenarios |

---

## 12. Observability

- Request logging: include `tenant_id` + `user_id` (safe metadata only — no PII in logs)
- Metrics: request latency, error rates, [domain-specific metrics]
- Structured logs for: calculation failures, conversion errors, auth rejections
- [Link to monitoring dashboard or alerting setup]

---

## 13. Scope Definition

### 13.1 In Scope (Build Now)

- [Feature or capability]
- [Feature or capability]
- [Feature or capability]

### 13.2 Out of Scope (Do Not Build Yet)

- [Feature] — [brief reason or phase it belongs to]
- [Feature] — [brief reason or phase it belongs to]
- [Feature] — [brief reason or phase it belongs to]

---

## 14. Open Questions & Decisions

Track unresolved design decisions here. Move to ADR log once resolved.

| Question | Options / Notes | Owner |
|---|---|---|
| [Decision needed] | [Possible approaches] | [Name] |
| [Decision needed] | [Possible approaches] | [Name] |
| [Decision needed] | [Possible approaches] | [Name] |

---

## 15. Change Log

| Date | Author | Summary |
|---|---|---|
| [YYYY-MM-DD] | [Name] | Initial draft |
