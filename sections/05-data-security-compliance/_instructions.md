# SECTION 5: DATA, SECURITY, AND COMPLIANCE - BUILD INSTRUCTIONS

## Overview
You are building Section 5 of the CFO Business Intelligence Calculator Suite PDR. This section defines data models, security standards, compliance requirements, and data retention policies.

## Context
- Reference Section 1.7 for: constraints (no PII by design, decision-support only)
- Reference Section 1.6 for: data retention defaults
- Reference Section 1.8 for: dashboard data access requirements
- Non-regulated product: no underwriting, no tax filing, no investment advice

## Your Task
Create 4 markdown files:
1. `5.1-data-model.md`
2. `5.2-security-privacy.md`
3. `5.3-compliance-posture.md`
4. `5.4-retention-policies.md`

---

## 5.1 DATA MODEL (SUITE-LEVEL)

**File:** `5.1-data-model.md`

**Content:**
Define core data models used across the suite.

**Models to define:**
1. **User Model**
   - Anonymous users (session ID, created_at, last_seen)
   - Registered users (user_id, email, tier, signup_date, last_login)
   - B2B/tenant users (tenant_id, role within tenant)

2. **Scenario Model**
   - Core fields: scenario_id, calculator_id, user_id/session_id, created_at, updated_at
   - Input data (JSON structure, calculator-specific)
   - Output data (calculated results, warnings)
   - Metadata: scenario name, description, tags, is_baseline
   - Versioning: calculator_version, formula_version

3. **Tenant/Organization Model (B2B)**
   - Tenant config: feature flags, branding settings, usage limits
   - User-to-tenant relationships (roles: admin, user, viewer)
   - Tenant-level usage tracking

4. **Export Model**
   - Export metadata: export_id, scenario_ids, format (PDF/CSV/Excel), created_at
   - File storage references (S3 key or file path)
   - Expiration/retention rules

5. **Analytics Event Model**
   - Event structure: event_name, properties (JSON), timestamp, session_id
   - Session tracking (anonymous vs authenticated)
   - Event aggregation approach

**Include:** JSON schema examples for each model

**Format:** Section per model with schema examples. 4-5 pages.

---

## 5.2 SECURITY & PRIVACY BASELINES

**File:** `5.2-security-privacy.md`

**Content:**
Define security and privacy standards.

**Security areas:**
1. **Authentication and Authorization**
   - Anonymous session management (cookie-based, 30-day expiry)
   - User authentication (OAuth, email/password)
   - Role-based access control (user, admin, tenant admin)
   - API authentication (JWT for web app, API keys for B2B)

2. **Data Encryption**
   - Encryption in transit (TLS 1.2+ for all connections)
   - Encryption at rest (database, file storage)
   - Key management (rotation schedule, access controls)

3. **Privacy and PII Handling**
   - What is PII in this system: email, name (that's it - no SSN, no financial account data)
   - PII exclusion from logs and analytics (use pseudonymous session_id/user_id only)
   - Pseudonymization strategy (hashed identifiers, no reversible links in analytics)
   - User data export (GDPR/CCPA: provide all user data in JSON format)
   - User data deletion (hard delete scenarios, user records, exports)

4. **Input Validation and Sanitization**
   - Client-side validation (basic checks, UX feedback)
   - Server-side validation (never trust client, validate all inputs)
   - SQL injection prevention (parameterized queries, ORMs)
   - XSS prevention (escape all user-provided content)

5. **Rate Limiting and Abuse Prevention**
   - Calculation rate limits: 100 per hour per session (anonymous), 500 per hour (registered)
   - Export rate limits: 10 per day (Free), 100 per day (Pro)
   - AI request rate limits: 0 (Free), 50 per month (AI tier)
   - DDoS protection (Cloudflare or similar)

**Format:** Section per security area. 4-5 pages.

---

## 5.3 COMPLIANCE POSTURE (NON-REGULATED, DECISION-SUPPORT ONLY)

**File:** `5.3-compliance-posture.md`

**Content:**
Define what this product is NOT and compliance requirements.

**Sections:**
1. **Regulatory Non-Goals**
   - Not a licensed lender (no credit decisions, no loan origination)
   - Not providing underwriting decisions (outputs are estimates only)
   - Not providing tax advice (high-level assumptions only)
   - Not providing investment advice (no SEC-regulated recommendations)

2. **Required Disclaimers**
   - "Decision support tool only" disclaimer (where it appears: footer, exports, AI outputs)
   - "Not a substitute for professional advice" disclaimer
   - AI outputs: "Advisory only, not credit advice" disclaimer
   - Exact disclaimer text to use

3. **Terms of Service and Privacy Policy**
   - Key terms to include (limitation of liability, no warranties, user responsibilities)
   - User acceptance flows (checkbox on signup, re-acceptance on major updates)
   - Update notification strategy (email, in-app banner)

4. **Geographic and Jurisdictional Scope**
   - Initial scope: US and Canada only
   - Tax assumptions: effective rates only, no statutory schedules
   - Currency: single currency per scenario (USD or CAD)
   - Future expansion: requires separate PDR per geography

**Include:** Sample disclaimer text

**Format:** Section per compliance area. 3-4 pages.

---

## 5.4 RETENTION POLICIES (CROSS-CALCULATOR)

**File:** `5.4-retention-policies.md`

**Content:**
Define data retention rules.

**Retention rules:**
1. **Scenario Data Retention**
   - Anonymous scenarios: 24 hours (deleted automatically)
   - Saved scenarios (registered users): 24 months default (user can delete anytime)
   - User-initiated deletion: immediate hard delete
   - Automated purging: nightly job for expired data

2. **Telemetry and Log Retention**
   - Analytics events: 12 months (aggregated summaries kept longer)
   - Error logs: 30 days detailed, 12 months aggregated
   - AI request logs: 12 months (redacted, no user content)
   - Audit logs (admin actions): 24 months

3. **Export File Retention**
   - Anonymous exports: 24 hours (download once, then delete)
   - Saved exports (registered users): 90 days
   - Download link expiration: 7 days after generation

4. **Per-Tenant Overrides (B2B)**
   - Tenant config for custom retention (shorter periods allowed)
   - Stricter retention for regulated customers
   - Data residency requirements (EU customers may require EU storage)
   - Opt-out of telemetry (B2B tenants can disable analytics collection)

**Include:** Table summarizing all retention periods

**Format:** Section per retention category. 2-3 pages.

---

## Overall Guidelines

**Tone:** Technical and compliance-focused. Writing for security/legal review.

**Formatting:**
- Markdown headers: ##, ###, ####
- Tables for retention schedules and comparisons
- JSON schema examples in code blocks
- Bullet lists for requirements

**Consistency:**
- Reference Section 1.6 for retention defaults
- Reference Section 1.7 for non-goals
- Use consistent terminology (PII, pseudonymous, retention)

**Length:** 13-16 pages total across 4 files

**Output Files:**
- 5.1-data-model.md
- 5.2-security-privacy.md
- 5.3-compliance-posture.md
- 5.4-retention-policies.md

Each file starts with # header matching title.