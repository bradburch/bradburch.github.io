# Portfolio Site Redesign — Design Spec

**Date:** 2026-04-05  
**Status:** Approved  
**Target roles:** Software Engineer (primary), Solutions Engineer (primary)

---

## Overview

Full redesign of `bradburch.github.io` — a single-page Jekyll/GitHub Pages portfolio. Replace the current generic purple-gradient design with a clean, technical aesthetic using steel blue and monospace accents. Update all copy to reflect the depth of the Brad Paws project using bullets from `RESUME_BULLETS.md`.

---

## Design System

### Color Palette

| Role | Value |
|------|-------|
| Background | `#ffffff` |
| Body text | `#374151` |
| Headings / name | `#111827` |
| Muted / meta | `#9ca3af` |
| Section dividers | `#f3f4f6` |
| Accent (primary) | `#1e40af` (steel blue) |
| Accent (bullets) | `#3b82f6` |
| Accent (badges) | `#1e40af` |
| Stack tag bg | `#eff6ff` |
| Stack tag border | `#bfdbfe` |
| Stack tag text | `#1e3a8a` |
| Story panel bg | `#eff6ff` |
| Story panel border | `#1d4ed8` |

### Typography

- **Body font:** `system-ui, -apple-system, sans-serif`
- **Monospace font:** `'SF Mono', ui-monospace, monospace` — used on stack tags, contact links, and the "Read the full story" button
- **Name:** `font-weight: 900`, `letter-spacing: -0.03em`, all black (no color treatment)
- **Subsystem labels:** `font-size: 0.6em`, uppercase, `letter-spacing: 0.1em`, steel blue, `border-left: 3px solid #1d4ed8`, `padding-left: 7px`

### Header

- Top border: `4px solid #1e40af`
- No background color — white
- Name: `Brad Burch` — all `#111827`, no color on surname
- Subtitle: `Software Engineer · Solutions Engineer` — uppercase, muted
- One-line summary: "Full-stack engineer. Production systems, not demos. Most recently: sole owner of a client portal and AI booking platform — auth, AI agents, MCP server, invoicing, and a sync pipeline, all from scratch on Cloudflare's edge."
- Links: `Email`, `LinkedIn`, `GitHub`, `Resume (PDF)` — monospace, steel blue bg pill style

---

## Page Structure

Single column, `max-width: 800px`, centered. All sections separated by `1px solid #f3f4f6` dividers.

```
Header
  └─ Name, subtitle, one-line summary, links

─── divider ───

Section label: EXPERIENCE

Brad Paws block
  └─ Job title + "Sole Engineer" badge + meta
  └─ Stack tags (monospace)
  └─ Subsystem groups (each with border-left label + bullets):
       1. Full-Stack Architecture & Infrastructure
       2. Authentication & Security
       3. AI Chat Agent
       4. MCP Booking Server
       5. Google Calendar Integration
       6. Booking, Invoicing & Pipeline
       7. Testing & CI/CD
  └─ "↗ Read the full story" button (inline expand)
       └─ Story panel: Why it exists / Architecture decision / What made it hard / On the test count

─── divider ───

Section label: PREVIOUS EXPERIENCE

Front.com (2022 · Impacted by RIF) — 3 bullets
Total Brain (2019–2021 · Impacted by RIF) — 4 bullets
Salesforce (2017–2018) — 3 bullets

─── divider ───

Section label: TECHNICAL STACK

Grid of labeled monospace tags (10 items):
  TypeScript/JS · CF Workers · D1/KV/R2 · Hono/React 19
  Claude/MCP · JWT/OAuth2+PKCE · Vitest · GH Actions
  Prior: Java/Scala/Go/Python · Prior: PostgreSQL/DynamoDB/Kafka

─── divider ───

Section label: GET IN TOUCH

Contact block: summary copy + monospace links (email, LinkedIn, GitHub, Resume PDF)

Footer: © 2026 Brad Burch
```

---

## Brad Paws — Full Bullet Content

