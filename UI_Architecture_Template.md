# [Project Name] — Frontend Architecture & UI Design

**Version:** 1.0
**Date:** [YYYY-MM-DD]
**Owner:** [Team / Person]
**Status:** Draft

| Field | Value |
|---|---|
| Project | [Project Name] |
| Owner | [Team / Person] |
| Status | Draft / In Review / Approved |
| Last Updated | [YYYY-MM-DD] |
| Primary Goal | [One sentence: what the UI must achieve for the user] |

---

## 1. Tech Stack

### 1.1 Core Framework

| Setting | Value | Notes |
|---|---|---|
| Framework | [e.g. Vite + React + TypeScript] | |
| Language | [e.g. TypeScript] | |
| Package Manager | [e.g. npm / pnpm / yarn] | |
| Node Version | [e.g. 20.x] | |

### 1.2 Key Libraries

| Library | Purpose | Version |
|---|---|---|
| [e.g. TanStack Query] | Server state / data fetching | |
| [e.g. Shadcn UI / Radix UI] | Design system / component primitives | |
| [e.g. Tailwind CSS] | Utility-first styling | |
| [e.g. React Router v6] | Client-side routing | |
| [e.g. Axios / Fetch] | HTTP client | |
| [e.g. Firebase Auth] | Authentication | |
| [e.g. Zod / Yup] | Schema validation | |
| [e.g. cmdk] | Command menus / searchable selects | |
| [e.g. React Hook Form] | Form state management | |

### 1.3 Testing

| Tool | Purpose |
|---|---|
| [e.g. Vitest] | Unit and component tests |
| [e.g. React Testing Library] | Component interaction tests |
| [e.g. Playwright] | End-to-end tests |

### 1.4 Code Quality

| Tool | Purpose | Config |
|---|---|---|
| [e.g. ESLint] | Linting — TypeScript + React rules | `npm run lint` |
| [e.g. Prettier] | Code formatting | `npm run format` / `npm run format:check` |
| [e.g. Husky + lint-staged] | Pre-commit hooks — format + lint staged files | Runs on every commit |

---

## 2. Design Principles

List the non-negotiable UX decisions that guide every screen and interaction.

- **[Principle 1]** — e.g. Product-first onboarding: users reach core value without upfront setup
- **[Principle 2]** — e.g. Fast time-to-value: key output visible within N minutes of first use
- **[Principle 3]** — e.g. Minimal navigation: keep top-level areas small and purposeful
- **[Principle 4]** — e.g. Progressive disclosure: hide advanced concepts until relevant
- **[Principle 5]** — e.g. Service-computed outputs: UI displays values returned by API, not client-calculated
- **[Add more as needed]**

---

## 3. Accessibility Standards

- [ ] Keyboard navigation (Tab, Enter, Escape) on all interactive elements
- [ ] Screen reader friendly labels and ARIA attributes
- [ ] Clear focus indicators
- [ ] Semantic HTML structure
- [ ] Searchable dropdowns with keyboard support (Arrow keys, Enter, Escape)
- [ ] Sufficient color contrast (WCAG AA minimum)
- [ ] [Add project-specific requirements]

---

## 4. Global App Shell

### 4.1 Navigation Structure

Top-level navigation areas (keep minimal):

1. **[Area 1]** — [purpose] — default landing
2. **[Area 2]** — [purpose]
3. **[Area 3]** — [purpose]
4. **[Area 4 — e.g. Settings]** — [purpose]

### 4.2 Header Utilities

- [e.g. Tenant / workspace switcher]
- [e.g. Account menu: profile, logout]
- [e.g. Contextual "Create" button]

### 4.3 Auth Shell

- Public routes: [list — e.g. `/login`, `/signup`]
- Protected routes: all others — redirect to login if unauthenticated
- Auth provider: [e.g. Firebase Auth — token stored in context, attached to all API requests via Axios interceptor]

---

## 5. Multi-Tenant / Workspace UX

### 5.1 Tenant Switcher

