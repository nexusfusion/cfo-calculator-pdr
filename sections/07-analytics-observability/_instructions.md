# SECTION 7: ANALYTICS & OBSERVABILITY - BUILD INSTRUCTIONS

## Overview
You are building Section 7 of the CFO Business Intelligence Calculator Suite PDR. This section defines analytics event tracking, dashboards, and monitoring standards across the suite.

## Context
- Reference Section 1.6 for: performance SLAs (p95 targets, error thresholds)
- Reference Section 1.8 for: internal dashboard requirements
- Reference Section 5.2 for: privacy (pseudonymous IDs, no PII in logs)
- Must track: usage, conversions, errors, performance

## Your Task
Create 4 markdown files:
1. `7.1-event-taxonomy.md`
2. `7.2-standard-events.md`
3. `7.3-suite-dashboards.md`
4. `7.4-error-performance-monitoring.md`

---

## 7.1 CORE EVENT TAXONOMY

**File:** `7.1-event-taxonomy.md`

**Content:**
Define how events are named and structured.

**Event naming system:**
1. **Event Naming Conventions**
   - Format: category_action (e.g., calculator_viewed, export_generated)
   - Allowed categories: calculator, scenario, export, ai, upgrade, user, admin
   - Allowed actions: viewed, calculated, created, updated, deleted, requested, generated, downloaded, clicked, shown, completed
   - Examples:
     - calculator_viewed - User opened a calculator
     - scenario_created - User created a new scenario
     - export_downloaded - User downloaded an export file
     - upgrade_prompt_shown - Upgrade modal displayed
     - ai_narrative_requested - User clicked "Explain this result"

2. **Required Event Properties**
   - Every event MUST include:
     - session_id - Anonymous or user session identifier
     - user_id - If authenticated (null for anonymous)
     - tier - Current user tier (free, pro, ai, b2b)
     - calculator_slug - Which calculator (e.g., "business-loan-dscr")
     - scenario_count - How many scenarios active in this calculator
     - timestamp - ISO 8601 format (e.g., "2025-11-17T21:30:00Z")

3. **Optional Event Properties**
   - Include when relevant:
     - tenant_id - For B2B users
     - ab_test_variant - For A/B testing (e.g., "new_ui_v2")
     - referrer - Traffic source (organic, paid, affiliate, direct)
     - export_format - PDF, CSV, Excel
     - ai_request_type - explain_result, suggest_scenario
     - error_code - For error events

**Event payload structure:**
Include a JSON example showing event_name and properties object with all required fields plus optional ones

**Format:** Section per naming rule, JSON examples. 3-4 pages.

---

## 7.2 STANDARD EVENTS FOR ALL CALCULATORS

**File:** `7.2-standard-events.md`

**Content:**
Define the core events every calculator must track.

**Standard events (all calculators):**

1. **calculator_viewed**
   - When: Calculator page loads
   - Properties: session_id, user_id, tier, calculator_slug, referrer, timestamp
   - Purpose: Traffic tracking, funnel entry

2. **calculator_calculated**
   - When: User triggers calculation (inputs to outputs)
   - Properties: session_id, user_id, tier, calculator_slug, scenario_count, timestamp
   - Purpose: Usage tracking, engagement

3. **scenario_created**
   - When: User creates a new scenario (Pro tier only)
   - Properties: session_id, user_id, tier, calculator_slug, scenario_count, timestamp
   - Purpose: Pro feature usage

4. **scenario_deleted**
   - When: User deletes a scenario
   - Properties: session_id, user_id, tier, calculator_slug, scenario_count, timestamp

5. **export_requested**
   - When: User clicks export button
   - Properties: session_id, user_id, tier, calculator_slug, scenario_count, export_format, timestamp
   - Purpose: Export funnel (request to generated to downloaded)

6. **export_generated**
   - When: Export file is successfully created
   - Properties: session_id, user_id, tier, calculator_slug, export_format, generation_time_ms, timestamp
   - Purpose: Performance tracking

7. **export_downloaded**
   - When: User downloads the export file
   - Properties: session_id, user_id, tier, calculator_slug, export_format, timestamp
   - Purpose: Export completion rate

8. **ai_narrative_requested**
   - When: User clicks "Explain this result" or similar
   - Properties: session_id, user_id, tier, calculator_slug, ai_request_type, timestamp
   - Purpose: AI usage tracking

9. **ai_narrative_displayed**
   - When: AI response is shown to user
   - Properties: session_id, user_id, tier, calculator_slug, ai_request_type, response_time_ms, timestamp
   - Purpose: AI performance, success rate

10. **upgrade_prompt_shown**
    - When: Upgrade modal displays
    - Properties: session_id, user_id, tier, calculator_slug, trigger_reason, timestamp
    - Purpose: Conversion funnel tracking

11. **upgrade_prompt_clicked**
    - When: User clicks "Upgrade" button in modal
    - Properties: session_id, user_id, tier, target_tier, calculator_slug, trigger_reason, timestamp
    - Purpose: Conversion click-through

12. **upgrade_completed**
    - When: User successfully upgrades (payment confirmed)
    - Properties: session_id, user_id, previous_tier, new_tier, timestamp
    - Purpose: Conversion completion

