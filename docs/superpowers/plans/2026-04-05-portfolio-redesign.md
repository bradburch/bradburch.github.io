# Portfolio Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Fully rewrite `index.html` and `assets/css/styles.scss` to implement the approved steel-blue/monospace portfolio design with all Brad Paws bullets, inline story expand, previous experience, stack grid, contact section, and mobile responsiveness.

**Architecture:** Two-file rewrite — `styles.scss` provides the full design system via CSS custom properties; `index.html` is a Jekyll/Liquid template with vanilla JS for the inline expand interaction. No new files, no build step, no external JS.

**Tech Stack:** Jekyll, SCSS (compiled by GitHub Pages), vanilla JS (`onclick` + class toggle), system fonts (`system-ui`, `SF Mono`/`ui-monospace`)

**Spec:** `docs/superpowers/specs/2026-04-05-portfolio-redesign-design.md`

---

## Files

| File | Action |
|------|--------|
| `assets/css/styles.scss` | Full rewrite — design system, all components, mobile breakpoint |
| `index.html` | Full rewrite — head, header, Brad Paws, experience, stack, contact, footer |

---

### Task 1: Rewrite `styles.scss`

**Files:**
- Modify: `assets/css/styles.scss`

- [ ] **Step 1: Verify current file exists**

```bash
cat assets/css/styles.scss | head -5
```
Expected: sees the current purple-gradient styles. Confirms path is correct.

- [ ] **Step 2: Rewrite `assets/css/styles.scss` in full**

