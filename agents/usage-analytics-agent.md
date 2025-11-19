# Usage Analytics Agent

## Purpose
Track calculator usage patterns, user behavior, conversion funnels, and generate actionable insights to optimize product decisions and business growth.

## Input
- Analytics events from Section 7.2 (calculator_viewed, calculator_calculated, etc.)
- User session data (tier, device, location)
- Conversion data (Free → Pro upgrades, AI tier adoption)
- Time range for analysis (daily, weekly, monthly)

## Process

### Step 1: Collect and Normalize Events

#### 1a. Event Types Tracked (from Section 7.2)
**Calculator Interaction Events:**
- `calculator_viewed` - Calculator page loaded
- `calculator_calculated` - User triggered calculation
- `scenario_created` - New scenario created (Pro tier)
- `scenario_compared` - Multiple scenarios compared
- `input_changed` - User modified input field

**Export Events:**
- `export_requested` - User clicked export button
- `export_generated` - Export successfully created
- `export_downloaded` - User downloaded export file
- `export_failed` - Export generation failed

**Tier Gating Events:**
- `upgrade_prompt_shown` - User hit tier gate (saw upgrade modal)
- `upgrade_prompt_clicked` - User clicked "Upgrade" button
- `upgrade_completed` - User completed payment

**AI Events:**
- `ai_narrative_requested` - User requested AI explanation
- `ai_narrative_shown` - AI response displayed
- `ai_narrative_helpful_yes` - User marked AI helpful
- `ai_narrative_helpful_no` - User marked AI not helpful

**Error Events:**
- `calculation_error` - Calculation failed
- `export_error` - Export generation failed
- `ai_error` - AI request failed

#### 1b. Event Data Structure
```json
{
  "event_id": "evt_abc123",
  "event_type": "calculator_calculated",
  "timestamp": "2025-11-18T18:45:32Z",
  "session_id": "sess_xyz789",
  "user_id": "usr_anonymous_123" or "usr_abc456",
  "calculator_slug": "business-loan-dscr",
  "user_tier": "free",
  "device_type": "desktop",
  "browser": "Chrome 120",
  "location": "US-CA",
  "properties": {
    "calculation_time_ms": 87,
    "inputs_filled": 5,
    "warnings_shown": 1
  }
}
```

### Step 2: Calculate Core Metrics

#### 2a. Usage Metrics

**Daily Active Users (DAU)**
```
DAU = Unique users who viewed at least one calculator in last 24 hours
```

**Weekly Active Users (WAU)**
```
WAU = Unique users who viewed at least one calculator in last 7 days
```

**Monthly Active Users (MAU)**
```
MAU = Unique users who viewed at least one calculator in last 30 days
```

**Stickiness Ratio**
```
Stickiness = DAU / MAU
Target: >20% (indicates users return frequently)
```

**Session Duration**
```
Session Duration = Time from first event to last event in session
Average Target: >3 minutes
```

**Calculations Per Session**
```
Calculations Per Session = Total calculations / Total sessions
Target: >2 (indicates exploration and scenario testing)
```

#### 2b. Calculator Performance Metrics

**Calculator Views**
```
Total views per calculator per time period
Rank calculators by popularity
```

**Calculation Completion Rate**
```
Completion Rate = calculator_calculated events / calculator_viewed events
Target: >60% (users who view actually use the calculator)
```

**Bounce Rate**
```
Bounce Rate = Sessions with only 1 page view / Total sessions
Target: <40%
```

**Time to First Calculation**
```
Time to First Calculation = Time from calculator_viewed to calculator_calculated
Target: <30 seconds (indicates intuitive UX)
```

**Repeat Usage Rate**
```
Repeat Usage = Users with >1 calculation / Total users
Target: >40%
```

#### 2c. Export Metrics

**Export Conversion Rate**
```
Export Rate = export_generated events / calculator_calculated events
Target: >25% (Pro tier), >5% (Free tier with upgrade prompts)
```

