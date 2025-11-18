# SECTION 9: IMPLEMENTATION PLAN & MILESTONES - BUILD INSTRUCTIONS

## Overview
You are building Section 9 of the CFO Business Intelligence Calculator Suite PDR. This section defines the phased rollout plan, workstream responsibilities, and risk mitigation strategies.

## Context
- Reference Section 2.3 for: phased roadmap (Phase 0-3)
- Reference Section 2.2 for: 8 MVP calculators
- Reference Section 1.6 for: quality goals and SLAs
- Implementation is phased: foundation first, then MVP calculators, then expansion

## Your Task
Create 3 markdown files:
1. `9.1-milestone-breakdown.md`
2. `9.2-workstream-mapping.md`
3. `9.3-risks-mitigations.md`

---

## 9.1 MILESTONE BREAKDOWN

**File:** `9.1-milestone-breakdown.md`

**Content:**
Define implementation milestones with deliverables and timelines.

**Milestones:**

**M0: Platform Baseline (Foundation)**
- Timeline: 8-10 weeks
- Deliverables:
  - Calculation engine architecture and formula library (with versioning, regression test framework)
  - Shared UI component library (React/TypeScript, input fields, result cards, scenario tabs, warning components)
  - Export service (PDF generation with watermarking, CSV generation, basic Excel support)
  - Analytics and telemetry infrastructure (event tracking, Mixpanel or similar integration)
  - WordPress plugin for embedding calculators (shortcode system, iframe embedding, basic configuration)
  - Basic tier gating logic (Free vs Pro checks, upgrade prompt modals)
  - Database schema (scenarios, users, sessions, exports)
  - API structure (REST endpoints for calculate, export, save scenario)
- Success criteria:
  - Formula library can execute test calculations with correct results within tolerance
  - UI components render correctly in WordPress and standalone
  - Exports generate successfully (PDF with watermark, CSV)
  - Events tracked in analytics platform
  - Upgrade prompts trigger correctly for gated features

**M1: First Two Calculators in Production**
- Timeline: 6-8 weeks after M0
- Deliverables:
  - Calculator 1: Business Loan + DSCR (fully implemented with golden scenarios)
  - Calculator 2: Cash Runway & Burn Rate (fully implemented with golden scenarios)
  - Full Free/Pro tier implementation (single scenario for Free, multiple for Pro, all gating working)
  - Basic AI narrative integration (explain DSCR, explain runway, even if limited to these two calculators)
  - Exports working end-to-end (PDF with version stamps, CSV with metadata)
  - WordPress embeds live on Plus One Capital (at least these two calculators embedded)
  - Payment integration (Stripe checkout, webhook handling for subscription events)
  - User authentication (signup, login, session management)
- Success criteria:
  - Both calculators pass all golden scenario tests
  - Free to Pro upgrade flow works end-to-end
  - Exports meet quality standards (board-ready PDFs)
  - AI narratives generate successfully for test scenarios
  - WordPress embeds load within 2 seconds

**M2: Full Phase 1 Set Live (8 Calculators)**
- Timeline: 8-12 weeks after M1
- Deliverables:
  - Remaining 6 MVP calculators deployed:
    - SBA 7(a) Loan Analyzer
    - Equipment Lease vs Buy
    - Breakeven & Contribution Margin
    - Invoice Factoring / AR Financing
    - Line of Credit Utilization
    - Simple Business Valuation
  - AI narratives expanded to all calculators (prompts customized per calculator type)
  - Pro tier upgrade flows optimized (A/B testing on modal messaging, pricing display)
  - B2B/white-label foundation (tenant configuration, basic branding options)
  - Internal dashboards (API Management, Subscription & Billing, Business Intelligence from Section 1.8)
  - SEO optimization (meta tags, sitemap, structured data for all calculator pages)
- Success criteria:
  - All 8 calculators pass golden scenario tests
  - AI narratives working for all calculator types
  - Free to Pro conversion rate tracked and baseline established
  - At least 3 B2B pilot customers onboarded
  - Internal dashboards showing real-time metrics

**M3+: Phase 2 and 3 Expansions**
- Timeline: 12+ weeks after M2 (ongoing)
- Deliverables (Phase 2):
  - Additional calculators per category (SBA 504, customer profitability, project ROI, etc.)
  - Richer AI narratives (scenario comparison commentary, pattern-based suggestions across calculators)
  - Enhanced B2B features (advanced tenant config, theming, more granular feature toggles)
  - Advanced exports (multi-tab Excel, board pack templates combining multiple calculators)
  - Performance optimizations (caching, CDN improvements, database query tuning)