```scss
---
---
:root {
  --color-bg: #ffffff;
  --color-text: #374151;
  --color-heading: #111827;
  --color-muted: #9ca3af;
  --color-divider: #f3f4f6;
  --color-accent: #1e40af;
  --color-accent-light: #3b82f6;
  --color-tag-bg: #eff6ff;
  --color-tag-border: #bfdbfe;
  --color-tag-text: #1e3a8a;
  --color-story-bg: #eff6ff;
  --color-story-border: #1d4ed8;
  --font-sans: system-ui, -apple-system, sans-serif;
  --font-mono: 'SF Mono', ui-monospace, monospace;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: var(--font-sans);
  color: var(--color-text);
  background: #f1f5f9;
  line-height: 1.6;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 32px 20px 60px;
}

/* ── Header ─────────────────────────────── */

.site-header {
  border-top: 4px solid var(--color-accent);
  padding: 36px 0 28px;
  margin-bottom: 40px;
  background: var(--color-bg);
  border-radius: 0 0 8px 8px;
  padding-left: 28px;
  padding-right: 28px;
}

.site-header h1 {
  font-size: 2em;
  font-weight: 900;
  color: var(--color-heading);
  letter-spacing: -0.03em;
  margin-bottom: 4px;
}

.header-title {
  font-size: 0.72em;
  color: var(--color-muted);
  letter-spacing: 0.07em;
  text-transform: uppercase;
  font-weight: 600;
  margin-bottom: 14px;
}

.header-summary {
  font-size: 0.88em;
  color: var(--color-text);
  line-height: 1.75;
  max-width: 640px;
  margin-bottom: 18px;
}

.header-links {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.header-link {
  font-family: var(--font-mono);
  font-size: 0.72em;
  font-weight: 700;
  color: var(--color-accent);
  background: var(--color-tag-bg);
  padding: 5px 12px;
  border-radius: 4px;
  text-decoration: none;
  border: 1px solid var(--color-tag-border);
  transition: background 0.15s, color 0.15s;
}

.header-link:hover {
  background: var(--color-accent);
  color: #fff;
  border-color: var(--color-accent);
}

/* ── Section layout ─────────────────────── */

.page-section {
  background: var(--color-bg);
  border-radius: 8px;
  padding: 28px;
  margin-bottom: 16px;
}

.section-label {
  font-size: 0.62em;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--color-muted);
  margin-bottom: 20px;
}

.section-divider {
  border: none;
  border-top: 1px solid var(--color-divider);
  margin: 28px 0;
}

/* ── Job block ──────────────────────────── */

.job {
  margin-bottom: 0;
}

.job + .job {
  padding-top: 28px;
  border-top: 1px solid var(--color-divider);
  margin-top: 28px;
}

.job-head {
  display: flex;
  align-items: baseline;
  gap: 8px;
  flex-wrap: wrap;
  margin-bottom: 2px;
}

.job-title {
  font-size: 0.95em;
  font-weight: 800;
  color: var(--color-heading);
  letter-spacing: -0.01em;
}

.job-company {
  font-size: 0.78em;
  font-weight: 600;
  color: var(--color-text);
}

.job-badge {
  font-size: 0.6em;
  font-weight: 700;
  color: #fff;
  background: var(--color-accent);
  padding: 2px 9px;
  border-radius: 10px;
  vertical-align: middle;
}

.job-rif {
  font-size: 0.62em;
  color: var(--color-muted);
  background: #f9fafb;
  border: 1px solid var(--color-divider);
  padding: 1px 7px;
  border-radius: 10px;
  vertical-align: middle;
}

.job-meta {
  font-size: 0.7em;
  color: var(--color-muted);
  margin-bottom: 12px;
}

/* ── Stack tags (job-level) ─────────────── */

.stack-tags {
  display: flex;
  gap: 5px;
  flex-wrap: wrap;
  margin-bottom: 18px;
}

.stack-tag {
  font-family: var(--font-mono);
  font-size: 0.63em;
  font-weight: 600;
  color: var(--color-tag-text);
  background: var(--color-tag-bg);
  padding: 2px 9px;
  border-radius: 4px;
  border: 1px solid var(--color-tag-border);
}

/* ── Subsystem groups ───────────────────── */

.subsystem {
  margin-bottom: 18px;
}

.subsystem:last-of-type {
  margin-bottom: 0;
}

.subsystem-label {
  display: block;
  font-size: 0.6em;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--color-accent);
  border-left: 3px solid var(--color-story-border);
  padding-left: 7px;
  margin-bottom: 8px;
}

/* ── Bullet lists ───────────────────────── */

.bullets {
  list-style: none;
  padding: 0;
  margin: 0;
}

.bullets li {
  font-size: 0.78em;
  color: var(--color-text);
  line-height: 1.82;
  padding-left: 16px;
  position: relative;
  margin-bottom: 4px;
}

.bullets li:last-child {
  margin-bottom: 0;
}

.bullets li::before {
  content: "→";
  position: absolute;
  left: 0;
  color: var(--color-accent-light);
  font-weight: 700;
}

.bullets li code {
  font-family: var(--font-mono);
  font-size: 0.9em;
  background: #f1f5f9;
  padding: 1px 4px;
  border-radius: 3px;
  color: var(--color-heading);
}

/* ── Inline story expand ────────────────── */

.story-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  margin-top: 16px;
  font-family: var(--font-mono);
  font-size: 0.7em;
  font-weight: 700;
  color: var(--color-accent);
  cursor: pointer;
  border: 1.5px solid var(--color-accent);
  padding: 6px 16px;
  border-radius: 20px;
  background: #fff;
  transition: background 0.15s;
  min-height: 36px;
}

.story-btn:hover {
  background: var(--color-tag-bg);
}

.story-panel {
  display: none;
  margin-top: 16px;
  background: var(--color-story-bg);
  border-left: 3px solid var(--color-story-border);
  padding: 16px 18px;
  border-radius: 0 8px 8px 0;
}

.story-panel.open {
  display: block;
}

.story-panel h4 {
  font-family: var(--font-mono);
  font-size: 0.65em;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--color-accent);
  margin-bottom: 5px;
  margin-top: 14px;
}

.story-panel h4:first-child {
  margin-top: 0;
}

.story-panel p {
  font-size: 0.78em;
  color: var(--color-text);
  line-height: 1.75;
}

/* ── Technical stack grid ───────────────── */

.stack-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 8px;
}

.stack-item {
  font-family: var(--font-mono);
  font-size: 0.72em;
  font-weight: 600;
  color: var(--color-tag-text);
  background: var(--color-tag-bg);
  border: 1px solid var(--color-tag-border);
  border-radius: 6px;
  padding: 8px 12px;
}

.stack-cat {
  display: block;
  font-family: var(--font-sans);
  font-size: 0.68em;
  font-weight: 400;
  color: var(--color-muted);
  text-transform: uppercase;
  letter-spacing: 0.06em;
  margin-bottom: 2px;
}

/* ── Contact block ──────────────────────── */

.contact-block {
  background: #f9fafb;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  padding: 20px 22px;
}

.contact-block p {
  font-size: 0.82em;
  color: var(--color-text);
  line-height: 1.75;
  margin-bottom: 12px;
}

.contact-links {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.contact-link {
  font-family: var(--font-mono);
  font-size: 0.72em;
  font-weight: 700;
  color: var(--color-accent);
  background: var(--color-tag-bg);
  padding: 5px 12px;
  border-radius: 4px;
  text-decoration: none;
  border: 1px solid var(--color-tag-border);
  display: inline-flex;
  align-items: center;
  min-height: 32px;
  transition: background 0.15s, color 0.15s;
}

.contact-link:hover {
  background: var(--color-accent);
  color: #fff;
  border-color: var(--color-accent);
}

/* ── Footer ─────────────────────────────── */

footer {
  text-align: center;
  color: var(--color-muted);
  font-size: 0.75em;
  padding: 28px 0 0;
}

/* ── Mobile (≤640px) ────────────────────── */

@media (max-width: 640px) {
  .container {
    padding: 12px 10px 40px;
  }

  .site-header {
    padding: 20px 16px 18px;
    margin-bottom: 12px;
  }

  .site-header h1 {
    font-size: 1.6em;
  }

  .header-summary {
    font-size: 0.82em;
  }

  .page-section {
    padding: 20px 16px;
    margin-bottom: 10px;
  }

  .section-divider {
    margin: 20px 0;
  }

  .subsystem-label {
    font-size: 0.65em;
  }

  .bullets li {
    font-size: 0.82em;
  }

  .story-btn {
    min-height: 44px;
    font-size: 0.75em;
    width: 100%;
    justify-content: center;
  }

  .contact-link {
    min-height: 44px;
    padding: 8px 14px;
  }

  .stack-grid {
    grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
  }

  .header-link {
    min-height: 40px;
    padding: 8px 12px;
  }
}
```