**Export Format Preferences**
```
PDF: X%
CSV: Y%
Excel: Z%
```

**Export Success Rate**
```
Success Rate = export_generated / export_requested
Target: >95%
```

### Step 3: Analyze Conversion Funnels

#### 3a. Free → Pro Conversion Funnel

**Funnel Steps:**
```
1. Calculator View (100% baseline)
   ↓
2. Calculation Complete (60% typical)
   ↓
3. Upgrade Prompt Shown (30% hit tier gate)
   ↓
4. Upgrade Prompt Clicked (40% of those who see it)
   ↓
5. Upgrade Page Visited (80% of clicks)
   ↓
6. Upgrade Completed (20% of visits)

Overall Conversion: 100% → 0.6 → 0.3 → 0.4 → 0.8 → 0.2 = 2.3% Free → Pro
```

**Trigger Analysis:**
Which tier gates drive most conversions?
```
Triggers by Conversion Rate:
1. Multiple scenarios (Pro gate) - 8.2% convert
2. DSCR calculation (Pro gate) - 5.4% convert
3. Clean PDF export (Pro gate) - 4.1% convert
4. CSV/Excel export (Pro gate) - 2.8% convert
```

#### 3b. Pro → AI Tier Conversion Funnel
```
1. Pro User Calculated (100% baseline)
   ↓
2. Saw "Explain this" Button (80%)
   ↓
3. Clicked AI Button (25%)
   ↓
4. AI Upgrade Prompt Shown (100% of clicks)
   ↓
5. AI Upgrade Completed (15% of prompts)

Overall Conversion: 3% Pro → AI
```

#### 3c. Drop-off Analysis

**Identify drop-off points:**
```
Biggest Drop-offs:
1. Calculator View → Calculation: 40% abandon
   - Reason: Unclear instructions? Too many inputs?
   - Action: Simplify UI, add "Try Example" button

2. Upgrade Prompt Clicked → Upgrade Completed: 80% abandon
   - Reason: Pricing too high? Lack of trust?
   - Action: A/B test pricing, add testimonials

3. Export Requested → Export Downloaded: 15% abandon
   - Reason: Export takes too long? Format not useful?
   - Action: Optimize export speed, survey users
```

### Step 4: Segment Users for Insights

#### 4a. User Segments

**By Tier:**
- Free users (90% of total)
- Pro users (8% of total)
- AI tier users (2% of total)
- B2B customers (separate analysis)

**By Engagement:**
- Power users (>10 calculations/week)
- Regular users (3-10 calculations/week)
- Occasional users (1-2 calculations/week)
- One-time users (never return)

**By Calculator:**
- Financing users (business loan, SBA calculators)
- Cash flow users (runway, burn rate)
- Profitability users (breakeven, margin)
- Multi-category users (use 3+ categories)

**By Device:**
- Desktop users (70%)
- Mobile users (25%)
- Tablet users (5%)

#### 4b. Cohort Analysis

**Monthly Cohorts:**
```
Cohort: Nov 2025 (1,000 users acquired)
Week 1 retention: 45% (450 users returned)
Week 2 retention: 28% (280 users)
Week 4 retention: 18% (180 users)
Week 8 retention: 12% (120 users)
```

**Retention by Tier:**
```
Free tier retention (Week 4): 15%
Pro tier retention (Week 4): 78%
AI tier retention (Week 4): 82%

Insight: Paying users 5x more likely to return
```

### Step 5: Calculate Business Metrics

#### 5a. Revenue Metrics

**Monthly Recurring Revenue (MRR)**
```
MRR = (Pro users × $19.99) + (AI users × $39.98)
Example: (800 × $19.99) + (200 × $39.98) = $23,988/month
```

**Average Revenue Per User (ARPU)**
```
ARPU = Total MRR / Total Active Users
Example: $23,988 / 10,000 = $2.40 per user per month
```