**For each event, provide:**
- Event name
- Trigger condition
- Required properties
- Purpose and usage

**Format:** Section per event. 4-5 pages.

---

## 7.3 SUITE DASHBOARDS

**File:** `7.3-suite-dashboards.md`

**Content:**
Define analytics dashboards for business metrics.

**Dashboards to define:**

1. **Calculator Funnel Dashboard**
   - Metrics:
     - Views to Calculations to Exports (conversion rates at each step)
     - Per-calculator breakdown
     - Drop-off points (where users abandon)
   - Filters: Date range, tier, calculator, referrer
   - Charts: Funnel chart, line chart for trends, bar chart per calculator

2. **Tier Conversion Dashboard**
   - Metrics:
     - Free to Pro conversion rate (percentage)
     - Pro to AI conversion rate (percentage)
     - Upgrade prompt effectiveness (clicks divided by shows)
     - Trial-to-paid conversion rate
     - Time to conversion (days from signup)
   - Filters: Date range, calculator, trigger reason
   - Charts: Conversion funnel, cohort retention, trigger breakdown

3. **AI Usage Dashboard**
   - Metrics:
     - AI requests per tier (total, per user)
     - AI request success rate (displayed divided by requested)
     - AI request latency (p50, p95, p99)
     - AI cost per request, total cost per month
     - Most popular AI request types
   - Filters: Date range, tier, calculator
   - Charts: Usage over time, success rate, latency distribution

4. **Export Dashboard**
   - Metrics:
     - Export volume per tier (Free vs Pro)
     - Export format preferences (example: PDF 60%, CSV 30%, Excel 10%)
     - Export generation time (p50, p95, p99)
     - Export failure rate (percentage)
     - Top exported calculators
   - Filters: Date range, tier, format, calculator
   - Charts: Volume over time, format pie chart, latency distribution

5. **Scenario Usage Dashboard**
   - Metrics:
     - Average scenarios per session (example: Free 1, Pro 2.3)
     - Scenario save rate (percentage of sessions that save)
     - Scenario comparison usage (percentage of Pro users using 2+ scenarios)
     - Scenarios per calculator (which calculators drive multi-scenario usage)
   - Filters: Date range, tier, calculator
   - Charts: Distribution histogram, per-calculator bar chart

**For each dashboard:**
- Purpose
- Key metrics with example values
- Filters available
- Chart types

**Format:** Section per dashboard. 4-5 pages.

---

## 7.4 ERROR AND PERFORMANCE MONITORING

**File:** `7.4-error-performance-monitoring.md`

**Content:**
Define error tracking, performance monitoring, and alerting.

**Monitoring areas:**

1. **Error Tracking**
   - Calculation engine errors (formula failures, edge cases causing NaN or infinity)
   - Export generation errors (PDF/CSV generation failures, timeout errors)
   - AI service errors (timeouts, rate limits, API failures from provider)
   - Client-side errors (JavaScript exceptions, network failures, timeout errors)
   - What gets logged: error type, error message, stack trace, user context (session_id, tier, calculator_slug)

2. **Performance Monitoring**
   - Calculation latency (p50, p95, p99 per calculator) - target p95 less than 150ms
   - Export generation time (p50, p95, p99 per format) - target p95 less than 3 seconds
   - AI request latency (p50, p95, p99) - target p95 1-3 seconds
   - Page load time (first contentful paint, time to interactive)
   - Database query times (slow query log for queries over 100ms)

3. **Alert Thresholds**
   - Error rate greater than 5% in 5 minutes - Critical alert
   - p95 calculation latency greater than 500ms - Warning alert
   - p95 export time greater than 6 seconds - Warning alert
   - AI failure rate greater than 10% - Warning alert
   - AI p95 latency greater than 8 seconds - Warning alert
   - Database connection pool exhausted - Critical alert

4. **Monitoring Tools**
   - APM tool: Datadog, New Relic, or similar for application performance
   - Log aggregation: Elasticsearch, CloudWatch Logs, or similar
   - Analytics platform: Mixpanel, Amplitude, or custom data warehouse
   - Uptime monitoring: Pingdom, UptimeRobot for external health checks
   - Alert routing: PagerDuty, Opsgenie, or Slack for critical alerts

**For each area:**
- What gets monitored
- How it's tracked
- Alert thresholds
- Tools used

**Format:** Section per monitoring area. 3-4 pages.

---

## Overall Guidelines

**Tone:** Technical and data-focused. Writing for data analysts and engineers.

**Formatting:**
- Markdown headers: ##, ###, ####
- Tables for event properties and dashboard metrics
- Bullet lists for requirements
- Example values in parentheses

**Consistency:**
- Reference Section 1.6 for SLAs
- Reference Section 5.2 for privacy requirements
- Use consistent event naming (calculator_viewed not CalculatorViewed)
- Use consistent property names (session_id not sessionId)

**Length:** 14-17 pages total across 4 files

**Output Files:**
- 7.1-event-taxonomy.md
- 7.2-standard-events.md
- 7.3-suite-dashboards.md
- 7.4-error-performance-monitoring.md

Each file starts with # header matching title.