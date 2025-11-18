# SECTION 2: PRODUCT SCOPE - BUILD INSTRUCTIONS

## Overview
You are building Section 2 of the CFO Business Intelligence Calculator Suite PDR. This section defines WHAT calculators exist, HOW they're organized, WHAT gets built in which phase, and HOW tiers behave across calculator categories.

## Context
- Reference Section 1 for vision, users, CFO-grade definition, monetization model, and constraints
- This suite contains 8 MVP calculators organized into 5 categories
- Phased rollout: Phase 0 (platform), Phase 1 (8 MVP calculators), Phase 2+ (expansion)
- Tier behavior: Free (limited), Pro (full features), AI (narratives), B2B (white-label)

## Your Task
Create 5 markdown files in this directory:
1. `2.1-calculator-categories.md`
2. `2.2-mvp-calculator-set.md`
3. `2.3-phased-roadmap.md`
4. `2.4-tier-behavior.md`
5. `2.5-out-of-scope.md`

---

## 2.1 CALCULATOR CATEGORIES

**File:** `2.1-calculator-categories.md`

**Content Requirements:**
Define 5 calculator categories. For each category provide:
- Category name
- Purpose (what business problems it solves)
- Primary personas (who uses calculators in this category)
- Typical decision questions (3-4 examples of questions users ask)

**The 5 categories:**

1. **Financing & Lending Intelligence**
   - Purpose: Structure, compare, and stress-test business debt
   - Personas: CFO, controller, lender, broker, SMB owner
   - Questions: "Which loan structure is better?", "What is our DSCR?", "SBA vs conventional?"

2. **Cash Flow & Liquidity Intelligence**
   - Purpose: Understand cash runway, timing gaps, and liquidity risks
   - Personas: CFO, FP&A, founder
   - Questions: "How many months of runway?", "What if revenue drops 20%?", "How much working capital credit do we need?"

3. **Profitability & Pricing Intelligence**
   - Purpose: Analyze margins, breakeven, and pricing structures
   - Personas: CFO, FP&A, revenue leaders, founders
   - Questions: "What is breakeven volume?", "How do discounts affect margin?", "Which products are profitable?"

4. **Valuation & Deal Intelligence**
   - Purpose: Simple, defensible valuation and deal-impact views
   - Personas: Founders, CFOs, investors, buyers
   - Questions: "What is a reasonable valuation range?", "How does new debt affect equity value?", "What does this acquisition do to our leverage?"

5. **Planning & Scenario Intelligence**
   - Purpose: Run what-if planning around growth, costs, and capital allocation
   - Personas: CFO, FP&A, founders, boards
   - Questions: "What happens if we hire 10 people?", "How sensitive are we to rate changes?", "Which scenario balances growth and risk?"

**Additional Requirement:**
- Statement: Each calculator PDR (Section 11) must map to exactly one primary category and optionally one secondary category