- Deliverables (Phase 3):
  - Cross-calculator scenario linking (loan + runway + margin in one plan)
  - Multi-year planning tools (simplified forecasting)
  - Advanced analytics (cohort retention curves, LTV predictions)
  - Evaluation of multi-currency and ERP integrations (via separate PDR)

**For each milestone:**
- Timeline estimate
- Key deliverables (bullet list with details)
- Success criteria (measurable outcomes)

**Format:** Section per milestone. 4-5 pages.

---

## 9.2 WORKSTREAM MAPPING

**File:** `9.2-workstream-mapping.md`

**Content:**
Define workstreams, ownership, and dependencies.

**Workstreams:**

1. **Calculation Engine & Formula Library**
   - Team/Owner: Backend engineering lead + senior engineer
   - Dependencies: None (foundation for everything else)
   - Key Deliverables:
     - Formula library with versioning
     - Input validation and sanitization
     - Golden scenario test framework
     - Formula documentation and references
   - Timeline: Weeks 1-6 (M0)
   - Risks: Formula errors, performance bottlenecks

2. **UI & UX System**
   - Team/Owner: Frontend engineering lead + designer
   - Dependencies: Calculation engine API contracts
   - Key Deliverables:
     - React component library (inputs, results, scenarios, warnings)
     - Design system (colors, typography, spacing)
     - Responsive layouts (desktop, tablet, mobile)
     - Accessibility compliance (WCAG 2.1 AA)
   - Timeline: Weeks 3-10 (M0, extends into M1)
   - Risks: Design inconsistencies, performance on low-end devices

3. **Export & Reporting**
   - Team/Owner: Backend engineer + designer for templates
   - Dependencies: Calculation engine outputs, branding/watermarking requirements
   - Key Deliverables:
     - PDF generation (with watermarking for Free tier)
     - CSV generation (with metadata headers)
     - Excel generation (basic, single sheet)
     - Export service API (async queue for large exports)
   - Timeline: Weeks 4-8 (M0)
   - Risks: PDF generation slowness, Excel formatting complexity

4. **AI Integration**
   - Team/Owner: Backend engineer + product manager for prompt design
   - Dependencies: Calculation engine outputs, AI provider API access
   - Key Deliverables:
     - Prompt framework (system prompts, user prompt templates)
     - AI provider integration (Anthropic API, error handling, fallback)
     - Response formatting and disclaimers
     - Usage tracking and cost monitoring
   - Timeline: Weeks 8-12 (M1), expanded in M2
   - Risks: AI latency, cost overruns, quality inconsistency

5. **Analytics & Observability**
   - Team/Owner: Backend engineer + data analyst
   - Dependencies: Event taxonomy defined, analytics platform chosen
   - Key Deliverables:
     - Event tracking implementation (all standard events from Section 7.2)
     - Analytics dashboards (funnel, conversion, AI usage, exports)
     - Error monitoring and alerting
     - Performance monitoring (APM integration)
   - Timeline: Weeks 6-10 (M0, ongoing instrumentation in M1/M2)
   - Risks: Data quality issues, alert fatigue

6. **B2B/White-Label Features**
   - Team/Owner: Backend engineer + product manager
   - Dependencies: Core platform stable (post-M1)
   - Key Deliverables:
     - Tenant configuration system (feature flags, branding)
     - API key management and authentication
     - White-label embed options (custom domains, theming)
     - Usage tracking per tenant
   - Timeline: Weeks 16-24 (M2)
   - Risks: Complexity of multi-tenancy, custom requirements from pilots

**For each workstream:**
- Team/Owner
- Dependencies
- Key Deliverables (bullet list)
- Timeline (which milestone phases)
- Risks specific to this workstream

**Format:** Section per workstream. 4-5 pages.

---

## 9.3 RISKS AND MITIGATIONS

**File:** `9.3-risks-mitigations.md`

**Content:**
Identify risks and mitigation strategies.

**Risk categories:**

