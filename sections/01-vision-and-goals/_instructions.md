# SECTION 1: VISION & GOALS - BUILD INSTRUCTIONS

## Overview
You are building Section 1 of the CFO Business Intelligence Calculator Suite PDR. This is the foundational section that defines WHY this product exists, WHO it serves, WHAT problems it solves, and WHAT success looks like.

## Context
- This is a suite of 8+ business calculators for CFOs, founders, lenders, and advisors
- Target: small to mid-sized businesses needing CFO-grade financial intelligence without a full FP&A team
- Monetization: Free tier (limited) → Pro tier (full features) → AI add-on (narratives)
- Initial scope: US and Canada only, single currency per scenario, no GL imports

## Your Task
Create 8 markdown files in this directory, one for each subsection:
1. `1.1-product-vision.md`
2. `1.2-target-users.md`
3. `1.3-cfo-grade-definition.md`
4. `1.4-differentiators.md`
5. `1.5-monetization.md`
6. `1.6-quality-goals.md`
7. `1.7-constraints.md`
8. `1.8-internal-operations.md`

---

## 1.1 PRODUCT VISION

**File:** `1.1-product-vision.md`

**Content Requirements:**
- Opening paragraph: What is this product in one sentence + primary value delivered
- Value equation with measurable targets:
  - Time savings: "Reduce decision cycle from X days to Y minutes"
  - Error reduction: "Reduce spreadsheet version-chasing by 50-70%"
  - Reliability: "95% numeric accuracy vs reference models (±0.1% tolerance)"
- Top 3 outcomes (in priority order):
  1. Speed (drastically shorten time from question to answer)
  2. Defensibility (outputs that stand up to boards, lenders, auditors)
  3. Embed-ability (easy to reuse across sites and dashboards)
- Product philosophy:
  - Feel like a calculator, behave like a lightweight BI tool
  - Consistent, trustworthy outputs across all calculators
  - Produce board-ready PDFs, banker-friendly CSVs
  - Modular and embeddable (WordPress, web app, dashboards)