**Format:**
- Section for each category (5 sections)
- Each section: Category name (###), Purpose (paragraph), Personas (bullet list), Typical Questions (bullet list)
- Closing statement about calculator mapping requirement

**Length:** 2-3 pages

---

## 2.2 MVP CALCULATOR SET (PHASE 1)

**File:** `2.2-mvp-calculator-set.md`

**Content Requirements:**
Define all 8 MVP Phase 1 calculators. For each calculator provide:
- Calculator name
- Primary category
- Business role (traffic magnet, Pro upgrade driver, AI showcase, B2B demo asset)
- Key functions (3-5 bullet points)
- Tier mapping (what's available in Free, Pro, AI tiers)
- Golden test scenario requirement (state that 2-3 scenarios are required in the calculator-specific PDR)

**The 8 MVP Calculators:**

1. **Business Loan + DSCR Intelligence Calculator**
   - Category: Financing & Lending Intelligence
   - Role: Core workhorse; Pro upgrade driver
   - Key functions:
     - Standard amortization and total cost
     - DSCR and covenant coverage metrics (advanced)
     - 2-3 scenario comparison (rate/term/fees)
   - Tier mapping:
     - Free: single scenario, basic payment and total interest
     - Pro: multi-scenario, DSCR/coverage metrics, covenant headroom, detailed fee breakdowns, exports
     - AI: narrative explaining DSCR, lender perspective, risk commentary

2. **SBA 7(a) Loan Analyzer**
   - Category: Financing & Lending Intelligence
   - Role: SEO/traffic magnet + affiliate driver for SBA lenders/brokers
   - Key functions:
     - SBA-specific fee structures (guarantee fees, closing costs)
     - Comparison against a conventional loan
     - Impact on DSCR and total cost (advanced)
   - Tier mapping:
     - Free: simple SBA payment + total cost vs one comparison
     - Pro: detailed fee breakdown, DSCR, multi-scenario comparison, exports
     - AI: explanation of SBA-specific tradeoffs and lender expectations

3. **Equipment Lease vs Buy Intelligence Calculator**
   - Category: Financing & Lending Intelligence
   - Role: High-intent traffic + affiliate driver for equipment lenders
   - Key functions:
     - Compare total cost of leasing vs financing vs cash purchase
     - After-tax cost (basic assumptions) and impact on cash
     - NPV/IRR of options (advanced)
   - Tier mapping:
     - Free: basic lease vs buy cost comparison
     - Pro: NPV/IRR, after-tax effects, multi-scenario comparison, exports
     - AI: narrative recommendation and risk notes

4. **Cash Runway & Burn Rate Calculator**
   - Category: Cash Flow & Liquidity Intelligence
   - Role: Founders/CFO awareness + AI showcase
   - Key functions:
     - Burn rate calculation
     - Runway in months under base, downside, and upside scenarios
     - Runway charts and sensitivity views (advanced)
   - Tier mapping:
     - Free: single scenario runway
     - Pro: 3-scenario comparison (base/downside/upside) with charts and exports
     - AI: commentary on burnout risk, suggested cost cuts or funding needs

5. **Breakeven & Contribution Margin Analyzer**
   - Category: Profitability & Pricing Intelligence
   - Role: Cross-sell into non-lending use cases
   - Key functions:
     - Breakeven units/revenue
     - Contribution margin and margin of safety (advanced)
     - Scenario comparison for pricing/discount strategies
   - Tier mapping:
     - Free: single scenario breakeven + margin
     - Pro: multiple pricing scenarios, contribution margin and margin of safety charts, exports
     - AI: narrative explaining risks of discounting / low margin

6. **Invoice Factoring / AR Financing Intelligence Calculator**
   - Category: Financing & Lending Intelligence
   - Role: Niche traffic + affiliate driver for factoring providers
   - Key functions:
     - Effective cost of factoring vs line of credit
     - Impact on cash timing and DSCR (advanced)
     - Simple comparison of different factoring terms
   - Tier mapping:
     - Free: factoring cost for one term set
     - Pro: multi-scenario and comparison to alternative financing, DSCR/coverage metrics, exports
     - AI: commentary on when factoring makes sense or is risky

7. **Line of Credit Utilization & Cost Analyzer**
   - Category: Cash Flow & Liquidity Intelligence
   - Role: Upsell/add-on for working capital users
   - Key functions:
     - Cost of using/maintaining a line versus not
     - Utilization patterns and interest cost ranges
     - Impact on DSCR and liquidity (advanced)
   - Tier mapping:
     - Free: simple utilization and cost
     - Pro: multiple utilization patterns, repayment schedules, DSCR/coverage views, exports
     - AI: narrative on healthy vs risky usage

8. **Simple Business Valuation (EBITDA/Revenue Multiple)**
   - Category: Valuation & Deal Intelligence
   - Role: Founders/investor interest + AI narrative driver
   - Key functions:
     - Valuation range from multiples
     - Sensitivity to changes in revenue/EBITDA/multiples (advanced)
   - Tier mapping:
     - Free: single-scenario valuation range
     - Pro: multiple scenarios and visual sensitivity views, exports
     - AI: narrative explaining how valuation might be perceived by buyers

**Format:**
- Introduction paragraph
- Section for each calculator (8 sections)
- Each section: Calculator name (###), bullet list for category/role/functions/tier mapping/golden scenario requirement
- Use consistent structure for all 8

**Length:** 6-8 pages

---

## 2.3 PHASED ROADMAP

**File:** `2.3-phased-roadmap.md`

**Content Requirements:**
Define 4 phases of product development:

**Phase 0 – Platform Foundation**
- Shared calculation engine and formula library
- Base React/TS UI shell with:
  - Input panels, results panel, scenario tabs
  - Warning and explanation components
- Export service for PDFs and CSV/Excel
- Basic analytics and event tracking
- WordPress/shortcode integration for embedding calculators

**Phase 1 – MVP Suite (calculators 1–8)**
- Implement the 8 MVP calculators with:
  - Deterministic formulas and golden test scenarios
  - Free/Pro feature gating (as defined in Section 1.5 and 2.2)
  - Basic AI narrative integration for the AI add-on (even if initially stubbed or limited to a subset of calculators)
- Launch on PlusOneCapital + SmartProfit-style frontends with:
  - SEO-optimized pages
  - Basic onboarding to Pro/AI

**Phase 2 – Depth and Coverage Expansion**
- Add more calculators within existing categories:
  - Additional SBA variations (504, working capital)
  - Deeper profitability tools (customer profitability, project ROI)
  - More detailed cash planning (rolling 13-week cash flow simplifications)
- Expand AI narratives:
  - Richer scenario comparison commentary
  - Pattern-based suggestions across calculators
- Enhance B2B/white-label capabilities:
  - Tenant-level configuration
  - Theming and branding
  - More granular feature toggles

**Phase 3 – Advanced Planning & Scenario Toolkit**
- Cross-calculator scenario sets:
  - Link loan, cash-flow, and profitability views into one "plan"
- Deeper planning tools:
  - Multi-year, high-level 'plan vs downside' views using simplified inputs
- Advanced exports:
  - Multi-tab Excel exports (summary + key tables)
  - Board-ready packs combining several calculators in one document
- Evaluate (via separate PDR) whether multi-currency and limited ERP integrations are justified by demand and risk

**Format:**
- Section for each phase (####)
- Bullet lists under each phase
- Use sub-bullets where appropriate (e.g., under "Add more calculators" list specific calculator types)

**Length:** 3-4 pages

---

## 2.4 TIER BEHAVIOR BY CATEGORY

**File:** `2.4-tier-behavior.md`

**Content Requirements:**
For each of the 5 calculator categories, define standard tier behavior patterns. This shows how Free/Pro/AI tiers should generally work for calculators in each category.

**For each category provide:**
- Category name
- Free tier behavior (what users can do, what's gated)
- Pro tier behavior (what unlocks)
- AI tier behavior (what AI features are available)

**The 5 categories with tier patterns:**

1. **Financing & Lending Intelligence**
   - Free: Single loan/facility scenario, basic payment/total cost metrics
   - Pro: Multiple loan/facility scenarios and side-by-side comparison, DSCR/covenant metrics, fee breakdowns, exports (advanced metrics)
   - AI: Lender-style commentary, highlighting risk thresholds and tradeoffs

2. **Cash Flow & Liquidity Intelligence**
   - Free: Single cash or runway scenario
   - Pro: Multi-scenario runway comparisons and charts (advanced metrics) within individual tools
   - AI: Narrative guidance (e.g., "You have X months runway; at this burn you should consider funding by Y date")

3. **Profitability & Pricing Intelligence**
   - Free: Single price/volume scenario with basic margin and breakeven
   - Pro: Multiple pricing/volume scenarios, contribution analysis, and charts (advanced metrics), plus exports
   - AI: Explanations of discounting risks, margin strategies, and sensitivity

4. **Valuation & Deal Intelligence**
   - Free: Basic valuation range from simple inputs
   - Pro: Scenario comparison (different multiples and performance cases), plus sensitivity views (advanced metrics) and exports
   - AI: Narrative describing how different scenarios might be perceived by buyers/lenders

5. **Planning & Scenario Intelligence**
   - Free: Single scenario snapshots within individual planners
   - Pro: Multiple scenarios within a single planning tool (e.g., plan vs downside) with comparisons and charts (advanced metrics)
   - AI: Planning narratives and suggestions, highlighting where risk or upside is concentrated
   - Note: Cross-calculator linked scenario sets (e.g., combined loan + runway + margin plans) are Phase 3+ features and not a v1 commitment; cross-calculator AI narratives are later-phase features

**Format:**
- Introduction paragraph explaining purpose
- Section for each category (5 sections)
- Each section: Category name (###), then subsections for Free/Pro/AI (####)
- Bullet lists under each tier describing behavior

**Length:** 3-4 pages

---

## 2.5 OUT-OF-SCOPE CALCULATORS FOR V1

**File:** `2.5-out-of-scope.md`

**Content Requirements:**
List calculator types that are intentionally out-of-scope for the initial suite. For each out-of-scope type, provide brief reasoning.

**Out-of-scope calculator types:**
- Detailed tax calculators (entity-specific corporate tax optimization, sales/use tax, multi-jurisdiction tax engines)
  - Reason: Requires legal/tax expertise, regulatory risk, multi-jurisdiction complexity beyond initial US/CA scope
- Payroll-specific calculators tied to hourly/time-tracking data
  - Reason: Different data model (employee-level vs scenario-level), integrations out of v1 scope
- Full-blown budgeting systems with month-by-month line-item planning
  - Reason: Too granular, conflicts with "aggregated scenarios only" constraint from Section 1.7
- ERP or GL-integrated tools that ingest detailed accounting exports
  - Reason: No automated accounting/ERP imports in v1 per Section 1.7
- Industry-specific deep models (e.g., hospital cost accounting, complex project finance) beyond simple aggregated inputs
  - Reason: Requires domain-specific expertise, limited to niche markets, phase 2+ consideration

**Additional Content:**
- Closing statement: These can be revisited once the core platform and MVP calculators are shipping and stable
- Process: Any of these require a separate PDR and must be justified by demand and resource availability

**Format:**
- Introduction paragraph
- Bullet list of out-of-scope types with sub-bullets for reasoning
- Closing paragraph on process for reconsidering

**Length:** 1-2 pages

---

## Overall Guidelines

**Tone:**
- Professional but direct
- No marketing fluff
- Specific examples and metrics
- Use "must" for requirements, "should" for strong preferences, "can" for options

**Formatting:**
- Markdown headers: ## for main sections, ### for subsections, #### for sub-subsections
- Tables only if explicitly requested (none in this section)
- Bullet lists for features, requirements, examples
- Numbered lists only when priority/sequence matters
- Bold key terms on first use

**Consistency:**
- Always use "scenario" not "case" or "model"
- Always use "Free tier" not "free plan"
- Always use "Pro tier" not "premium"
- Always use "AI add-on" or "AI tier" consistently
- Always use "advanced metrics" when referring to Pro-gated features
- Reference Section 1 where appropriate (e.g., "As defined in Section 1.5" or "Per Section 1.7 constraints")

**Cross-References:**
- Reference Section 1 subsections where relevant
- Use format: "See Section X.Y" or "As described in Section X.Y"
- When mentioning constraints, reference Section 1.7
- When mentioning monetization, reference Section 1.5
- When mentioning quality goals, reference Section 1.6

**Length Target:**
- Total Section 2: 15-20 pages across all 5 files
- Individual file lengths as specified above

**Output Files:**
Create all 5 files in `sections/02-product-scope/`:
- 2.1-calculator-categories.md
- 2.2-mvp-calculator-set.md
- 2.3-phased-roadmap.md
- 2.4-tier-behavior.md
- 2.5-out-of-scope.md

Each file should start with a # header matching its title (e.g., "# 2.1 Calculator Categories")