- [ ] **Step 3: Verify Jekyll builds without error**

```bash
bundle exec jekyll build 2>&1 | tail -5
```
Expected: `done in X seconds` with no error lines. If `bundle exec` fails, try `jekyll build`.

- [ ] **Step 4: Commit**

```bash
git add assets/css/styles.scss
git commit -m "feat: rewrite styles.scss with steel blue design system and mobile breakpoint"
```

---

### Task 2: Rewrite `index.html` — head + header

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace the `<head>` block**

Replace the entire `<head>...</head>` with:

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Brad Burch — Software Engineer & Solutions Engineer</title>
    <link rel="stylesheet" href="/assets/css/styles.css">
    {%- if jekyll.environment == 'production' and site.google_analytics -%}
    {%- include google-analytics.html -%}
    {%- endif -%}
</head>
```

Note: Inter Google Fonts `<link>` is intentionally removed. `styles.css` is the compiled output of `styles.scss` — Jekyll handles this automatically.

- [ ] **Step 2: Replace the `<body>` opening and header block**

Replace from `<body>` through the closing `</header>` tag with:

```html
<body>
    <div class="container">
        <header class="site-header">
            <h1>Brad Burch</h1>
            <p class="header-title">Software Engineer &nbsp;·&nbsp; Solutions Engineer</p>
            <p class="header-summary">Sole engineer on a production AI booking platform: auth from scratch, Claude agent with 10 tools, MCP server, invoicing pipeline, and 1,180+ tests — all on Cloudflare's edge. Targeting SWE and SE roles in San Francisco.</p>
            <div class="header-links">
                <a href="https://github.com/bradburch" class="header-link" target="_blank">github</a>
                <a href="mailto:bradburch.jobs@gmail.com" class="header-link">email</a>
                <a href="{{page.linkedin}}" class="header-link" target="_blank">linkedin</a>
                <a href="{{page.resume}}" class="header-link">resume.pdf</a>
            </div>
        </header>