All bullets sourced from `RESUME_BULLETS.md` Software Engineer section.

### Full-Stack Architecture & Infrastructure
- Sole-architected a full-stack client portal on Cloudflare Workers (D1, KV, R2), serving two custom domains from a single Worker with zero origin servers — designed for edge-first performance (sub-ms D1 reads, zero cold starts) on a platform proven to 10M+ req/day
- Designed a 15-table relational schema on D1 (edge SQLite) spanning core entities, operational tables, chat persistence, and audit logging — with UUID primary keys for distributed ID generation, E.164 phone normalization, and `ExternalId`-based cross-source deduplication

### Authentication & Security
- Implemented cookie-based JWT authentication (HS256 via `jose`) with HttpOnly/Secure/SameSite=Lax cookies and E.164 phone normalization — protecting the client dashboard from XSS and CSRF without a third-party auth provider
- Built a full OAuth 2.0 authorization code flow with PKCE (RFC 7636) from scratch — dynamic client registration, S256 code challenge verification, single-use authorization codes, refresh token rotation, and IP-based rate limiting to prevent brute-force enumeration
- Added non-blocking D1 audit logging for all OAuth events (registration, authorization, token exchange, refresh, revocation) with indexed queries by owner, action, and timestamp

### AI Chat Agent
- Built a production AI chat agent (Claude Haiku 4.5, Vercel AI SDK v6, 10 tools) that wraps all booking, cancellation, and account services — clients interact via natural language with a two-phase confirmation flow (single-use KV tokens, 5-min TTL) preventing any destructive action without explicit approval
- Implemented layered cost controls: per-user rate limiting (20 msg/hour), daily token budgets (50k tokens/day), and a circuit breaker (3 failures → 60s cooldown) — bounding API spend while maintaining availability
- Designed chat persistence with a 20-message sliding window, automatic session rotation at 100 messages, and a 90-day retention cleanup cron

### MCP Booking Server
- Built a production MCP booking server (`@modelcontextprotocol/sdk` v1.x, Streamable HTTP) enabling external AI agents (Claude Desktop) to manage bookings through 10 shared tool definitions
- Aligned MCP transport with Workers' V8 isolate model: stateless per-request server instances with closure-cached tool state — no server-side sessions, no Durable Objects required
- Centralized tool definitions across chat agent and MCP server in a single source of truth (names, Zod schemas, MCP annotations), eliminating behavioral drift between access channels

### Google Calendar Integration
- Built a Google Calendar v3 integration via service account with KV-backed token caching (55-min TTL), reducing live OAuth calls by ~98% under normal load
- Replaced pet-name substring matching with a structured JSON metadata system — `serializeMetadata` writes owner IDs at booking time, `parseEventDescription` resolves ownership on read — eliminating false attribution for multi-owner pets
- Implemented multipart/mixed HTTP batching for Calendar writes: raw multipart boundary construction (no library support in Workers), 50 PATCH requests per HTTP call, multipart response parsing, and 429/500/503 retry with exponential backoff — reducing API quota usage by up to 50x during bulk migrations