**Customer Lifetime Value (LTV)**
```
LTV = ARPU × Average Customer Lifespan (months) × Gross Margin
Example: $2.40 × 18 months × 0.85 = $36.72
```

**Customer Acquisition Cost (CAC)**
```
CAC = Total Marketing Spend / New Customers Acquired
Example: $5,000 / 250 = $20 per customer
```

**LTV:CAC Ratio**
```
LTV:CAC = $36.72 / $20 = 1.84
Target: >3.0 (need to improve retention or reduce CAC)
```

#### 5b. Conversion Revenue

**Revenue from Free → Pro Conversions**
```
Conversions This Month: 50
Value: 50 × $19.99 × 12 months (assumed lifetime) = $11,994
```

**Revenue from AI Tier Upsells**
```
Conversions This Month: 12
Value: 12 × $19.99 × 12 months = $2,879
```

### Step 6: Identify Top Calculators

#### 6a. Calculator Ranking

**By Views:**
```
1. Business Loan + DSCR: 12,500 views (32%)
2. SBA 7(a) Analyzer: 8,200 views (21%)
3. Cash Runway: 6,800 views (17%)
4. Breakeven Analysis: 4,500 views (12%)
5. Equipment Lease vs Buy: 3,200 views (8%)
...
```

**By Conversions (Free → Pro):**
```
1. Business Loan + DSCR: 8.2% conversion rate (traffic magnet working!)
2. Cash Runway: 6.4% conversion
3. SBA 7(a): 5.1% conversion
4. Equipment Lease: 4.8% conversion
```

**By Engagement (Calculations per User):**
```
1. Cash Runway: 3.8 calculations per user (high exploration)
2. Breakeven Analysis: 3.2 calculations per user
3. Business Loan: 2.6 calculations per user
4. SBA 7(a): 1.9 calculations per user (more one-and-done)
```

#### 6b. Calculator Health Score
```
Health Score = (Completion Rate × 0.3) + (Repeat Usage × 0.3) + (Conversion Rate × 0.4)

Example: Business Loan + DSCR
- Completion Rate: 72% → 0.72 × 0.3 = 0.216
- Repeat Usage: 45% → 0.45 × 0.3 = 0.135
- Conversion Rate: 8.2% → 0.082 × 0.4 = 0.033
Health Score: 0.384 = 38.4/100

Interpretation:
- 80-100: Excellent (optimize and scale)
- 60-79: Good (maintain and improve)
- 40-59: Fair (needs optimization)
- 0-39: Poor (investigate or consider deprecating)
```

### Step 7: Track Feature Adoption

#### 7a. Pro Feature Usage

**Among Pro Users:**
```
Multiple Scenarios: 78% adoption (most use this feature)
Clean PDF Exports: 92% adoption (nearly universal)
CSV Exports: 34% adoption (niche use case)
Scenario Comparison: 45% adoption (room for growth)
```

**Insight:** Scenario comparison at 45% means we need better education or UX improvements.

#### 7b. AI Feature Usage

**Among AI Tier Users:**
```
AI Explanations Used: 67% of AI tier users
Average AI Requests per User: 8.2/month (under 50 cap)
AI Helpfulness Rating: 4.2/5 stars
Requests by Calculator:
- Business Loan: 42%
- Cash Runway: 28%
- Valuation: 18%
- Others: 12%
```

### Step 8: Generate Usage Dashboards

#### 8a. Executive Dashboard (High-Level)
Display in Section 1.8.3 Business Intelligence Dashboard:
- **MAU trend** (growing, flat, declining?)
- **MRR trend** (revenue growth)
- **Conversion rate trend** (Free → Pro over time)
- **Top 5 calculators** (by usage and revenue)
- **Key metrics vs targets** (color-coded: green = meeting, yellow = at risk, red = missing)