```

Note: GitHub is listed first per spec — SF hiring managers click it immediately.

- [ ] **Step 3: Build and open in browser to verify header**

```bash
bundle exec jekyll serve --livereload
```

Open `http://localhost:4000`. Verify:
- Steel blue top border on header
- Name is all black (no color on "Burch")
- Four monospace pill links, GitHub first
- Header summary text is readable, not truncated on desktop

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: rewrite head and header section"
```

---

### Task 3: Rewrite `index.html` — Brad Paws block

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add the Experience section opening and Brad Paws block**

After the closing `</header>` tag, replace everything through the end of the current "Brad's Pet Services" job div with:

```html
        <main>
            <section class="page-section">
                <p class="section-label">Experience</p>

                <div class="job">
                    <div class="job-head">
                        <span class="job-title">Brad Paws</span>
                        <span class="job-badge">Sole Engineer</span>
                    </div>
                    <p class="job-meta">San Francisco, CA &nbsp;·&nbsp; 2022–Present</p>

                    <div class="stack-tags">
                        <span class="stack-tag">TypeScript</span>
                        <span class="stack-tag">Cloudflare Workers</span>
                        <span class="stack-tag">D1 · KV · R2</span>
                        <span class="stack-tag">Hono</span>
                        <span class="stack-tag">React 19</span>
                        <span class="stack-tag">Claude Haiku 4.5</span>
                        <span class="stack-tag">MCP</span>
                        <span class="stack-tag">OAuth 2.0+PKCE</span>
                        <span class="stack-tag">Vitest</span>
                    </div>

                    <div class="subsystem">
                        <span class="subsystem-label">AI Chat Agent</span>
                        <ul class="bullets">
                            <li>Built a production AI chat agent (Claude Haiku 4.5, Vercel AI SDK v6, 10 tools) that wraps all booking, cancellation, and account services — clients interact via natural language with a two-phase confirmation flow (single-use KV tokens, 5-min TTL) preventing any destructive action without explicit approval</li>
                            <li>Implemented layered cost controls: per-user rate limiting (20 msg/hour), daily token budgets (50k tokens/day), and a circuit breaker (3 failures → 60s cooldown) — bounding API spend while maintaining availability</li>
                            <li>Designed chat persistence with a 20-message sliding window, automatic session rotation at 100 messages, and a 90-day retention cleanup cron</li>
                        </ul>
                    </div>

                    <div class="subsystem">
                        <span class="subsystem-label">MCP Booking Server</span>
                        <ul class="bullets">
                            <li>Built a production MCP booking server (<code>@modelcontextprotocol/sdk</code> v1.x, Streamable HTTP) enabling external AI agents (Claude Desktop) to manage bookings through 10 shared tool definitions</li>
                            <li>Aligned MCP transport with Workers' V8 isolate model: stateless per-request server instances with closure-cached tool state — no server-side sessions, no Durable Objects required</li>
                            <li>Centralized tool definitions across chat agent and MCP server in a single source of truth (names, Zod schemas, MCP annotations), eliminating behavioral drift between access channels</li>
                        </ul>
                    </div>

                    <div class="subsystem">
                        <span class="subsystem-label">Authentication &amp; Security</span>
                        <ul class="bullets">
                            <li>Implemented cookie-based JWT authentication (HS256 via <code>jose</code>) with HttpOnly/Secure/SameSite=Lax cookies and E.164 phone normalization — protecting the client dashboard from XSS and CSRF without a third-party auth provider</li>
                            <li>Built a full OAuth 2.0 authorization code flow with PKCE (RFC 7636) from scratch — dynamic client registration, S256 code challenge verification, single-use authorization codes, refresh token rotation, and IP-based rate limiting to prevent brute-force enumeration</li>
                            <li>Added non-blocking D1 audit logging for all OAuth events (registration, authorization, token exchange, refresh, revocation) with indexed queries by owner, action, and timestamp</li>
                        </ul>
                    </div>

                    <div class="subsystem">
                        <span class="subsystem-label">Full-Stack Architecture &amp; Infrastructure</span>
                        <ul class="bullets">
                            <li>Sole-architected a full-stack client portal on Cloudflare Workers (D1, KV, R2), serving two custom domains from a single Worker with zero origin servers — designed for edge-first performance (sub-ms D1 reads, zero cold starts) on a platform proven to 10M+ req/day</li>
                            <li>Designed a 15-table relational schema on D1 (edge SQLite) spanning core entities, operational tables, chat persistence, and audit logging — with UUID primary keys for distributed ID generation, E.164 phone normalization, and <code>ExternalId</code>-based cross-source deduplication</li>
                        </ul>
                    </div>

                    <div class="subsystem">
                        <span class="subsystem-label">Google Calendar Integration</span>
                        <ul class="bullets">
                            <li>Built a Google Calendar v3 integration via service account with KV-backed token caching (55-min TTL), reducing live OAuth calls by ~98% under normal load</li>
                            <li>Replaced pet-name substring matching with a structured JSON metadata system — <code>serializeMetadata</code> writes owner IDs at booking time, <code>parseEventDescription</code> resolves ownership on read — eliminating false attribution for multi-owner pets</li>
                            <li>Implemented multipart/mixed HTTP batching for Calendar writes: raw multipart boundary construction (no library support in Workers), 50 PATCH requests per HTTP call, multipart response parsing, and 429/500/503 retry with exponential backoff — reducing API quota usage by up to 50x during bulk migrations</li>
                        </ul>
                    </div>

                    <div class="subsystem">
                        <span class="subsystem-label">Booking, Invoicing &amp; Pipeline</span>
                        <ul class="bullets">
                            <li>Built a transactional booking flow for three service types (boarding, house sit, walk/check-in) with automatic Google Calendar rollback on DB failure and capacity-aware conflict detection (boundary date soft rules, configurable daily limits)</li>
                            <li>Built a monthly invoice pipeline: <code>pdf-lib</code> PDF synthesis → R2 storage → Resend email delivery, with historical backfill support and a concurrency guard to prevent overlapping runs; invoice download with per-file streaming and batch ZIP download (<code>fflate</code> <code>zipSync</code>, ~5MB peak memory) behind ownership-verified Hono endpoints</li>
                            <li>Built a bidirectional Notion sync: pulls owner/pet/payment records into D1, pushes locally-modified records back via dirty-flag tracking — keeping D1 and Notion synchronized without data races or manual reconciliation</li>
                            <li>Designed a concurrent multi-source payment pipeline (Notion REST, Venmo CSV via R2, Gmail) normalized through a common <code>PaymentEvent</code> interface with idempotent <code>ExternalId</code>-based deduplication and cursor-per-source commit semantics</li>
                        </ul>
                    </div>

                    <div class="subsystem">
                        <span class="subsystem-label">Testing &amp; CI/CD</span>
                        <ul class="bullets">
                            <li>Wrote 1,180+ Vitest tests across 91+ files covering auth, booking, cancellation, calendar parsing, AI tools, and all 14 dashboard components — 100% server-side statement coverage</li>
                            <li>Configured GitHub Actions test gates on every push, Cloudflare auto-deploy on merge to <code>main</code>, and production source maps for error debugging</li>
                        </ul>
                    </div>

                    <button class="story-btn" onclick="
                        var p = document.getElementById('brad-paws-story');
                        var open = p.classList.toggle('open');
                        this.textContent = open ? '↑ Collapse' : '↗ Read the full story';
                    ">↗ Read the full story</button>

                    <div class="story-panel" id="brad-paws-story">
                        <h4>Why this project exists</h4>
                        <p>Brad Paws is a real, operating pet care business. Paying clients book through it, invoices go out monthly, and the AI agent handles real bookings. Built to replace a manual workflow spread across Notion, Google Calendar, Venmo, and Gmail.</p>
                        <h4>The architecture decision</h4>
                        <p>Cloudflare Workers was chosen for a specific reason: the operator should never think about infrastructure. No cold starts, no scaling knobs, zero monthly cost at current volume — with a documented path to Durable Objects if write concurrency ever demands it.</p>
                        <h4>What made it hard</h4>
                        <p>The MCP server needed to share 10 tool definitions exactly with the chat agent. Behavioral drift between two access channels would silently break client-facing features. Centralizing both on a single source of truth — names, Zod schemas, MCP annotations — was a deliberate scope decision, not an accident.</p>
                        <h4>On the test count</h4>
                        <p>1,180+ tests and 100% server-side coverage isn't a metric exercise. The booking and billing flows have real financial consequences for real clients. Catching a recency-guarded upsert edge case in testing rather than in a monthly billing run was worth every test written.</p>
                    </div>
                </div>