- Displays current workspace name in header
- Dropdown lists all workspaces the user belongs to
- "[Create new workspace]" entry at bottom of list

### 5.2 Tenant Context

- Active `tenant_id` stored in [e.g. React context / URL / header]
- All API calls include tenant identifier: [e.g. `X-Tenant-Id` header]
- Switching tenant resets relevant query cache

---

## 6. Pages & Screens

### 6.1 Page Inventory

| Page | Route | Purpose | Auth Required? |
|---|---|---|---|
| [Page Name] | `/[route]` | [brief description] | Yes / No |
| [Page Name] | `/[route]` | [brief description] | Yes / No |
| [Page Name] | `/[route]` | [brief description] | Yes / No |

### 6.2 [Primary Resource] — List Page

**Purpose:** [brief]

- Primary CTA: **[Action]**
- Search: [fields searched, trigger behavior — e.g. on Enter or on type]
- Sort: [sortable columns, default sort]
- Pagination: [e.g. `?page=0&size=20` — infinite scroll or page controls]
- Card / row shows: [list key fields displayed]
- Quick actions: [e.g. View / Edit / Duplicate / Archive]
- Empty state: [headline + helper text + CTA]

### 6.3 [Primary Resource] — Create / Edit Page

**Purpose:** [brief — this is typically the hero screen]

**Section A: [Name]**
- [field] — [type, required/optional, validation]
- [field] — [type, required/optional, validation]

**Section B: [Name]**
- [field] — [type, required/optional, validation]
- [Computed / read-only outputs from API]

**Section C: [Add sections as needed]**

Actions: **Save** · **Save & Add Another** · **Cancel** · [others]

### 6.4 [Secondary Resource] — List Page

[Repeat pattern from 6.2]

### 6.5 [Secondary Resource] — Create / Edit Page

[Repeat pattern from 6.3]

### 6.6 Settings

- **[Tab 1: e.g. Account]** — [fields]
- **[Tab 2: e.g. Team]** — [fields: invite, role assignment, member list]
- **[Tab 3: e.g. Workspace]** — [fields]

---

## 7. Shared Components & Patterns

### 7.1 Forms

- Validation library: [e.g. Zod + React Hook Form]
- Validation trigger: on blur + on submit
- Required field indicator: [e.g. red asterisk `*`]
- Error display: [e.g. red border + message below field]
- Precision rules: [e.g. money fields max 2 decimal places, quantities up to 4]

### 7.2 Search

- Trigger: [e.g. Enter key or Search button — not on every keystroke]
- Clear: X button to reset
- Visual feedback: "[Filtering by: X]" indicator when active
- Caching: [e.g. TanStack Query caches results for instant re-display]

### 7.3 Sorting

- Implementation: server-side — sends `?sort=field,direction` to API
- UI: clickable column headers
- Indicators: inactive columns show neutral ⇅ at 50% opacity; active column shows ↑ or ↓ at full opacity
- Toggle: click active column header to reverse direction

### 7.4 Inline / Modal Creation

