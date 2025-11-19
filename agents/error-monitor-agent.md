# Error Monitor Agent

## Purpose
Monitor calculator errors in real-time, categorize by severity, alert appropriate teams, and track error trends to maintain high reliability and user satisfaction.

## Input
- Error logs from application (frontend and backend)
- Calculator slug (which calculator errored)
- Error type and stack trace
- User context (tier, session ID, anonymized)
- Timestamp and frequency

## Process

### Step 1: Capture and Parse Errors

#### 1a. Error Sources
Monitor errors from:
- **Frontend JavaScript errors** (React component crashes, calculation failures)
- **Backend API errors** (formula library errors, database failures)
- **Export generation errors** (PDF/CSV/Excel failures)
- **AI service errors** (timeouts, API failures, rate limits)
- **Integration errors** (WordPress plugin, third-party APIs)

#### 1b. Error Data Structure
```json
{
  "error_id": "err_abc123xyz",
  "timestamp": "2025-11-18T18:45:32Z",
  "calculator_slug": "business-loan-dscr",
  "error_type": "calculation_error",
  "error_message": "Cannot calculate DSCR: division by zero",
  "stack_trace": "...",
  "user_context": {
    "session_id": "sess_xyz789",
    "user_tier": "free",
    "browser": "Chrome 120",
    "device": "desktop"
  },
  "inputs": {
    "loan_amount": 250000,
    "interest_rate": 7.5,
    "term_years": 10,
    "annual_revenue": 1500000,
    "annual_expenses": 1500000
  },
  "severity": "high",
  "impact": "calculation_failed"
}
```

### Step 2: Categorize Errors by Type

#### 2a. Error Type Classification

**Calculation Errors**
- Formula execution failures (divide by zero, invalid inputs)
- Numerical overflow/underflow
- Missing formula library functions
- Invalid calculation results (NaN, Infinity)

**Validation Errors**
- Input validation failures (out of range, wrong type)
- Schema validation errors (JSON config invalid)
- Tier gating violations (user accessing Pro features on Free tier)

**Integration Errors**
- API timeouts or failures
- Database connection errors
- External service failures (AI provider down)
- WordPress embed errors

**Export Errors**
- PDF generation failures
- CSV/Excel formatting errors
- Export timeout (>3 seconds)
- Missing data in exports

**UI/UX Errors**
- Component render failures
- JavaScript runtime errors
- CSS/layout issues causing non-functional UI
- Browser compatibility issues

**Performance Errors**
- Calculation exceeding SLA (>150ms p95)
- Export exceeding SLA (>3s p95)
- AI request exceeding timeout (>8s)
- Page load time exceeding target (>2s)

### Step 3: Assess Error Severity

#### 3a. Severity Levels

**Critical (P0) - Immediate Action Required**
- Calculator completely non-functional
- All calculations failing
- Data loss or corruption
- Security breach detected
- Payment processing failures

Criteria:
- Affects >50% of users
- No workaround available
- Revenue impact immediate
- Security/compliance risk

**High (P1) - Urgent**
- One calculator completely broken
- Major feature non-functional (exports, AI)
- Frequent errors affecting many users (>5% error rate)
- Performance SLA violations (>2x target)

Criteria:
- Affects 10-50% of users
- Limited workaround available
- Significant user impact
- Degraded experience

**Medium (P2) - Important**
- Specific scenario/edge case failing
- Minor feature broken (one export format)
- Intermittent errors affecting some users (<5% error rate)
- Warning messages not displaying correctly

Criteria:
- Affects <10% of users
- Workaround available
- Moderate user impact
- Does not block core functionality

**Low (P3) - Monitor**
- Cosmetic issues (styling, tooltips)
- Non-critical warnings
- Logging/telemetry issues
- Known limitations (documented)

Criteria:
- Minimal user impact
- Easy workaround
- Does not affect functionality
- Quality-of-life improvement

### Step 4: Determine Impact and Blast Radius

#### 4a. Impact Assessment
- **User Impact:** How many users affected? (count, percentage)
- **Feature Impact:** Which features broken? (core vs optional)
- **Calculator Impact:** Which calculators affected? (one vs multiple)
- **Tier Impact:** Which tiers affected? (Free, Pro, AI, B2B)
- **Revenue Impact:** Estimated revenue loss (blocked upgrades, churn risk)

#### 4b. Blast Radius Calculation
```
Blast Radius = (Users Affected / Total Active Users) × Feature Criticality

Where:
- Feature Criticality:
  - Core calculation: 1.0
  - Export: 0.7
  - AI: 0.5
  - UI/cosmetic: 0.2

Example:
- 500 users affected out of 10,000 active = 5%
- Core calculation broken = 1.0 criticality
- Blast Radius = 5% × 1.0 = 5% (HIGH severity)
```

### Step 5: Alert Appropriate Teams

#### 5a. Alerting Rules

**Critical (P0) Alerts**
- **Who:** On-call engineer (immediate page), CTO, product manager
- **How:** PagerDuty page, SMS, phone call
- **When:** Immediately (within 1 minute of detection)
- **Escalation:** If no response in 5 minutes, escalate to backup engineer