```

- [ ] **Step 2: Verify in browser**

With `jekyll serve` running, open `http://localhost:4000`. Verify:
- 7 subsystem labels with blue left-border treatment
- AI Chat Agent and MCP Booking Server appear first
- "↗ Read the full story" button opens inline panel below bullets
- Button text changes to "↑ Collapse" when open
- Stack tags are monospace and blue-tinted
- No horizontal scroll on desktop

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Brad Paws block with all subsystem bullets and inline story expand"
```

---

### Task 4: Rewrite `index.html` — Previous Experience

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add previous experience section**

Immediately after the Brad Paws closing `</div>` (end of `.job`), and before the next `</section>`, add the three older jobs. Replace the existing Front.com, Total Brain, and Salesforce job divs entirely:

```html
                <div class="job">
                    <div class="job-head">
                        <span class="job-title">Software Engineer</span>
                        <span class="job-company">· Front.com</span>
                        <span class="job-rif">Impacted by RIF</span>
                    </div>
                    <p class="job-meta">San Francisco, CA &nbsp;·&nbsp; 2022</p>
                    <ul class="bullets">
                        <li>Redesigned a two-way sync microservice handling <strong>1.1M requests/hour</strong> — replaced a Node.js monolith with a priority-based Go scheduler and Kafka streams, increasing accuracy and reliability for all email webhooks</li>
                        <li>Developed and deployed a REST API for account-based rules, driving a <strong>30% adoption increase</strong></li>
                        <li>Optimized CSV importer for large file uploads; upgraded core email infrastructure handling <strong>200k+ emails/month</strong></li>
                    </ul>
                </div>

                <div class="job">
                    <div class="job-head">
                        <span class="job-title">Software Engineer</span>
                        <span class="job-company">· Total Brain</span>
                        <span class="job-rif">Impacted by RIF</span>
                    </div>
                    <p class="job-meta">San Francisco, CA &nbsp;·&nbsp; 2019–2021</p>
                    <ul class="bullets">
                        <li>Designed and built a Scala backend integration system for Apple, Google, and Facebook auth — drove a <strong>60% increase</strong> in new user sign-ups</li>
                        <li>Migrated user data from MSSQL to DynamoDB with a modernized NoSQL schema — <strong>50% reduction</strong> in API response times, further improved with in-memory DynamoDB caching</li>
                        <li>Standardized SSO for SAML 2.0 and JWT; integrated HubSpot APIs; built Jenkins CI/CD pipeline increasing developer productivity by <strong>70%</strong></li>
                        <li>Facilitated onboarding of enterprise clients alongside Customer Success; mentored and onboarded new backend engineers</li>
                    </ul>
                </div>

                <div class="job">
                    <div class="job-head">
                        <span class="job-title">Software Engineer</span>
                        <span class="job-company">· Salesforce</span>
                    </div>
                    <p class="job-meta">San Francisco, CA &nbsp;·&nbsp; 2017–2018</p>
                    <ul class="bullets">
                        <li>Designed and implemented a persistent state repository for integration test execution using Java, PostgreSQL, and EclipseLink</li>
                        <li>Built an OAuth2 security filter for microservice API access control and a management UI in React for the testing platform</li>
                        <li>Technical PM for config builder/validator — owned milestones, communicated with stakeholders, presented status to directors</li>
                    </ul>
                </div>
            </section>