- Pattern: [e.g. if a required related entity doesn't exist, user can create it inline without leaving the current form]
- Modal fields: [list fields shown in the modal]
- After save: [e.g. return to parent form with new entity pre-selected]

### 7.5 Computed / Read-Only Display

- All computed values come from API responses — never calculated client-side
- Display rules:
  - Money: 2 decimal places
  - Percentages: 1–2 decimal places
  - Quantities: up to 4 decimal places where needed
- Placeholder when no data: [e.g. "Add items to see costs"]

### 7.6 Empty States

Each list page has a designed empty state:
- Headline: [e.g. "Create your first [resource]"]
- Helper text: [brief explanation of value]
- CTA: primary action button

---

## 8. State Management

### 8.1 Server State

- Tool: [e.g. TanStack Query (React Query)]
- Query keys: [e.g. `['cost-items', tenantId]`, `['product', tenantId, productId]`]
- Invalidation strategy: [e.g. on mutation success, invalidate related query keys]
- Stale time: [e.g. 30s for lists, 0 for computed endpoints]

### 8.2 Client / UI State

- Tool: [e.g. React `useState` / `useReducer` / Zustand / Context]
- Global state: [e.g. active tenant, auth user, theme]
- Local state: [e.g. form values, modal open/close, selected tab]

### 8.3 Auth State

- Provider: [e.g. Firebase Auth — `onAuthStateChanged` listener]
- Storage: [e.g. Firebase manages token; stored in memory / context]
- Token attachment: [e.g. Axios request interceptor adds `Authorization: Bearer <token>`]

---

## 9. API Integration

### 9.1 HTTP Client Setup

- Client: [e.g. Axios instance with base URL from env]
- Base URL env var: `[e.g. VITE_API_BASE_URL]`
- Request interceptor: attach Firebase auth token
- Response interceptor: [e.g. redirect to login on 401, surface error toasts on 4xx/5xx]

### 9.2 Tenant Header

- All authenticated requests include: `X-Tenant-Id: [activeTeantId]` (or equivalent)
- Active tenant sourced from: [e.g. React context]

### 9.3 Error Handling

| HTTP Status | UI Behavior |
|---|---|
| 400 | Show field-level validation errors from `fieldErrors[]` |
| 401 | Redirect to login |
| 403 | Show "Access denied" message |
| 404 | Show "Not found" state |
| 429 | Show "Too many requests — try again shortly" toast |
| 500 | Show generic error toast; log to console |

---

## 10. Routing

### 10.1 Route Structure

```
/                        → redirect to /products
/login                   → Login page (public)
/[primary-resource]      → List page
/[primary-resource]/new  → Create page
/[primary-resource]/:id  → Edit / detail page
/[secondary-resource]    → List page
/[secondary-resource]/new
/[secondary-resource]/:id
/settings                → Settings (tabs: account, team, workspace)
```

### 10.2 Route Guards

- Unauthenticated users → redirect to `/login`
- Post-login redirect → return to originally requested route
- No-tenant state → redirect to tenant creation / onboarding

---

## 11. Onboarding Flow

Goal: [e.g. user reaches first core output within 5–10 minutes]

1. User signs in ([auth provider])
2. If first time: [e.g. create workspace automatically → redirect to primary resource empty state]
3. [Step 3]
4. [Step 4]
5. [Step 5 — first "wow" moment: user sees computed output]
6. Optional prompt: [e.g. "Want to add X to see Y?"]

---

## 12. Data Display Rules

- All computed values come from API — UI does not compute independently
- Money: 2 decimal places (e.g. `$12.50`)
- Percentages: 1–2 decimal places (e.g. `34.5%`)
- Quantities: up to 4 decimal places where relevant
- Graceful handling: missing optional market fields default to 0, not errors

---

## 13. Form Validation Rules

| Field Type | Rules |
|---|---|
| Money / cost | Numeric, ≥ 0, max 2 decimal places |
| Quantity | Numeric, > 0, max 4 decimal places |
| Percentage | Numeric, ≥ 0 and < 1 (stored) or 0–100% (display) |
| Text / name | Required, not blank, max [N] chars |
| Select / enum | Must match allowed values |
| UUID reference | Must be a valid existing entity ID |

---

## 14. MVP Scope

### 14.1 Must-Have Pages / Features

- [Feature or page]
- [Feature or page]
- [Feature or page]

### 14.2 Nice-to-Have (Later)

- [Feature] — [phase or reason deferred]
- [Feature] — [phase or reason deferred]
- [Feature] — [phase or reason deferred]

---

## 15. Edge Cases to Handle

- [Edge case] — [how to handle]
- [Edge case] — [how to handle]
- [Edge case] — [how to handle]

---

## 16. Open Questions & Decisions

| Question | Options / Notes | Owner |
|---|---|---|
| [Decision needed] | [Possible approaches] | [Name] |
| [Decision needed] | [Possible approaches] | [Name] |

---

## 17. Change Log

| Date | Author | Summary |
|---|---|---|
| [YYYY-MM-DD] | [Name] | Initial draft |