**High (P1) Alerts**
- **Who:** Engineering team lead, product manager
- **How:** Slack alert, email
- **When:** Within 5 minutes
- **Escalation:** If no response in 30 minutes, page on-call

**Medium (P2) Alerts**
- **Who:** Engineering team (general channel)
- **How:** Slack message
- **When:** Within 1 hour (or bundled in daily digest)
- **Escalation:** None unless error rate increases

**Low (P3) Alerts**
- **Who:** Engineering team
- **How:** Daily digest email, Jira ticket auto-created
- **When:** Next business day
- **Escalation:** None

#### 5b. Alert Message Format

**Slack Alert (P1 Example):**
```
🚨 HIGH SEVERITY ERROR DETECTED

Calculator: business-loan-dscr
Error Type: calculation_error
Error Rate: 8.2% (41 errors in last 10 minutes)
Impact: Calculation failing for DSCR when expenses equal revenue

Affected Users: ~200 (estimated)
First Seen: 2025-11-18 18:45:32 UTC
Last Seen: 2025-11-18 18:55:12 UTC

Error Message: "Cannot calculate DSCR: division by zero"

Dashboard: https://dashboard.smartprofit.com/errors/err_abc123xyz
Logs: https://logs.smartprofit.com/search?error_id=err_abc123xyz

Action Required:
1. Investigate root cause (likely formula library edge case)
2. Apply hotfix or rollback if necessary
3. Update golden scenarios to catch this case

Assigned: @engineering-oncall
Priority: P1 (resolve within 24 hours)
```

### Step 6: Track Error Trends and Patterns

#### 6a. Error Aggregation
Group errors by:
- Calculator (which calculators have highest error rates?)
- Error type (what types of errors most common?)
- Time of day (errors spike at certain times?)
- User tier (Free vs Pro error rates different?)
- Browser/device (mobile vs desktop error rates?)

#### 6b. Trend Analysis
```
Weekly Error Report:
====================

Total Errors: 347 (up 12% from last week)
Error Rate: 0.8% (target: <1%)

Top 5 Error Sources:
1. business-loan-dscr: 89 errors (26% of total)
   - Root cause: Division by zero when expenses = revenue
   - Status: Fix deployed 2025-11-17
   
2. sba-7a-analyzer: 67 errors (19% of total)
   - Root cause: Guarantee fee calculation incorrect for loans >$500k
   - Status: Investigating
   
3. AI service timeouts: 54 errors (16% of total)
   - Root cause: Anthropic API intermittent latency
   - Status: Monitoring, no action needed
   
4. Export PDF generation: 45 errors (13% of total)
   - Root cause: Large scenarios (>10 pages) timing out
   - Status: Optimization in progress
   
5. Mobile UI crashes: 32 errors (9% of total)
   - Root cause: Safari iOS 16 compatibility issue
   - Status: Fix scheduled for next release

Trends:
- Error rate increasing on mobile devices (2.1% vs 0.5% desktop)
- AI timeouts increased 40% (external issue, monitoring)
- Export errors decreased 15% after optimization
```

### Step 7: Auto-Triage Common Errors

#### 7a. Known Error Patterns
Maintain database of known errors:
```json
{
  "error_pattern": "division_by_zero_dscr",
  "error_message_regex": "Cannot calculate DSCR.*division by zero",
  "root_cause": "Annual debt service is zero (invalid loan amount or term)",
  "severity": "medium",
  "auto_action": "reject_calculation",
  "user_message": "Unable to calculate DSCR. Please verify your loan amount and term are greater than zero.",
  "fix_status": "known_limitation",
  "documentation": "https://docs.smartprofit.com/errors/dscr-division-zero"
}
```

#### 7b. Auto-Resolution Actions
For known errors:
- Return helpful error message to user (not technical stack trace)
- Log error for tracking but don't alert (unless frequency spikes)
- Suggest corrective action to user ("Try increasing loan term")
- Link to documentation or FAQ

### Step 8: Generate Error Dashboards

#### 8a. Real-Time Error Dashboard
Display in Section 1.8.6 Security & Compliance Dashboard:
- **Error rate chart** (last 24 hours, last 7 days, last 30 days)
- **Top errors** (by frequency)
- **Errors by calculator** (which calculators most problematic?)
- **Errors by severity** (P0, P1, P2, P3 counts)
- **Mean Time To Resolution (MTTR)** (average time to fix errors)
- **Error-free uptime** (percentage of time with zero errors)

#### 8b. Weekly Error Report
Auto-generated every Monday:
- Error summary (total, rate, trend)
- Top issues (root causes, status, owners)
- Fixed issues (closed this week)
- Action items (what needs attention)
- SLA performance (% of P0 resolved <4 hours, P1 <24 hours)

### Step 9: Integrate with Incident Management

#### 9a. Auto-Create Incidents
For P0/P1 errors:
- Create incident in incident management system (PagerDuty, Opsgenie)
- Assign to on-call engineer
- Track status (investigating, fixing, resolved)
- Post-mortem required for all P0 incidents