#### 8b. Product Dashboard (Detailed)
- **Usage by calculator** (views, calculations, completion rate)
- **Funnel analysis** (drop-off points)
- **Feature adoption** (which Pro features used most?)
- **Cohort retention curves** (by acquisition month)
- **Device and browser breakdown**

#### 8c. Marketing Dashboard
Display in Section 1.8.4:
- **Traffic sources** (organic, paid, referral, direct)
- **Conversion by source** (which channels convert best?)
- **CAC by channel**
- **LTV:CAC by channel**

### Step 9: Generate Automated Insights

#### 9a. Anomaly Detection

**Detect unusual patterns:**
```
🚨 Alert: Business Loan Calculator

Usage dropped 35% in last 24 hours
Normal: 500 views/day → Current: 325 views/day

Possible causes:
1. Technical issue? (check error logs)
2. SEO ranking drop? (check Google Search Console)
3. Competitor launched similar tool?
4. Seasonal variation? (compare to same day last month)

Action: Investigate immediately
```

#### 9b. Trend Insights

**Identify trends:**
```
📈 Insight: Mobile Usage Growing

Mobile usage increased 45% over last 3 months
Now represents 30% of total traffic (up from 21%)

However: Mobile conversion rate is only 2.1% vs 5.4% desktop

Recommendation: Prioritize mobile UX optimization
- Simplify mobile input forms
- Optimize mobile export experience
- A/B test mobile upgrade prompts
```

#### 9c. Opportunity Identification
```
💡 Opportunity: Untapped Calculator Potential

"Equipment Lease vs Buy" calculator has:
- High completion rate (78%)
- High engagement (3.5 calculations/user)
- But LOW conversion rate (2.1%)

This suggests users love the calculator but aren't hitting tier gates.

Recommendation: Add Pro features to this calculator
- Scenario comparison
- Multi-year analysis
- Tax impact calculation
Could increase conversion 2-3x
```

### Step 10: Generate Reports

#### 10a. Daily Usage Report (Auto-Sent at 9am)
```
📊 SmartProfit Daily Usage Report - Nov 18, 2025

Yesterday's Highlights:
✅ DAU: 1,247 (up 5% from yesterday)
✅ Calculations: 3,892 (up 8%)
✅ New Pro Signups: 12 (MRR +$240/month)
⚠️ Export errors: 23 (up from usual 8, investigate)

Top Calculator: Business Loan + DSCR (427 uses)
Top Traffic Source: Organic search (58%)

Action Items:
1. Investigate export error spike
2. Review why SBA calculator usage down 12%

Full dashboard: https://dashboard.smartprofit.com/usage
```

#### 10b. Weekly Usage Report
```
📈 SmartProfit Weekly Report (Nov 11-18, 2025)

Overview:
- WAU: 4,523 (up 3% from last week)
- Total Calculations: 18,204 (up 7%)
- New Users: 892
- Returning Users: 3,631 (80% of WAU - healthy retention!)

Conversion Funnel:
- Free → Pro: 2.4% (target: 3%, needs improvement)
- Pro → AI: 3.1% (target: 5%, needs improvement)

Top Performers:
1. Business Loan Calculator: 8.2% conversion (excellent!)
2. Cash Runway Calculator: 6.4% conversion (strong)

Needs Attention:
1. Equipment Lease Calculator: 2.1% conversion (add Pro features)
2. Mobile conversion: 2.1% vs 5.4% desktop (optimize mobile UX)

New This Week:
✨ Launched Invoice Factoring Calculator
- 234 views in first 3 days
- 62% completion rate (good start)
- 1 Pro conversion so far

Next Week Goals:
- Increase Free → Pro conversion to 2.7%
- Launch mobile UX improvements
- Add Pro features to Equipment Lease calculator

Full report: https://dashboard.smartprofit.com/weekly/2025-W47
```