### Booking, Invoicing & Pipeline
- Built a transactional booking flow for three service types (boarding, house sit, walk/check-in) with automatic Google Calendar rollback on DB failure and capacity-aware conflict detection (boundary date soft rules, configurable daily limits)
- Built a monthly invoice pipeline: `pdf-lib` PDF synthesis → R2 storage → Resend email delivery, with historical backfill support and a concurrency guard to prevent overlapping runs; invoice download frontend with per-file streaming and batch ZIP download (`fflate` `zipSync`, ~5MB peak memory) behind ownership-verified Hono endpoints
- Built a bidirectional Notion sync: pulls owner/pet/payment records into D1, pushes locally-modified records back via dirty-flag tracking — keeping D1 and Notion synchronized without data races or manual reconciliation
- Designed a concurrent multi-source payment pipeline (Notion REST, Venmo CSV via R2, Gmail) normalized through a common `PaymentEvent` interface with idempotent `ExternalId`-based deduplication and cursor-per-source commit semantics (each source's cursor advances only after data is fully committed to D1)

### Testing & CI/CD
- Wrote 1,180+ Vitest tests across 91+ files covering auth, booking, cancellation, calendar parsing, AI tools, and all 14 dashboard components — 100% server-side statement coverage
- Configured GitHub Actions test gates on every push, Cloudflare auto-deploy on merge to `main`, and production source maps for error debugging

---

## Inline Story Expand — Content

Triggered by "↗ Read the full story" button below Brad Paws bullets. Expands inline with steel blue left border panel.

**Why this project exists**
Brad Paws is a real, operating pet care business. Paying clients book through it, invoices go out monthly, and the AI agent handles real bookings. Built to replace a manual workflow spread across Notion, Google Calendar, Venmo, and Gmail.

**The architecture decision**
Cloudflare Workers was chosen for a specific reason: the operator should never think about infrastructure. No cold starts, no scaling knobs, zero monthly cost at current volume — with a documented path to Durable Objects if write concurrency ever demands it.

**What made it hard**
The MCP server needed to share 10 tool definitions exactly with the chat agent. Behavioral drift between two access channels would silently break client-facing features. Centralizing both on a single source of truth — names, Zod schemas, MCP annotations — was a deliberate scope decision, not an accident.

**On the test count**
1,180+ tests and 100% server-side coverage isn't a metric exercise. The booking and billing flows have real financial consequences for real clients. Catching a recency-guarded upsert edge case in testing rather than in a monthly billing run was worth every test written.

---

## Previous Experience Bullets

### Front.com (2022)
- Redesigned a two-way sync microservice handling 1.1M requests/hour — replaced Node.js monolith with priority-based Go scheduler and Kafka streams, increasing accuracy and reliability for all email webhooks
- Developed and deployed a REST API for account-based rules, driving a 30% adoption increase
- Optimized CSV importer for large file uploads; upgraded core email infrastructure handling 200k+ emails/month

### Total Brain (2019–2021)
- Designed and built a Scala backend integration system for Apple, Google, and Facebook auth — drove a 60% increase in new user sign-ups
- Migrated user data from MSSQL to DynamoDB with a modernized NoSQL schema — 50% reduction in API response times, further improved with in-memory DynamoDB caching
- Standardized SSO for SAML 2.0 and JWT; integrated HubSpot APIs; built Jenkins CI/CD pipeline increasing developer productivity by 70%
- Facilitated onboarding of enterprise clients alongside Customer Success; mentored and onboarded new backend engineers

### Salesforce (2017–2018)
- Designed and implemented a persistent state repository for integration test execution using Java, PostgreSQL, and EclipseLink
- Built an OAuth2 security filter for microservice API access control and a management UI in React for the testing platform
- Technical PM for config builder/validator — owned milestones, communicated with stakeholders, presented status to directors

---

## Technical Constraints

- **Platform:** GitHub Pages (Jekyll static site generator)
- **Build:** Jekyll compiles SCSS natively — no Node.js build step
- **JavaScript:** Vanilla only — no framework, no bundler, no CDN imports required
- **Files:** `index.html` (Jekyll front matter + Liquid), `assets/css/styles.scss`
- **No new files** beyond what already exists unless absolutely necessary
- Inline expand implemented with `onclick` + CSS `display:none` toggle (not `<details>/<summary>` — styling cross-browser issues)
- Monospace font stack: `'SF Mono', ui-monospace, monospace` — system fonts only, no Google Fonts import for mono

---

## Out of Scope

- Multi-page site
- JavaScript framework or bundler
- Blog / writing section
- Dark mode toggle
- Animations — remove existing `slideDown` / `fadeIn` keyframes; clean static load suits the aesthetic
- The Inter Google Fonts `<link>` in the current `<head>` — remove it; body font switches to `system-ui`
- Any content not in `RESUME_BULLETS.md` or the existing `index.html`