**Format:**
- Markdown headers (## for section title, ### for subsections)
- Bullet lists for value equation and outcomes
- Paragraphs for vision and philosophy

**Length:** 1.5-2 pages

---

## 1.2 TARGET USERS AND CONTEXTS

**File:** `1.2-target-users.md`

**Content Requirements:**
- **Primary user personas** (create a table):
  - CFOs / Controllers / Finance Directors
  - Owners / Founders / CEOs (SMB)
  - Lenders / Brokers / Advisors
  - FP&A / Analysts
  - For each: what they need from this product

- **Day-1 usage contexts:**
  - Pre-qualification and deal exploration
  - "Can we afford this?" decisions
  - Quick margin and cash-flow checks

- **Day-2 / recurring workflows:**
  - Month-end and quarter-end reruns
  - Board packet preparation
  - Lender renewal packages
  - CEO approval flows

- **Permissions and collaboration scope (v1):**
  - Full scenario sharing vs outputs-only views
  - Lightweight "prepared by / reviewed by" metadata
  - What's out of scope: heavy workflow/approval engines

**Format:**
- Table for personas: | Role | Need | Example Use Case |
- Bullet lists for Day-1 and Day-2 contexts
- Paragraph for collaboration scope

**Length:** 2-3 pages

---

## 1.3 DEFINITION OF "CFO-GRADE" AND CORE PROBLEMS TO SOLVE

**File:** `1.3-cfo-grade-definition.md`

**Content Requirements:**
- **CFO-grade standards (non-negotiable):**
  - Audited formulas (validated vs reference models, ±0.1% tolerance)
  - Explicit units, currency, time handling (no ambiguity)
  - Audit trail of assumptions (reconstruct any result)
  - Permissions and roles (viewer vs editor, internal vs external sharing)
  - Export quality (board-ready PDFs with version stamps, banker-friendly CSVs)

- **Core problems the suite must solve:**
  - Decision clarity (yes/no, Option A vs B vs C with tradeoffs)
  - Scenario comparison and baselines (side-by-side, change tracking)
  - Risk visibility (warnings, thresholds, covenant tension)
  - Time savings (replace 30-90 min of spreadsheet work with guided calculator)
  - Shareable, repeatable outputs (PDFs, CSVs with timestamps and versions)

**Format:**
- Section: "CFO-Grade Standards" with bullet list (5-7 standards, each with 1-2 sentence description)
- Section: "Core Problems to Solve" with bullet list (5 problems, each with 2-3 sentence description)

**Length:** 2-3 pages

---

## 1.4 DIFFERENTIATORS AND TECHNICAL DIRECTION

**File:** `1.4-differentiators.md`

**Content Requirements:**
- **Differentiators vs generic calculators:**
  - CFO-grade metrics beyond basics (DSCR, coverage ratios, NPV/IRR, not just payment amounts)
  - Consistent UX, language, behavior (same layout across all calculators)
  - Scenario-centric design (named scenarios, side-by-side comparison)
  - AI-enhanced but not AI-dependent (deterministic math + AI commentary, AI never modifies formulas)

- **High-level technical direction:**
  - Shared calculation engine (versioned formula library, strongly typed)
  - Modular UI shells (React/TypeScript components, embed anywhere)
  - Export services (centralized PDF/CSV generation)
  - Locale/currency/tax parameterization (config-driven for US/CA)
  - Data boundaries (aggregated scenario inputs only, no GL imports, no PII by design)
  - AI guardrails (logging, redaction, opt-out, "advisory only" disclaimers)
  - Self-hosted/B2B data controls (retention policies, telemetry opt-out, AI disable options)

**Format:**
- Section: "Differentiators" with 4 bullet points (2-3 sentences each)
- Section: "Technical Direction" with 7-8 bullet points (1-2 sentences each)

**Length:** 2-3 pages

---

## 1.5 MONETIZATION AND BUSINESS GOALS

**File:** `1.5-monetization.md`

**Content Requirements:**
- **Tiered monetization model:**
  - Free tier: single scenario, key metrics only, watermarked exports, ads/affiliate placement
  - Pro tier: multiple scenarios, advanced metrics (DSCR, NPV/IRR, etc.), clean exports, sharing
  - AI add-on: narratives, suggestions, risk commentary (capped per month)
  - B2B/white-label: embeddable SDK, branding, feature toggles, per-seat/per-domain pricing

- **Advanced metrics definition (default Pro-only):**
  - Financing & Lending: DSCR, covenant headroom, NPV/IRR, detailed fee breakdowns
  - Cash Flow & Liquidity: multi-scenario runway charts, sensitivity views
  - Profitability & Pricing: contribution margin, margin of safety, multi-scenario pricing charts
  - Valuation & Deal: multi-scenario valuation ranges, sensitivity charts
  - Planning & Scenario: scenario comparisons within single tools, sensitivity views

- **Identity and access defaults (SaaS):**
  - Anonymous users: can use calculators, tracked via session IDs, exports stored 24 hours max
  - Logged-in users: required for saving scenarios, cross-device access
  - Telemetry: pseudonymous IDs, aggregated descriptors, no PII in logs

- **Measurable product/business goals:**
  - Suite-level: SEO traffic for target keywords, Free → Pro conversion rates, exports per session, AI adoption rate
  - Calculator-level: each calculator PDR must declare primary business role and success metric

**Format:**
- Table for tier definitions: | Tier | Features | Limits | Pricing/Gates |
- Section: "Advanced Metrics by Category" with bullet list by category
- Section: "Identity and Access" with paragraphs for anonymous, logged-in, telemetry
- Section: "Business Goals" with bullet list for suite-level, statement for calculator-level

**Length:** 3-4 pages

---

## 1.6 PRODUCT QUALITY GOALS, SLAS, AND DATA RETENTION

**File:** `1.6-quality-goals.md`

**Content Requirements:**
- **Numerical correctness and stability:**
  - Calculations match reference models within ±0.1% tolerance
  - Edge cases handled gracefully (no NaNs, infinities, nonsense)

- **Performance SLAs (internal targets):**
  - Server-side:
    - p95 recalculation latency < 150ms (typical calculators, excluding WAN/AI)
    - p95 export generation < 3 seconds (standard report sizes)
    - p99 recalculation < 500ms, p99 export < 6 seconds
  - End-to-end UX:
    - Visible UI update within 300ms (modern devices, reasonable network)
    - 500ms on low-end devices (with loading feedback)
    - AI narratives: ~1-3 seconds p95, 6-8 second timeout, hard cap ~10 seconds
  - Large exports: progress indicators, async generation

- **Reliability and regression safety:**
  - Formula changes covered by regression tests with golden scenarios
  - Automated test suites on each release
  - Formula and product version IDs in exports

- **Clarity of explanation:**
  - Tooltips: plain language, no jargon
  - Warnings: direct and actionable (e.g., "Your DSCR is below 1.25; consider reducing debt or improving cash flow")

- **Consistency and maintainability:**
  - Shared components (no copy-paste)
  - Versioning and change logs

- **Data retention, logs, and analytics:**
  - Saved scenarios: 24 months default (SaaS), subject to user deletion
  - Usage analytics/logs: 12 months default (no PII, pseudonymous IDs only)
  - B2B deployments: configurable retention, opt-out telemetry

**Format:**
- 6 sections with headers (##)
- Bullet lists under each (2-5 bullets per section)
- Table for SLAs: | Metric | Target (p95) | Target (p99) | Notes |

**Length:** 3-4 pages

---

## 1.7 CONSTRAINTS AND NON-GOALS

**File:** `1.7-constraints.md`

**Content Requirements:**
- **What the suite will NOT do (v1):**
  - Not a full accounting, ERP, payroll, or tax filing system
  - No bank connections or transaction-level feeds (v1)
  - No automated accounting/ERP imports (v1)
  - No detailed multi-jurisdictional tax (only US/CA with basic assumptions)
  - No customer-identifiable transactions (aggregated scenarios only)
  - No legally binding credit/underwriting/investment advice (decision-support only)
  - No full offline operation (connectivity required for exports and AI)
  - No heavy workflow engines (lightweight prepared/reviewed metadata only)
  - No multi-currency within a single scenario (v1)

- **Handling out-of-scope requests:**
  - Reject as out-of-scope, OR
  - Break out as separate phase with its own PDR and risk review

**Format:**
- Bullet list: 9-10 non-goals with brief explanations (1-2 sentences each)
- Closing paragraph: process for handling conflicts

**Length:** 1-2 pages

---

## 1.8 INTERNAL OPERATIONS & MANAGEMENT SYSTEMS

**File:** `1.8-internal-operations.md`

**Content Requirements:**
This subsection covers 10 internal dashboards needed to run the business:

1. **API Management Dashboard**
   - Primary users, purpose
   - Key features: API health, rate limiting, API key management (B2B), usage analytics
   - Actions available: enable/disable endpoints, adjust rate limits, generate/revoke keys

2. **Subscription & Billing Dashboard**
   - Primary users, purpose
   - Key features: subscriber overview, subscription management, payment tracking, revenue metrics (MRR, ARR, ARPU, churn, LTV), invoicing
   - Actions available: upgrade/downgrade users, issue refunds, extend trials

3. **Business Intelligence Dashboard**
   - Primary users, purpose
   - Key features: calculator usage metrics, conversion funnels, user cohort analysis, export analytics, AI usage & costs, feature adoption
   - Actions available: segment users, export analytics, set custom alerts

4. **Marketing Dashboard**
   - Primary users, purpose
   - Key features: traffic sources, SEO performance, campaign performance, A/B test results, affiliate partner performance, content performance, email campaigns
   - Actions available: pause/activate campaigns, adjust budgets, approve affiliates

5. **Social Media Dashboard**
   - Primary users, purpose
   - Key features: post performance, traffic from social, content calendar, community mentions, social conversions
   - Actions available: schedule posts, respond to mentions, export reports

6. **Security & Compliance Dashboard**
   - Primary users, purpose
   - Key features: access monitoring, API abuse detection, security incidents, data access logs, compliance tracking (GDPR/CCPA), encryption & key management
   - Actions available: block IPs/accounts, approve data deletion requests, export security logs

7. **Admin Dashboard (User Management & Support)**
   - Primary users, purpose
   - Key features: user search, user impersonation, account actions, support tickets, scenario management, usage limits
   - Actions available: impersonate user, upgrade/downgrade, grant free access, ban/suspend

8. **Calculator Management Dashboard**
   - Primary users, purpose
   - Key features: calculator inventory, performance, version management, feature flags, configuration management, deployment status
   - Actions available: deploy new versions, rollback, enable/disable calculators, edit configs

9. **Embed/Shortcode Management Dashboard**
   - Primary users, purpose
   - Key features: embed inventory, shortcode generator, embed performance, access control, white-label configuration, affiliate tracking
   - Actions available: generate shortcodes, revoke access, configure branding

10. **Cross-Dashboard Requirements**
    - Role-based access control
    - Date range filters
    - Export capabilities
    - Alerts & notifications
    - Mobile responsive
    - Data freshness indicators

**Additional Content:**
- Dashboard architecture considerations (hosting, data sources, performance, security)
- Implementation priority (Phase 0: Admin, Calculator Management, API; Phase 1: Subscription, BI, Security; Phase 2: Marketing, Social, Embed)

**Format:**
- Section for each dashboard (10 sections total)
- Each section: Primary users, Purpose, Key features (bullet list), Actions available (bullet list)
- Section: "Cross-Dashboard Requirements" with bullet list
- Section: "Dashboard Architecture Considerations" with 4 subsections (hosting, data sources, performance, security)
- Section: "Implementation Priority" with phased bullet lists

**Length:** 8-10 pages

---

## Overall Guidelines

**Tone:**
- Professional but direct
- No marketing fluff or buzzwords
- Use specific metrics and targets, not vague goals
- Use "must" for requirements, "should" for strong preferences, "can" for options

**Formatting:**
- Use markdown headers: ## for main sections, ### for subsections, #### for sub-subsections
- Use tables where specified
- Use bullet lists for features, requirements, examples
- Use numbered lists only when priority/sequence matters
- Bold key terms on first use

**Consistency:**
- Always use "scenario" not "case" or "model"
- Always show currency with $ symbol or explicitly state
- Always show percentages with % symbol
- Always use "Free tier" not "free plan" or "basic tier"
- Always use "Pro tier" not "premium" or "paid tier"
- Always use "AI add-on" not "AI tier" (unless referring to user who has AI)

**Cross-References:**
- Reference other subsections where relevant (e.g., "As defined in 1.3, CFO-grade means...")
- Use format: "See Section X.Y" or "As described in X.Y"

**Length Target:**
- Total Section 1: 20-25 pages across all 8 files
- Individual file lengths as specified above

**Output Files:**
Create all 8 files in `sections/01-vision-and-goals/`:
- 1.1-product-vision.md
- 1.2-target-users.md
- 1.3-cfo-grade-definition.md
- 1.4-differentiators.md
- 1.5-monetization.md
- 1.6-quality-goals.md
- 1.7-constraints.md
- 1.8-internal-operations.md

Each file should start with a # header matching its title (e.g., "# 1.1 Product Vision")