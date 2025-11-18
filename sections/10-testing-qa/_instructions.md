# SECTION 10: TESTING, QA, AND RELEASE MANAGEMENT - BUILD INSTRUCTIONS

## Overview
You are building Section 10 of the CFO Business Intelligence Calculator Suite PDR. This section defines testing standards, the golden scenario framework, release processes, and ongoing support/maintenance.

## Context
- Reference Section 1.6 for: quality goals (95% accuracy, regression safety)
- Reference Section 2.2 for: golden scenario requirement (2-3 per calculator)
- Reference Section 3 for: architecture (stateless engine, modular components)
- Must ensure: formula correctness, performance, and regression safety

## Your Task
Create 4 markdown files:
1. `10.1-test-strategy.md`
2. `10.2-golden-scenarios.md`
3. `10.3-release-process.md`
4. `10.4-support-maintenance.md`

---

## 10.1 TEST STRATEGY (SUITE-LEVEL)

**File:** `10.1-test-strategy.md`

**Content:**
Define testing approach across all levels.

**Testing levels:**

1. **Unit Testing**
   - Scope: Individual functions and components
   - Coverage targets:
     - Formula library: 100% coverage (every formula, every edge case)
     - Input validation: 100% coverage
     - Formatting utilities: 100% coverage
     - UI components: 80% coverage minimum
   - Tools: Jest for JavaScript/TypeScript, pytest for Python (if backend uses Python)
   - Examples of unit tests:
     - Test amortization formula with standard inputs
     - Test DSCR calculation with zero debt service (should handle gracefully)
     - Test input validation rejects negative loan amounts
     - Test currency formatter handles large numbers correctly

2. **Integration Testing**
   - Scope: Multiple components working together
   - Test areas:
     - API endpoints (calculate, export, save scenario, AI request)
     - Database read/write operations (save scenario, retrieve scenario)
     - Tier gating logic (Free user attempts Pro feature, upgrade prompt shown)
     - Export generation (inputs to PDF/CSV/Excel output)
   - Tools: Supertest for API testing, integration test framework
   - Examples of integration tests:
     - POST to /api/calculate returns correct results within 200ms
     - Save scenario as Pro user succeeds, as Free user fails with 403
     - Export PDF includes watermark for Free tier, no watermark for Pro
     - AI request returns narrative within 5 seconds or timeout error

3. **End-to-End Testing**
   - Scope: Complete user workflows
   - Test scenarios:
     - Free tier workflow: View calculator, enter inputs, see results, attempt export (blocked), see upgrade prompt
     - Pro tier workflow: Create multiple scenarios, compare side-by-side, export clean PDF
     - AI tier workflow: Request AI explanation, receive narrative, stay within monthly cap
     - Upgrade workflow: Click upgrade, complete Stripe checkout, features unlock immediately
   - Tools: Playwright or Cypress for browser automation
   - Cross-browser: Test on Chrome, Safari, Firefox, Edge
   - Mobile responsive: Test on tablet and mobile viewports

4. **Performance and Load Testing**
   - Scope: Verify SLAs under load
   - Test scenarios:
     - Calculation engine load: 100 concurrent requests, verify p95 < 150ms
     - Export service load: 50 concurrent PDF generations, verify p95 < 3s
     - AI service load: 20 concurrent AI requests, verify p95 < 3s (or timeout gracefully)
   - Tools: k6, Apache JMeter, or Artillery for load testing
   - Run before each major release

**For each testing level:**
- Scope
- Coverage targets or test scenarios
- Tools used
- Example tests

**Format:** Section per testing level. 4-5 pages.

---

## 10.2 GOLDEN SCENARIO FRAMEWORK

**File:** `10.2-golden-scenarios.md`

**Content:**
Define the golden scenario system for regression testing.

**Golden scenario requirements:**

1. **Golden Scenario Requirements (Per Calculator)**
   - Each calculator MUST define 2-3 golden scenarios
   - Each scenario MUST have:
     - Scenario name and description (e.g., "Typical SBA Loan - Restaurant")
     - Fully specified inputs (all required fields, specific values)
     - Expected outputs with tolerances (e.g., "Monthly payment: $2,958 ± $1", "DSCR: 1.42 ± 0.01")
     - Expected warnings (if any should appear)
   - Purpose: Catch regressions when formulas or logic changes

2. **Golden Scenario Structure**
   - Storage format: JSON files in /tests/golden-scenarios/
   - File naming: {calculator_slug}_golden_{scenario_number}.json
   - Example structure:
