# SECTION 6: TIERING, PRICING, AND PACKAGING - BUILD INSTRUCTIONS

## Overview
You are building Section 6 of the CFO Business Intelligence Calculator Suite PDR. This section defines how to implement the tiered monetization model, feature flags, and upgrade flows.

## Context
- Reference Section 1.5 for: tier definitions, advanced metrics, business goals
- Reference Section 2.4 for: tier behavior by category
- Must implement: Free/Pro/AI tiers with clear upgrade paths

## Your Task
Create 3 markdown files:
1. `6.1-tier-definitions.md`
2. `6.2-feature-flagging.md`
3. `6.3-upgrade-flows.md`

---

## 6.1 TIER DEFINITIONS (IMPLEMENTATION DETAIL)

**File:** `6.1-tier-definitions.md`

**Content:**
Define each tier with implementation-level detail.

**For each tier provide:**
- Feature checklist (what's included)
- Usage limits (specific numbers)
- Technical gates (what code checks for access)
- Pricing (if applicable)

**The 4 tiers:**

1. **Free Tier**
   - Features:
     - Single active scenario per calculator
     - Key metrics only (no advanced metrics)
     - Watermarked exports (PDF with "Free Tier" footer, CSV with header row)
     - Ads allowed (banner placement, affiliate links)
   - Usage limits:
     - 100 calculations per day per session
     - 10 exports per month per session
     - No saved scenarios (session-only, 24 hour expiry)
   - Upgrade triggers:
     - Attempting to create second scenario → Pro prompt
     - Clicking locked advanced metric → Pro prompt
     - Requesting clean export → Pro prompt
     - Any AI request → AI prompt

2. **Pro Tier**
   - Features:
     - Multiple saved scenarios (up to 50 per calculator)
     - All metrics (key + advanced)
     - Clean exports (no watermarks)
     - Scenario sharing (sharable links, outputs-only option)
     - Prepared by / reviewed by metadata
   - Usage limits:
     - 1000 calculations per day
     - 100 exports per month
     - 50 scenarios per calculator
   - Pricing: $29/month or $290/year (save 17%)
   - Trial: 14 days free

3. **AI Add-On**
   - Features:
     - All Pro features
     - AI narratives (explain results, risk commentary)
     - AI scenario suggestions
   - Usage limits:
     - 50 AI requests per month (hard cap)
     - When cap hit: show "You've used all AI requests this month. Upgrade or wait until [reset date]"
   - Pricing: +$20/month on top of Pro (total $49/month)
   - No separate trial (try with Pro trial)

4. **B2B / White-Label**
   - Features:
     - All AI features
     - Tenant configuration (branding, feature toggles)
     - API access with keys
     - Custom domain support
     - Priority support
   - Pricing models:
     - Per-seat: $39/user/month (min 5 seats)
     - Per-domain: $500/month unlimited users
     - Usage-based: $0.10 per calculation (min $200/month)
   - Volume tiers: 10% off >50 seats, 20% off >100 seats

**Include:** Comparison table of all 4 tiers

**Format:** Section per tier, then comparison table. 3-4 pages.

---

## 6.2 FEATURE FLAGGING STRATEGY

**File:** `6.2-feature-flagging.md`

**Content:**
Define how feature flags work across the suite.

**Feature flag system:**
1. **Suite-Level Feature Flags**
   - Flag naming: `suite_feature_name` (e.g., `suite_ai_enabled`, `suite_exports_enabled`)
   - Flag types:
     - Boolean (on/off)
     - Percentage rollout (e.g., 10% of users see new UI)
     - User segment (e.g., only Pro users)
   - Flag management: Use LaunchDarkly, Flagsmith, or custom database table

2. **Per-Calculator Feature Flags**
   - Flag naming: `calc_{slug}_feature_name` (e.g., `calc_loan_dscr_advanced_charts`)
   - Use case: Gradual rollout of calculator-specific features
   - Example: "New DSCR chart only for 25% of Pro users to test performance"

3. **Tenant-Level Overrides (B2B)**
   - Tenant config schema:
```json
     {
       "tenant_id": "acme_corp",
       "features": {
         "ai_enabled": false,
         "max_scenarios": 100,
         "custom_branding": true
       },
       "tier_overrides": {
         "all_users_pro": true
       }
     }
```
   - Where stored: Database table `tenant_configs`
   - Priority: Tenant config > User tier > Default tier

**Feature flag decision tree:**
- Check: Is there a tenant config? → Use tenant settings
- Else: Check user tier → Apply tier rules
- Else: Apply Free tier defaults

**Include:** Code examples of flag checks (pseudocode)

**Format:** Section per flag type. 2-3 pages.

---

## 6.3 UPSELL & UPGRADE FLOWS (GLOBAL)

**File:** `6.3-upgrade-flows.md`

**Content:**
Define upgrade triggers, modal design, payment integration, and post-upgrade experience.

**Sections:**

1. **Upgrade Triggers**
   - Free → Pro triggers:
     - Click "Add Scenario" button → Modal: "Unlock multiple scenarios with Pro"
     - Click locked advanced metric → Modal: "Unlock advanced metrics with Pro"
     - Click "Export" for clean version → Modal: "Get clean exports with Pro"
   - Pro/Free → AI triggers:
     - Click "Explain this result" → Modal: "Unlock AI insights with AI add-on"
     - See "Get AI suggestions" teaser → Click → AI modal

2. **Upgrade Modal Design**
   - Modal structure:
     - Headline: "Unlock [Feature] with [Tier]"
     - Features list: 3-5 bullet points of what they get
     - Pricing: Clear price with any discount (annual save 17%)
     - CTA button: "Upgrade to Pro" or "Start Free Trial"
     - Dismiss: "Not now" link (closes modal, no penalty)
   - Messaging per trigger:
     - Second scenario: "Compare unlimited scenarios side-by-side"
     - Advanced metrics: "Get CFO-grade metrics like DSCR and coverage ratios"
     - Clean export: "Create board-ready reports without watermarks"
     - AI: "Get plain-English explanations and risk insights"

3. **Payment Integration**
   - Payment processor: Stripe
   - Checkout flow:
     - In-app modal with Stripe Checkout embed OR
     - Redirect to hosted Stripe page
   - Subscription management:
     - Upgrade: Immediate (prorate current period)
     - Downgrade: At end of current period
     - Cancel: At end of current period, no refund
   - Webhook handling:
     - `payment_intent.succeeded` → Activate Pro immediately
     - `payment_intent.failed` → Show retry message
     - `customer.subscription.deleted` → Downgrade to Free

4. **Post-Upgrade Experience**
   - Immediate unlocking:
     - Close modal, refresh tier status
     - Show "You're now Pro!" success message
     - Unlock all gated features immediately
   - Onboarding (optional):
     - Tooltip tour: "Here's what's new" (point to advanced metrics, scenario tabs)
     - Dismissible, shown once
   - Email confirmation:
     - Subject: "Welcome to [Tier]"
     - Receipt with pricing
     - Link to manage subscription

**Include:** Modal wireframe (ASCII or described in detail)

**Format:** Section per upgrade area. 4-5 pages.

---

## Overall Guidelines

**Tone:** Product/implementation focused. Writing for product managers and engineers.

**Formatting:**
- Markdown headers: ##, ###, ####
- Tables for tier comparisons
- JSON examples for configs
- Code blocks for pseudocode
- Bullet lists for features

**Consistency:**
- Reference Section 1.5 for tier definitions
- Use consistent tier names (Free tier, Pro tier, AI add-on, B2B)
- Use consistent pricing ($29/month, $290/year)

**Length:** 9-12 pages total across 3 files

**Output Files:**
- 6.1-tier-definitions.md
- 6.2-feature-flagging.md
- 6.3-upgrade-flows.md

Each file starts with # header matching title.