#### 9b. Incident Timeline
```
Incident #1247: business-loan-dscr calculation failures
=======================================================

Status: RESOLVED
Severity: P1 (High)
Duration: 2 hours 18 minutes
Impact: ~400 users unable to calculate DSCR

Timeline:
18:45 UTC - Error detected (automated alert)
18:47 UTC - On-call engineer acknowledged
18:52 UTC - Root cause identified (division by zero)
19:15 UTC - Hotfix developed and tested
19:32 UTC - Hotfix deployed to production
20:05 UTC - Verification: error rate returned to normal
21:03 UTC - Incident closed

Root Cause:
Formula library did not handle edge case where annual expenses exactly equal annual revenue, resulting in division by zero when calculating DSCR.

Fix:
Added validation check: if (annualDebtService === 0) return error with user-friendly message.

Prevention:
- Added golden test scenario for this edge case
- Updated input validation to warn when expenses ≥ revenue
- Improved formula library error handling

Post-Mortem: https://docs.smartprofit.com/postmortems/2025-11-18-dscr-calculation
```

### Step 10: Learn and Improve

#### 10a. Error Pattern Recognition
Use ML/heuristics to:
- Identify recurring error patterns
- Predict when errors likely to occur (time of day, traffic spikes)
- Suggest preventive measures (input validation, better error handling)

#### 10b. Proactive Improvements
Based on error trends:
- **If mobile errors high:** Prioritize mobile testing and compatibility
- **If export errors high:** Optimize export generation, add timeouts
- **If AI errors high:** Implement better retry logic, fallback messaging
- **If specific calculator errors high:** Review formula library, add golden scenarios

## Output

### Real-Time Alert Example:
```
🚨 CRITICAL ERROR ALERT

Calculator: business-loan-dscr
Severity: P0 (CRITICAL)
Error Rate: 52% (104 errors in last 5 minutes)

Impact: Calculator non-functional for majority of users
Blast Radius: 50%+ of active users affected

Error: "Formula library function 'calculateDSCR' not found"

Likely Cause: Deployment issue - formula library not loaded

IMMEDIATE ACTION REQUIRED:
1. Check if formula library deployed correctly
2. Rollback deployment if necessary
3. Verify formula library version matches calculator config

Incident: INC-1248 created
Assigned: @oncall-engineer
Dashboard: https://dashboard.smartprofit.com/incidents/1248

⏰ Response Time Target: 15 minutes
```

### Weekly Error Summary:
```
📊 Error Monitor Weekly Report (Nov 11-18, 2025)

Overall Health: 🟢 HEALTHY
Error Rate: 0.7% (target <1%) ✅
Uptime: 99.4% (target >99%) ✅

Errors This Week: 312 (down 8% from last week)

Severity Breakdown:
- P0 (Critical): 0 ✅
- P1 (High): 2 (both resolved <24 hours) ✅
- P2 (Medium): 47 (avg resolution time: 3.2 days)
- P3 (Low): 263 (triaged to backlog)

Top Issues Resolved:
✅ business-loan-dscr division by zero (P1) - fixed
✅ PDF export timeout for large scenarios (P2) - optimized
✅ Safari iOS 16 UI crash (P2) - compatibility fix deployed

Active Issues:
🔧 sba-7a guarantee fee calculation (P1) - investigating
🔧 AI service intermittent timeouts (P2) - monitoring external issue

Action Items:
- [ ] Add golden scenarios for edge cases (prevent future P1 errors)
- [ ] Improve mobile error handling (reduce mobile error rate)
- [ ] Review and update error documentation

MTTR This Week:
- P0: N/A (no P0 incidents)
- P1: 3.5 hours (target <24 hours) ✅
- P2: 2.8 days (target <7 days) ✅

Full report: https://dashboard.smartprofit.com/errors/weekly/2025-W47
```

## Success Criteria
- All errors captured and logged
- P0/P1 errors alerted within 1 minute
- Error trends identified and reported
- Auto-triage for common errors working
- MTTR meeting SLA targets
- Weekly report generated automatically

## Quality Checks
- Alert noise is low (no alert fatigue)
- Severity classification accurate (not over/under-alerting)
- Error messages helpful to users (not technical jargon)
- Dashboards provide actionable insights
- Post-mortems completed for all P0 incidents

## Usage Example
```bash
# Monitor errors in real-time
claude-code "Use error-monitor-agent to start monitoring calculator errors"

# Generate error report for date range
claude-code "Use error-monitor-agent to generate error report for Nov 11-18 2025"

# Investigate specific error pattern
claude-code "Use error-monitor-agent to investigate division by zero errors in business-loan-dscr"

# Check error trends
claude-code "Use error-monitor-agent to analyze error trends and identify patterns"
```

## Dependencies
- Section 1.6: SLA targets (for detecting SLA violations)
- Section 7.2: Analytics events (error tracking)
- Section 1.8.6: Security & Compliance Dashboard (error display)
- Section 10.4: Support & Maintenance (bug triage process)
- Error tracking service (Sentry, Rollbar, or similar)
- Incident management system (PagerDuty, Opsgenie)