```
     {
       "calculator_slug": "business-loan-dscr",
       "scenario_name": "Typical SBA Loan - Restaurant",
       "description": "Standard SBA 7(a) loan for restaurant equipment purchase",
       "inputs": {
         "loan_amount": 250000,
         "interest_rate": 7.5,
         "term_years": 10,
         "annual_revenue": 1500000,
         "annual_expenses": 1200000
       },
       "expected_outputs": {
         "monthly_payment": {
           "value": 2958,
           "tolerance": 1
         },
         "total_interest": {
           "value": 104960,
           "tolerance": 100
         },
         "dscr": {
           "value": 1.42,
           "tolerance": 0.01
         }
       },
       "expected_warnings": [
         {
           "type": "info",
           "message_contains": "DSCR is above typical lender minimum"
         }
       ]
     }
```

3. **Regression Testing Policy**
   - When to run golden scenarios:
     - Every formula change (before merge)
     - Every release (automated in CI/CD)
     - On-demand when investigating issues
   - Pass/fail criteria:
     - All outputs within tolerance: PASS
     - Any output outside tolerance: FAIL (blocks release)
     - Missing expected warning: FAIL
     - Unexpected warning: WARN (review required, may or may not block)
   - On intentional output changes:
     - Update golden scenario file
     - Document reason in commit message
     - Get approval from product manager

4. **Golden Scenario Repository**
   - Where stored: Version control (Git), in /tests/golden-scenarios/
   - How to add new scenarios:
     - Create JSON file following structure above
     - Run scenario manually, verify outputs are correct
     - Add to automated test suite
     - Document in calculator PDR (Section 11)
   - Automated execution:
     - CI/CD pipeline runs all golden scenarios on every commit
     - Test runner compares actual vs expected outputs
     - Reports any tolerance violations
     - Blocks merge if failures

**For each subsection:**
- Requirements or structure
- Examples (JSON or pseudocode)
- Policies and procedures

**Format:** Section per golden scenario topic. 3-4 pages.

---

## 10.3 RELEASE PROCESS

**File:** `10.3-release-process.md`

**Content:**
Define versioning, rollout strategy, and release procedures.

**Release process:**

1. **Versioning Strategy**
   - Suite version: Major.Minor.Patch (e.g., 1.2.3)
     - Major: Breaking changes, major feature additions
     - Minor: New calculators, new features (backward compatible)
     - Patch: Bug fixes, performance improvements
   - Calculator version: Independent per calculator (e.g., business-loan-dscr v2.1.0)
     - Version stored in calculator config
     - Displayed in exports and UI footer
   - Formula library version: Semantic versioning (e.g., formulas v1.3.2)
     - Version stamped in all calculation outputs
     - Tracked separately from suite version
   - Version stamping in exports:
     - PDF footer: "Generated by CFO Calculator Suite v1.2.3 | Calculator: business-loan-dscr v2.1.0 | Formulas: v1.3.2 | 2025-11-18"
     - CSV header row: "# Generated by CFO Calculator Suite v1.2.3, business-loan-dscr v2.1.0, Formulas v1.3.2, 2025-11-18"

2. **Rollout Strategy**
   - Feature flags for gradual rollout:
     - New calculator: Start at 10% of traffic, monitor for errors, increase to 50%, then 100%
     - New formula version: Canary to internal users first, then 10%, 50%, 100%
     - New UI feature: A/B test with 50/50 split, measure conversion impact
   - Canary deployments:
     - Deploy to staging environment first (internal testing)
     - Deploy to 5-10 canary tenants (B2B early access partners)
     - Monitor error rates, performance metrics for 24-48 hours
     - If stable, roll out to all users
   - Rollback plan:
     - Feature flag can disable new calculator instantly
     - Database migrations are reversible
     - Previous version deployable within 15 minutes
     - If error rate spikes above 5%, automatic rollback triggered

3. **Release Checklist**
   - Pre-release (before deployment):
     - [ ] All unit tests passing (100% for formulas)
     - [ ] All golden scenarios passing (within tolerance)
     - [ ] Integration tests passing
     - [ ] End-to-end tests passing
     - [ ] Performance benchmarks met (p95 < 150ms calc, < 3s exports)
     - [ ] Security review completed (if code touches auth, payments, or data handling)
     - [ ] Documentation updated (calculator PDRs, API docs, changelog)
     - [ ] Version numbers incremented in all relevant files
   - Post-release (after deployment):
     - [ ] Monitor error rate for first 1 hour (< 1% threshold)
     - [ ] Check performance metrics (p95 latency within SLAs)
     - [ ] Verify new features accessible to correct tiers
     - [ ] Test one complete user workflow (view, calculate, export)
     - [ ] Update internal team on Slack/email with release notes
     - [ ] Publish changelog to users (in-app banner, blog post for major releases)

4. **Post-Release Monitoring**
   - First 24 hours:
     - Error rate monitored every hour
     - Performance metrics (latency, throughput) checked every 2 hours
     - User feedback channels monitored (support tickets, in-app feedback)
   - First week:
     - Daily review of key metrics (error rate, conversion rate, AI usage)
     - Identify any unexpected behavior or user confusion
     - Plan hotfix if critical issues found
   - Success criteria:
     - Error rate < 2% (below baseline)
     - p95 latency within SLAs
     - No critical bugs reported
     - Conversion rate stable or improved (for releases affecting upgrade flow)