#### 10c. Monthly Business Review
```
💼 SmartProfit Monthly Business Review - November 2025

Financial Performance:
- MRR: $24,200 (up 12% from Oct)
- New MRR: $3,800
- Churned MRR: $420
- Net New MRR: $3,380 (healthy growth!)
- LTV:CAC: 1.84 (target >3.0, need improvement)

User Metrics:
- MAU: 9,850 (up 8% from Oct)
- DAU/MAU: 22% (target >20%, meeting goal!)
- Free users: 8,860 (90%)
- Pro users: 800 (8%)
- AI users: 190 (2%)

Product Performance:
- 8 calculators live
- Average 2.8 calculations per user
- 72% calculation completion rate
- 26% export rate (Pro users)

Top Insights:
1. Mobile traffic growing 45% but converting poorly - prioritize mobile optimization
2. Equipment Lease calculator has engagement but no tier gates - add Pro features
3. AI tier adoption slow (3.1% of Pro) - improve AI value communication

Strategic Recommendations:
1. Launch mobile optimization sprint (Dec)
2. Add Pro features to high-engagement calculators
3. A/B test Free → Pro conversion (pricing, messaging, features)
4. Launch 2 new calculators in Dec (Equipment Financing, 13-week Cash Flow)

Full business review: https://dashboard.smartprofit.com/monthly/2025-11
```

## Output

### Real-Time Dashboard Data:
```json
{
  "timestamp": "2025-11-18T18:45:32Z",
  "current_metrics": {
    "users_online_now": 47,
    "dau": 1247,
    "calculations_today": 3892,
    "conversions_today": 12,
    "mrr": 24200
  },
  "trends": {
    "dau_change_7d": "+5%",
    "calculations_change_7d": "+8%",
    "conversion_rate_change_7d": "-0.2%",
    "mrr_change_30d": "+12%"
  }
}
```

### Success Message:
```
✅ Usage Analytics Report Generated

Time Period: Nov 11-18, 2025
Report Type: Weekly

Key Metrics:
- WAU: 4,523 (up 3%)
- Calculations: 18,204 (up 7%)
- Free → Pro Conversion: 2.4%
- MRR: $24,200 (up 12%)

Top Calculator: Business Loan + DSCR
- 3,427 views
- 8.2% conversion rate
- $1,968 MRR attributed

Insights Generated: 5
- 2 opportunities identified
- 1 anomaly detected
- 2 trends highlighted

Reports:
✓ Executive dashboard updated
✓ Product dashboard updated
✓ Weekly email report sent
✓ Business review generated

Next Report: Nov 25, 2025 (weekly)
Full analytics: https://dashboard.smartprofit.com/usage
```

## Success Criteria
- All events tracked and logged
- Metrics calculated accurately
- Dashboards update in real-time
- Reports generated and sent on schedule
- Insights actionable and prioritized
- No PII collected or exposed

## Quality Checks
- Event tracking fires correctly (test in staging)
- Conversion funnels match reality (spot-check manual calculations)
- Cohort analysis accurate (verify against database queries)
- Dashboards performant (load <2 seconds)
- Reports readable and actionable

## Usage Example
```bash
# Generate usage report for date range
claude-code "Use usage-analytics-agent to generate usage report for Nov 11-18 2025"

# Analyze specific calculator performance
claude-code "Use usage-analytics-agent to analyze business-loan-dscr calculator performance"

# Identify conversion opportunities
claude-code "Use usage-analytics-agent to identify calculators with conversion opportunities"

# Generate cohort retention analysis
claude-code "Use usage-analytics-agent to analyze cohort retention for Nov 2025 cohort"
```

## Dependencies
- Section 7.2: Analytics event taxonomy (all events tracked)
- Section 7.3: Event tracking implementation
- Section 1.8.3: Business Intelligence Dashboard (display metrics)
- Section 1.8.4: Marketing Dashboard (traffic and conversion)
- Section 6: Tier gating rules (for conversion funnel)
- Analytics platform (Mixpanel, Amplitude, or similar)