1. **Technical Risks**

   **Risk: Calculation engine performance below SLAs**
   - Probability: Medium
   - Impact: High (violates p95 < 150ms target)
   - Mitigation:
     - Early load testing during M0 (before building calculators)
     - Implement caching for repeated calculations (5-minute TTL)
     - Optimize formula library (avoid expensive operations, use approximations where acceptable)
     - Plan for horizontal scaling (stateless engine, deploy multiple instances)

   **Risk: Export generation too slow**
   - Probability: Medium
   - Impact: Medium (violates p95 < 3s target)
   - Mitigation:
     - Use async generation for complex exports (progress indicators)
     - Optimize PDF templates (simple layouts, minimal graphics)
     - Consider pre-rendering common export formats
     - Add worker pool for export queue

   **Risk: AI latency or costs exceed budget**
   - Probability: High
   - Impact: Medium (affects AI tier profitability)
   - Mitigation:
     - Implement prompt caching (90% cost reduction on cached prompts)
     - Cache AI responses for identical inputs (1-hour TTL)
     - Hard timeout on AI requests (8 seconds, fallback to "AI unavailable")
     - Monitor costs daily, adjust caps if needed

   **Risk: WordPress plugin compatibility issues**
   - Probability: Medium
   - Impact: Medium (affects primary distribution channel)
   - Mitigation:
     - Test across WordPress versions (current, previous 2 major versions)
     - Use iframe embedding (isolates calculator from WordPress environment)
     - Graceful degradation if JavaScript disabled
     - Clear documentation for site admins

2. **Product Risks**

   **Risk: Calculators too complex for target users**
   - Probability: Medium
   - Impact: High (low adoption, high support burden)
   - Mitigation:
     - User testing before each calculator launch (5-10 users per calculator)
     - Sample scenarios pre-filled for first-time users
     - Comprehensive tooltips and help text
     - "Try it" mode with realistic example data

   **Risk: Low Free to Pro conversion**
   - Probability: Medium
   - Impact: High (revenue target missed)
   - Mitigation:
     - A/B test upgrade prompts (messaging, pricing display, CTA text)
     - Adjust feature gates based on usage data (which features drive upgrades)
     - Offer 14-day trial (reduce friction to trying Pro)
     - Monitor conversion funnel, optimize drop-off points

   **Risk: AI narratives not valuable enough to justify cost**
   - Probability: Medium
   - Impact: Medium (AI tier adoption low)
   - Mitigation:
     - User surveys after AI usage (5-star rating, would you pay for this)
     - Iterate on prompt quality based on feedback
     - Showcase AI in Free tier (1 free AI request to demonstrate value)
     - Consider bundling AI with Pro instead of separate tier

3. **Compliance and Data Handling Risks**

   **Risk: Accidentally sending PII to AI providers**
   - Probability: Low
   - Impact: Critical (GDPR/privacy violation)
   - Mitigation:
     - Strict redaction rules (no free-text by default)
     - Automated checks in code review (block any free-text fields in AI prompts)
     - Logging all AI requests (audit trail of what was sent)
     - Regular security audits

   **Risk: Data retention violates local regulations**
   - Probability: Low
   - Impact: High (compliance issues, fines)
   - Mitigation:
     - Per-tenant retention controls (B2B can set stricter policies)
     - Data residency options (future: EU customers on EU servers)
     - Automated purging schedules (nightly jobs, tested)
     - Legal review of retention policies before launch

   **Risk: Disclaimers insufficient or unclear**
   - Probability: Medium
   - Impact: Medium (liability concerns, user confusion)
     - Mitigation:
     - Legal review of all disclaimers (Section 5.3 text)
     - Prominent placement (footer, exports, AI outputs)
     - User acceptance flow on first use (checkbox acknowledging terms)
     - Clear language testing with non-finance users

**For each risk:**
- Risk description
- Probability (Low, Medium, High)
- Impact (Low, Medium, High, Critical)
- Mitigation strategies (3-5 specific actions)

**Format:** Section per risk category, subsection per risk. 4-5 pages.

---

## Overall Guidelines

**Tone:** Project management focused. Writing for engineering leadership and stakeholders.

**Formatting:**
- Markdown headers: ##, ###, ####
- Bullet lists for deliverables and mitigations
- Tables for workstream timelines if helpful
- Bold for risk names and mitigation strategies

**Consistency:**
- Reference Section 2.3 for phased roadmap
- Reference Section 1.6 for SLAs
- Use consistent milestone names (M0, M1, M2, M3+)
- Use consistent risk levels (Low, Medium, High, Critical)

**Length:** 12-15 pages total across 3 files

**Output Files:**
- 9.1-milestone-breakdown.md
- 9.2-workstream-mapping.md
- 9.3-risks-mitigations.md

Each file starts with # header matching title.