```

- [ ] **Step 2: Verify in browser**

Check `http://localhost:4000`. Verify:
- Three older jobs appear below Brad Paws with dividers between them
- "Impacted by RIF" pill appears in muted gray on Front.com and Total Brain
- Salesforce has no RIF pill
- Metric values (`1.1M`, `30%`, `60%`, etc.) render in bold

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add previous experience section (Front, Total Brain, Salesforce)"
```

---

### Task 5: Rewrite `index.html` — Stack, Contact, Footer

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add Technical Stack section**

After the closing `</section>` of the experience block, add:

```html
            <section class="page-section">
                <p class="section-label">Technical Stack</p>
                <div class="stack-grid">
                    <div class="stack-item"><span class="stack-cat">Language</span>TypeScript / JS</div>
                    <div class="stack-item"><span class="stack-cat">Runtime</span>CF Workers</div>
                    <div class="stack-item"><span class="stack-cat">Storage</span>D1 · KV · R2</div>
                    <div class="stack-item"><span class="stack-cat">Framework</span>Hono · React 19</div>
                    <div class="stack-item"><span class="stack-cat">AI</span>Claude · MCP</div>
                    <div class="stack-item"><span class="stack-cat">Auth</span>JWT · OAuth2+PKCE</div>
                    <div class="stack-item"><span class="stack-cat">Testing</span>Vitest</div>
                    <div class="stack-item"><span class="stack-cat">CI/CD</span>GH Actions</div>
                    <div class="stack-item"><span class="stack-cat">Prior</span>Java · Scala · Go · Python</div>
                    <div class="stack-item"><span class="stack-cat">Prior</span>PostgreSQL · DynamoDB · Kafka</div>
                </div>
            </section>