**For each subsection:**
- Process or policy
- Examples or checklists
- Success criteria

**Format:** Section per release topic. 4-5 pages.

---

## 10.4 SUPPORT & MAINTENANCE

**File:** `10.4-support-maintenance.md`

**Content:**
Define ongoing support, bug triage, deprecation, and formula updates.

**Support and maintenance:**

1. **Bug Triage and Prioritization**
   - Severity levels:
     - **Critical (P0):** System down, data loss, security breach, payment failures
       - Response time: Immediate (within 1 hour)
       - Fix time: Within 4 hours or rollback
       - Examples: API completely down, all exports failing, login broken
     - **High (P1):** Major feature broken, affects many users, no workaround
       - Response time: Within 4 hours
       - Fix time: Within 24 hours
       - Examples: One calculator completely broken, exports fail for Pro users, AI timeouts for all requests
     - **Medium (P2):** Feature partially broken, affects some users, workaround exists
       - Response time: Within 1 business day
       - Fix time: Within 1 week
       - Examples: One specific scenario fails, export formatting wrong, tooltip text incorrect
     - **Low (P3):** Minor bug, cosmetic issue, feature request
       - Response time: Within 3 business days
       - Fix time: Next release cycle
       - Examples: Button alignment off, typo in warning message, nice-to-have feature
   - Escalation process:
     - P0/P1 bugs page on-call engineer immediately
     - P2 bugs assigned to next sprint
     - P3 bugs triaged monthly, prioritized by impact and effort

2. **Deprecation Policy**
   - Calculator deprecation:
     - Minimum notice period: 90 days
     - Migration path provided (alternative calculator, export old scenarios)
     - Deprecated calculator marked in UI: "This calculator will be retired on [date]. Use [alternative] instead."
     - After retirement: Calculator removed from UI, API endpoints return 410 Gone
   - Metric deprecation:
     - Minimum notice period: 60 days
     - Replacement metric provided (or clear explanation why removed)
     - Old metric still calculated but marked "Deprecated" in UI
     - Exports include both old and new metrics during transition
   - API version deprecation:
     - API versioned (e.g., /v1/calculate, /v2/calculate)
     - Old version supported for minimum 12 months after new version released
     - Deprecation warnings in API responses (HTTP header: X-API-Deprecated: true, X-API-Sunset: 2026-11-18)
     - Documentation updated with migration guide

3. **Formula Updates and Validation**
   - When formulas can be updated:
     - Bug fixes (formula produces incorrect result): Immediate fix, golden scenarios updated
     - Formula improvements (better accuracy, new methodology): Requires validation and notice
     - Formula changes for regulatory reasons: Requires legal review and immediate implementation
   - Validation process:
     - Update golden scenarios to reflect new expected outputs
     - Run all golden scenarios, verify new formula passes
     - Compare new vs old outputs for sample scenarios (document differences)
     - Get approval from product manager and finance SME
     - Update formula library version number
     - Document change in changelog
   - Communication to users:
     - In-app notification: "We've improved the DSCR calculation for better accuracy. Results may differ slightly from previous versions."
     - Changelog entry with detailed explanation
     - For major formula changes: Blog post or email to Pro/AI users

4. **Ongoing Monitoring and Optimization**
   - Monthly performance reviews:
     - Review p95/p99 latency trends (are we getting slower?)
     - Review error rates by calculator and tier
     - Review AI success rates and costs
     - Identify slow calculators or high-error calculators for optimization
   - Quarterly cost reviews:
     - AI costs per calculator (which calculators drive high AI usage?)
     - Infrastructure costs (database, hosting, CDN)
     - Identify optimization opportunities (caching, query tuning, formula simplification)
   - Quarterly user feedback analysis:
     - Review support tickets (common issues, feature requests)
     - Review in-app feedback and survey responses
     - Review conversion funnel drop-offs (are upgrade prompts effective?)
     - Plan product improvements based on themes

**For each subsection:**
- Process or policy
- Severity levels or timelines
- Examples and procedures

**Format:** Section per support topic. 3-4 pages.

---

## Overall Guidelines

**Tone:** Process and quality-focused. Writing for QA, engineering, and operations teams.

**Formatting:**
- Markdown headers: ##, ###, ####
- Tables for severity levels and timelines
- Checklists for release process
- JSON examples for golden scenarios
- Bullet lists for procedures

**Consistency:**
- Reference Section 1.6 for quality goals
- Reference Section 2.2 for golden scenario requirement
- Use consistent severity levels (P0, P1, P2, P3)
- Use consistent versioning (Major.Minor.Patch)

**Length:** 14-17 pages total across 4 files

**Output Files:**
- 10.1-test-strategy.md
- 10.2-golden-scenarios.md
- 10.3-release-process.md
- 10.4-support-maintenance.md

Each file starts with # header matching title.