```

- [ ] **Step 2: Add Contact section and footer, close container and body**

```html
            <section class="page-section" id="contact">
                <p class="section-label">Get In Touch</p>
                <div class="contact-block">
                    <p>Actively targeting <strong>Software Engineer</strong> and <strong>Solutions Engineer</strong> roles in San Francisco. Open to in-person, hybrid, and remote. Strong fit for teams where production ownership, AI integration, and customer impact overlap.</p>
                    <div class="contact-links">
                        <a href="mailto:bradburch.jobs@gmail.com" class="contact-link">bradburch.jobs@gmail.com</a>
                        <a href="https://github.com/bradburch" class="contact-link" target="_blank">github</a>
                        <a href="{{page.linkedin}}" class="contact-link" target="_blank">linkedin</a>
                        <a href="{{page.resume}}" class="contact-link">resume.pdf</a>
                    </div>
                </div>
            </section>
        </main>

        <footer>
            <p>© 2026 Brad Burch</p>
        </footer>
    </div>
</body>
</html>
```

- [ ] **Step 3: Remove any remaining old HTML**

Verify `index.html` has no leftover sections from the old design — no `.highlight`, no `.skills-grid`, no `.skill-tag`, no `.achievement`, no `.metric`, no old `<footer>` with white color, no old `<section class="section">` blocks. The file should end cleanly at `</html>`.

```bash
grep -n "skill-tag\|achievement\|metric\|skills-grid\|section section\|cta-button" index.html
```
Expected: no output. If any lines are found, delete them.

- [ ] **Step 4: Verify full page in browser**

Check `http://localhost:4000`. Full checklist:
- Header: steel blue top border, black name, monospace links, GitHub first
- Brad Paws: 7 subsystems, AI Chat Agent first, MCP second
- Story expand button works, panel opens/closes with correct text
- Previous experience: 3 jobs with correct bullets and RIF pills
- Stack grid: 10 items, monospace, blue tinted
- Contact block: gray background, correct copy, monospace links
- Footer: muted gray "© 2026 Brad Burch"
- No purple anywhere on the page

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add stack grid, contact section, and footer — complete page rewrite"
```

---

### Task 6: Mobile Verification and Final Commit

**Files:**
- Modify: `index.html` and/or `assets/css/styles.scss` only if issues found

- [ ] **Step 1: Open browser DevTools and verify mobile layout**

In Chrome/Firefox DevTools, set viewport to 390×844 (iPhone 14). Verify:
- Header text does not overflow or clip
- Stack tags wrap correctly — no horizontal scroll
- Subsystem labels are readable (should be slightly larger than desktop per breakpoint)
- "↗ Read the full story" button spans full width, min-height 44px
- Contact links are tappable (min-height 44px)
- Page sections have correct reduced padding (20px 16px)
- No element causes horizontal scroll at 320px width

- [ ] **Step 2: Verify 320px (smallest supported width)**

Set DevTools viewport to 320×568. The page must render without horizontal scroll. If any element overflows, add `overflow-x: hidden` to `.container` in `styles.scss` and rebuild.

- [ ] **Step 3: Run final Jekyll build for production**

```bash
JEKYLL_ENV=production bundle exec jekyll build 2>&1 | tail -5
```
Expected: clean build, no errors, no warnings about missing files.

- [ ] **Step 4: Final commit**

```bash
git add index.html assets/css/styles.scss
git commit -m "feat: complete portfolio redesign — steel blue, Brad Paws bullets, mobile responsive"
```

- [ ] **Step 5: Push to GitHub Pages**

```bash
git push origin main
```

Wait ~60 seconds, then open `https://bradburch.github.io` to verify the live site matches the local preview.
