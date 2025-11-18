# Cash Runway & Burn Rate Calculator

## 1. Calculator Overview

**Calculator Name**: Cash Runway & Burn Rate Calculator

**Calculator Slug**: `cash-runway`

**Primary Category**: Cash Flow & Liquidity Intelligence

**Secondary Category**: Planning & Forecasting Intelligence

**Business Role**:
- **Founders/CFO awareness tool**: High emotional value (answers "when do we run out of money?")
- **AI narrative showcase**: Excellent use case for AI scenario suggestions ("when should we fundraise?")
- **Pro upgrade driver**: Scenario analysis (base/upside/downside) drives conversions
- **Viral potential**: Startup founders share with co-founders and investors

**Primary Success Metric**: AI narrative usage rate (target: 40-50% of Pro users request AI insights)

**Version**: v1.0.0

**Formula Library Version**: v1.0.0

---

## 2. User Scenarios and Use Cases

### Who Uses This Calculator

**Primary Personas**:
- **Startup founders** tracking cash runway and planning fundraising timeline
- **CFOs** monitoring burn rate and forecasting cash needs
- **Investors** evaluating portfolio company cash positions and runway

**Secondary Personas**:
- **Board members** reviewing cash position during board meetings
- **Fractional CFOs** advising clients on cash management
- **Business owners** (non-startups) managing seasonal cash flow

### When They Use It (Decision Contexts)

**Timing**:
- **After fundraising**: Tracking how long new capital will last
- **Monthly/quarterly monitoring**: Tracking burn rate trends (is burn increasing or decreasing?)
- **Before fundraising**: Determining how much time remains to close next round
- **During budget planning**: Scenario testing ("what if we hire 3 more people?")
- **Crisis mode**: Cash is tight, need to know exactly how long until zero

**Frequency**:
- **Monthly monitoring** (most common): Pro users update cash balance monthly
- **Weekly monitoring** (high-burn startups): Track runway closely when < 6 months
- **One-time use** (Free tier): Founders check runway once, then forget

### What Questions It Answers

This calculator helps users answer:

1. **How many months of runway do we have?** (Most urgent question, clear answer)
2. **What is our current burn rate?** (Monthly cash consumption)
3. **When will we hit zero cash?** (Specific date for planning)
4. **What if revenue grows 20%?** (Upside scenario - runway extends)
5. **What if revenue drops 20%?** (Downside scenario - need to fundraise sooner)
6. **When should we start fundraising?** (Typically need 18 months runway when starting)

---

## 3. Inputs

### Input Field Definitions

| Field Name | Type | Units | Required | Validation | Default | Placeholder | Tooltip |
|------------|------|-------|----------|------------|---------|-------------|---------|
| current_cash_balance | currency | dollars | Yes | min: 0, max: 1000000000 | null | "500000" | "Current cash and cash equivalents (checking, savings, money market). Do not include accounts receivable or inventory." |
| monthly_revenue | currency | dollars | Yes | min: 0, max: 100000000 | null | "50000" | "Average monthly revenue (cash received, not just invoiced). Use trailing 3-month average for accuracy." |
| monthly_expenses | currency | dollars | Yes | min: 0, max: 100000000 | null | "75000" | "Average monthly operating expenses (all cash out: payroll, rent, software, marketing, etc.). Use trailing 3-month average." |

### Input Groups

**Group 1: Cash Position** (always visible)
- current_cash_balance

**Group 2: Monthly Cash Flow** (always visible)
- monthly_revenue
- monthly_expenses

**Group 3: Scenario Assumptions** (collapsed by default, Pro tier feature)
- **revenue_growth_rate_monthly** (percentage): Monthly revenue growth rate (compound). Default: 0%
- **expense_growth_rate_monthly** (percentage): Monthly expense growth rate (e.g., hiring plan). Default: 0%
- **one_time_inflow** (currency): Expected one-time cash inflow (e.g., upcoming invoice payment). Default: 0
- **one_time_inflow_month** (number): Month when one-time inflow occurs. Default: 0
- **one_time_outflow** (currency): Expected one-time cash outflow (e.g., tax payment, equipment purchase). Default: 0
- **one_time_outflow_month** (number): Month when one-time outflow occurs. Default: 0

### Optional Inputs

**Advanced Options** (collapsed, rarely used):
- **minimum_cash_buffer** (currency): Minimum cash balance to maintain (operational buffer). Default: 0
- **include_ar_in_cash** (boolean): Include accounts receivable expected in next 30 days as "cash". Default: No

---

## 4. Outputs

### Key Metrics (Free Tier - Always Visible)

| Output Name | Description | Format | Units |
|-------------|-------------|--------|-------|
| monthly_burn_rate | Net monthly cash consumption (expenses - revenue) | "$#,##0.00" | dollars |
| runway_months | Months until cash reaches zero (current_cash / burn_rate) | "#.0" | months |
| runway_end_date | Specific date when cash hits zero | "MMM DD, YYYY" | date |

### Advanced Metrics (Pro Tier - Gated)

| Output Name | Description | Format | Units | Pro Only |
|-------------|-------------|--------|-------|----------|
| runway_base_case | Runway assuming current burn continues (base case) | "#.0 months" | months | Yes |
| runway_downside | Runway if revenue drops 20% (downside scenario) | "#.0 months" | months | Yes |
| runway_upside | Runway if revenue grows 20% (upside scenario) | "#.0 months" | months | Yes |
| breakeven_month | Month when revenue = expenses (burn = 0) | "Month #" or "Never" | month | Yes |
| breakeven_revenue_needed | Monthly revenue needed to reach breakeven (zero burn) | "$#,##0.00" | dollars | Yes |
| total_funding_needed | Cash needed to reach breakeven (if revenue growing) | "$#,##0.00" | dollars | Yes |
| daily_burn_rate | Daily cash consumption (monthly burn / 30) | "$#,##0.00" | dollars | Yes |

### Charts and Visualizations

**Chart 1: Cash Runway Timeline** (Free tier, basic; Pro tier, detailed)
- Type: Line chart
- X-axis: Month (0 to runway end)
- Y-axis: Cash balance
- Purpose: Visualize cash declining to zero over time
- Tier: Free (base case only), Pro (base/upside/downside scenarios)

**Chart 2: Burn Rate Trend** (Pro tier only)
- Type: Line chart
- X-axis: Month (historical 6-12 months)
- Y-axis: Burn rate (dollars per month)
- Purpose: Show if burn is increasing, decreasing, or stable
- Tier: Pro (requires historical burn rate data)

**Chart 3: Scenario Comparison** (Pro tier only)
- Type: Multi-line chart
- X-axis: Month
- Y-axis: Cash balance
- Lines: Base case, upside (+20% revenue), downside (-20% revenue)
- Purpose: Visualize sensitivity to revenue changes
- Tier: Pro

### Output Formatting Rules

- **Currency**: $#,##0.00 (two decimals for precision)
- **Runway months**: One decimal (e.g., "14.3 months" for clarity)
- **Dates**: "Jan 15, 2026" (month day, year format)
- **Burn rate**: Always show as positive number (absolute value) with note "(cash out)"
- **Breakeven month**: Integer (e.g., "Month 18") or "Never" if expenses > revenue with no growth

---

## 5. Formulas and Calculation Logic

### Formula 1: Monthly Burn Rate

**Purpose**: Calculate net monthly cash consumption

**Equation**:
```
monthly_burn_rate = monthly_expenses - monthly_revenue

If monthly_burn_rate > 0:
    # Burning cash (expenses > revenue)
    status = "Cash burn"
elif monthly_burn_rate < 0:
    # Generating cash (revenue > expenses)
    status = "Cash positive"
else:
    # Breakeven (revenue = expenses)
    status = "Breakeven"
```

**Variables**:
- monthly_expenses: Total monthly cash outflows
- monthly_revenue: Total monthly cash inflows
- monthly_burn_rate: Net cash consumption (positive = burning, negative = generating)

**Source/Reference**: Standard cash flow analysis

### Formula 2: Runway (Base Case)

**Purpose**: Calculate months until cash reaches zero at current burn rate

**Equation**:
```
if monthly_burn_rate <= 0:
    # Cash positive or breakeven - infinite runway
    runway_months = Infinity
    runway_end_date = "Never (cash positive)"
else:
    # Cash burn - calculate months until zero
    runway_months = current_cash_balance / monthly_burn_rate

    # Calculate specific end date
    runway_end_date = today + (runway_months × 30.44 days)  # 30.44 = avg days/month
```

**Variables**:
- current_cash_balance: Cash on hand today
- monthly_burn_rate: Net monthly cash consumption
- runway_months: Months until cash = 0
- runway_end_date: Calendar date when cash hits zero

**Source/Reference**: Standard runway calculation used in venture capital

### Formula 3: Runway with Growth Scenarios

**Purpose**: Calculate runway assuming revenue/expense growth over time

**Equation**:
```
# Initialize
cash_balance = current_cash_balance
month = 0
revenue = monthly_revenue
expenses = monthly_expenses

# Simulate month-by-month until cash reaches zero
while cash_balance > 0 and month < 120:  # max 10 years
    month += 1

    # Apply growth rates
    if revenue_growth_rate_monthly != 0:
        revenue = revenue × (1 + revenue_growth_rate_monthly / 100)

    if expense_growth_rate_monthly != 0:
        expenses = expenses × (1 + expense_growth_rate_monthly / 100)

    # Apply one-time cash flows
    if month == one_time_inflow_month:
        cash_balance += one_time_inflow

    if month == one_time_outflow_month:
        cash_balance -= one_time_outflow

    # Calculate net cash flow for month
    net_cash_flow = revenue - expenses
    cash_balance += net_cash_flow

    # Check if reached breakeven
    if net_cash_flow >= 0 and month > 1:
        breakeven_month = month
        break

runway_months = month
```

**Source/Reference**: Monte Carlo cash flow simulation methodology

### Formula 4: Breakeven Analysis

**Purpose**: Calculate when revenue will equal expenses (zero burn)

**Equation**:
```
# If revenue is growing and expenses are flat or growing slower
if revenue_growth_rate_monthly > expense_growth_rate_monthly:
    # Calculate month when revenue catches up to expenses
    # revenue × (1 + g)^n = expenses × (1 + e)^n
    # Solve for n (month)

    # Simplified: if revenue growing at constant rate
    months_to_breakeven = log(expenses / revenue) / log(1 + revenue_growth_rate_monthly)

    if months_to_breakeven > 0 and months_to_breakeven < 120:
        breakeven_month = ceil(months_to_breakeven)
    else:
        breakeven_month = "Never"
else:
    # Revenue not growing or growing slower than expenses
    breakeven_month = "Never"

# Calculate revenue needed for immediate breakeven
breakeven_revenue_needed = monthly_expenses
revenue_gap = breakeven_revenue_needed - monthly_revenue
```

**Source/Reference**: Breakeven analysis formula

### Formula 5: Total Funding Needed to Reach Breakeven

**Purpose**: Calculate how much additional funding is needed to reach breakeven

**Equation**:
```
if breakeven_month == "Never":
    total_funding_needed = "Breakeven not achievable with current growth trajectory"
else:
    # Simulate cash burn from now until breakeven month
    total_burn = 0
    cash_balance = current_cash_balance

    for month in range(1, breakeven_month + 1):
        revenue_month = monthly_revenue × (1 + revenue_growth_rate_monthly / 100) ^ month
        expenses_month = monthly_expenses × (1 + expense_growth_rate_monthly / 100) ^ month
        net_cash_flow = revenue_month - expenses_month
        total_burn += abs(net_cash_flow) if net_cash_flow < 0 else 0
        cash_balance += net_cash_flow

    if cash_balance < 0:
        total_funding_needed = abs(cash_balance)
    else:
        total_funding_needed = 0  # Current cash is sufficient
```

### Formula 6: Scenario Analysis (Upside/Downside)

**Purpose**: Calculate runway under different revenue scenarios

**Equation**:
```
# Base case (current burn)
runway_base = current_cash_balance / monthly_burn_rate

# Upside scenario (revenue +20%)
upside_revenue = monthly_revenue × 1.20
upside_burn = monthly_expenses - upside_revenue
if upside_burn > 0:
    runway_upside = current_cash_balance / upside_burn
else:
    runway_upside = Infinity  # Cash positive in upside case

# Downside scenario (revenue -20%)
downside_revenue = monthly_revenue × 0.80
downside_burn = monthly_expenses - downside_revenue
if downside_burn > 0:
    runway_downside = current_cash_balance / downside_burn
else:
    runway_downside = Infinity
```

### Step-by-Step Calculation Flow

1. **Validate inputs**: Check required fields, ensure cash ≥ 0, expenses > 0
2. **Calculate monthly burn rate**: `monthly_expenses - monthly_revenue`
3. **Determine cash status**: Burning, breakeven, or cash positive
4. **Calculate base case runway**: `current_cash / monthly_burn_rate`
5. **Calculate runway end date**: `today + (runway_months × 30.44 days)`
6. **Calculate daily burn rate**: `monthly_burn_rate / 30`
7. **If Pro tier, calculate scenarios**:
   - Upside runway (revenue +20%)
   - Downside runway (revenue -20%)
   - Breakeven month (if revenue growing)
   - Total funding needed to reach breakeven
8. **If growth rates provided, run month-by-month simulation**
9. **Check warning conditions**: Low runway (< 6 months), high burn, etc.
10. **Return results with version stamp**

### Edge Cases and Handling

| Edge Case | Condition | Handling | Warning/Error |
|-----------|-----------|----------|---------------|
| Cash positive | monthly_revenue > monthly_expenses | Runway = Infinity | Info: "Business is cash positive. No runway concern." |
| Zero cash | current_cash_balance = 0 | Runway = 0 months | Critical: "Zero cash balance. Immediate funding needed." |
| Exactly breakeven | monthly_revenue = monthly_expenses | Runway = Infinity | Info: "Business is at breakeven. Cash neither grows nor shrinks." |
| Very high burn | monthly_burn_rate > current_cash_balance | Runway < 1 month | Critical: "Less than 1 month of runway. Urgent action required." |
| Negative revenue | monthly_revenue < 0 | Error | Error: "Revenue cannot be negative. Enter 0 if no revenue." |

### Formula Version

**Current Version**: v1.0.0

**Version History**:
- v1.0.0: Initial implementation (burn rate, runway, scenario analysis)

---

## 6. Warnings and Alerts

### Warning Definitions

| Warning Code | Condition | Message | Severity | Action |
|--------------|-----------|---------|----------|--------|
| CRITICAL_RUNWAY | runway_months < 3 | "Critical: Only {runway_months} months of runway remaining. You should be actively fundraising or cutting expenses immediately." | danger | Start fundraising now or implement cost cuts |
| LOW_RUNWAY | runway_months < 6 | "Warning: {runway_months} months of runway. Typical fundraising takes 3-6 months. Start fundraising process now." | warning | Begin fundraising or improve cash generation |
| COMFORTABLE_RUNWAY | runway_months >= 12 | "Healthy runway of {runway_months} months. Continue monitoring burn rate monthly." | info | Maintain current trajectory |
| CASH_POSITIVE | monthly_burn_rate < 0 | "Business is cash positive (revenue > expenses). Runway is effectively infinite. Strong position." | success | Consider reinvesting cash in growth |
| HIGH_BURN_ACCELERATION | (current_burn - previous_burn) / previous_burn > 0.25 | "Burn rate increased {percent}% vs previous period. Monitor closely and understand drivers." | warning | Review expense increases, validate ROI of new spending |
| ZERO_CASH | current_cash_balance == 0 | "Zero cash balance. Business cannot operate. Immediate funding or revenue required." | danger | Emergency funding needed immediately |
| DOWNSIDE_CRITICAL | runway_downside < 3 | "In downside scenario (revenue -20%), runway drops to {runway_downside} months. Build cash buffer for safety." | warning | Reduce expenses or raise more capital |
| BREAKEVEN_UNREACHABLE | breakeven_month == "Never" | "Breakeven not achievable with current revenue growth trajectory. Revenue must grow faster or expenses must decrease." | warning | Increase revenue growth or cut expenses |

### Warning Thresholds

**Configurable Thresholds** (can be adjusted per tenant for B2B):
- **Critical runway**: 3 months (venture-backed startups), 1 month (services businesses)
- **Low runway**: 6 months (venture-backed), 3 months (bootstrapped)
- **Comfortable runway**: 12 months (venture-backed), 6 months (bootstrapped)

**Hard-Coded Thresholds** (universal, not configurable):
- **Zero cash**: 0 dollars (mathematical fact)
- **Burn acceleration**: 25% increase month-over-month (concerning rate of change)

---

## 7. Golden Test Scenarios

### Scenario 1: Healthy Startup - 15 Months Runway

**Description**: Typical post-seed startup with healthy runway. No immediate funding pressure.

**Inputs**:
```json
{
  "current_cash_balance": 750000,
  "monthly_revenue": 50000,
  "monthly_expenses": 100000
}
```

**Expected Outputs**:
```json
{
  "monthly_burn_rate": {
    "value": 50000,
    "tolerance": 0,
    "unit": "USD",
    "note": "$100k expenses - $50k revenue = $50k burn"
  },
  "runway_months": {
    "value": 15.0,
    "tolerance": 0.1,
    "unit": "months",
    "note": "$750k / $50k burn = 15 months"
  },
  "runway_end_date": {
    "value": "2027-02-18",
    "note": "Assumes today is 2025-11-18, 15 months forward"
  },
  "daily_burn_rate": {
    "value": 1666.67,
    "tolerance": 1,
    "unit": "USD",
    "note": "$50k / 30 days"
  },
  "runway_base_case": {
    "value": 15.0,
    "tolerance": 0.1,
    "unit": "months"
  },
  "runway_upside": {
    "value": 18.8,
    "tolerance": 0.2,
    "unit": "months",
    "note": "Revenue +20% = $60k, burn = $40k, runway = $750k / $40k"
  },
  "runway_downside": {
    "value": 12.5,
    "tolerance": 0.2,
    "unit": "months",
    "note": "Revenue -20% = $40k, burn = $60k, runway = $750k / $60k"
  },
  "breakeven_revenue_needed": {
    "value": 100000,
    "tolerance": 0,
    "unit": "USD",
    "note": "Need $100k revenue to match $100k expenses"
  }
}
```

**Expected Warnings**:
- COMFORTABLE_RUNWAY: "Healthy runway of 15.0 months. Continue monitoring burn rate monthly."

### Scenario 2: Crisis Mode - 4 Months Runway

**Description**: Startup with low runway, urgent fundraising needed. Downside scenario is critical (< 3 months).

**Inputs**:
```json
{
  "current_cash_balance": 200000,
  "monthly_revenue": 20000,
  "monthly_expenses": 70000
}
```

**Expected Outputs**:
```json
{
  "monthly_burn_rate": {
    "value": 50000,
    "tolerance": 0,
    "unit": "USD",
    "note": "$70k expenses - $20k revenue = $50k burn"
  },
  "runway_months": {
    "value": 4.0,
    "tolerance": 0.1,
    "unit": "months",
    "note": "$200k / $50k burn = 4 months"
  },
  "runway_end_date": {
    "value": "2026-03-18",
    "note": "Assumes today is 2025-11-18, 4 months forward"
  },
  "daily_burn_rate": {
    "value": 1666.67,
    "tolerance": 1,
    "unit": "USD"
  },
  "runway_base_case": {
    "value": 4.0,
    "tolerance": 0.1,
    "unit": "months"
  },
  "runway_upside": {
    "value": 5.0,
    "tolerance": 0.1,
    "unit": "months",
    "note": "Revenue +20% = $24k, burn = $46k, runway = $200k / $46k"
  },
  "runway_downside": {
    "value": 2.9,
    "tolerance": 0.1,
    "unit": "months",
    "note": "Revenue -20% = $16k, burn = $54k, runway = $200k / $54k"
  },
  "breakeven_revenue_needed": {
    "value": 70000,
    "tolerance": 0,
    "unit": "USD"
  }
}
```

**Expected Warnings**:
- LOW_RUNWAY: "Warning: 4.0 months of runway. Typical fundraising takes 3-6 months. Start fundraising process now."
- DOWNSIDE_CRITICAL: "In downside scenario (revenue -20%), runway drops to 2.9 months. Build cash buffer for safety."

### Scenario 3: Cash Positive - Infinite Runway

**Description**: Profitable business, revenue exceeds expenses. No runway concern.

**Inputs**:
```json
{
  "current_cash_balance": 300000,
  "monthly_revenue": 150000,
  "monthly_expenses": 120000
}
```

**Expected Outputs**:
```json
{
  "monthly_burn_rate": {
    "value": -30000,
    "tolerance": 0,
    "unit": "USD",
    "note": "Negative burn = cash positive (revenue > expenses)"
  },
  "runway_months": {
    "value": "Infinity",
    "note": "Cash positive business, runway is effectively infinite"
  },
  "runway_end_date": {
    "value": "Never (cash positive)",
    "note": "Business generates cash, no end date"
  },
  "daily_burn_rate": {
    "value": -1000,
    "tolerance": 0,
    "unit": "USD",
    "note": "Negative = cash generation"
  },
  "runway_base_case": {
    "value": "Infinity",
    "note": "Cash positive"
  },
  "runway_upside": {
    "value": "Infinity",
    "note": "Revenue +20% = $180k, burn = -$60k (even more cash positive)"
  },
  "runway_downside": {
    "value": "Infinity",
    "note": "Revenue -20% = $120k, burn = $0 (breakeven)"
  },
  "breakeven_revenue_needed": {
    "value": 120000,
    "tolerance": 0,
    "unit": "USD",
    "note": "Already above breakeven"
  }
}
```

**Expected Warnings**:
- CASH_POSITIVE: "Business is cash positive (revenue > expenses). Runway is effectively infinite. Strong position."

---

## 8. AI Narrative Strategy (AI-Enabled)

### What AI Explains for This Calculator

**Primary Explanations**:
1. **What the runway number means and what to do about it** (most common: "is 15 months good?")
2. **When to start fundraising** (rule of thumb: start when 12-18 months runway)
3. **Upside vs downside scenarios** (sensitivity to revenue changes)
4. **Path to breakeven and funding needed** (if revenue is growing)

**Scenario Suggestions**:
- "What if we cut expenses by 20%?" (extend runway)
- "What if we hire 2 more people ($15k/month each)?" (reduce runway)
- "When should we start our Series A fundraise?" (timing recommendation)

### Prompt Template

**System Prompt**:
```
You are a CFO advisor helping startup founders and business owners understand cash runway and burn rate.

Your role is to:
- Explain what the runway number means in plain language (how long until cash runs out)
- Interpret whether the runway is healthy, concerning, or critical
- Suggest when to start fundraising (typically 12-18 months runway for startups)
- Explain upside/downside scenarios and sensitivity to revenue changes

Output constraints:
- Length: 150-250 words
- Tone: Direct and actionable (founders need clear guidance)
- Format: Plain paragraphs (no bullet lists)
- Disclaimers: Always end with standard disclaimer

Prohibited behaviors:
- Never guarantee fundraising success (use "typically", "most investors prefer")
- Never recommend specific investors or valuation
- Never use absolute language (avoid "always", "never", "must")
```

**User Prompt Template**:
```
The business has ${current_cash_balance} in cash, monthly revenue of ${monthly_revenue}, and monthly expenses of ${monthly_expenses}. Monthly burn rate is ${monthly_burn_rate}. Calculated runway is {runway_months} months (ending {runway_end_date}). Upside scenario (revenue +20%): {runway_upside} months. Downside scenario (revenue -20%): {runway_downside} months. Explain what this means and what the founder should do.
```

**Expected AI Response** (150-250 words):
```
With a monthly burn rate of $50,000, your current cash balance of $750,000 provides 15 months of runway, lasting until February 2027. This is a healthy position for a startup—you have enough time to execute your roadmap without immediate fundraising pressure.

Most venture-backed companies begin fundraising when they have 12-18 months of runway remaining, as the fundraising process typically takes 3-6 months. Given your current position, you could comfortably wait 3-6 months before starting a fundraising process, but monitoring burn rate monthly is critical.

Your upside scenario (revenue +20% to $60k/month) extends runway to 18.8 months, while the downside scenario (revenue -20% to $40k/month) reduces runway to 12.5 months. This sensitivity shows the importance of hitting revenue targets. If revenue underperforms, you'll have less margin for error.

To reach breakeven, you need monthly revenue of $100,000 (currently $50k). If revenue is growing at 10% per month, you could reach breakeven in approximately 7-8 months. Until then, focus on efficient growth and plan for additional funding if needed.

This analysis is for informational purposes only and does not constitute financial, legal, or professional advice. Consult qualified professionals for decisions affecting your business.
```

---

## 9. Export Format Details

### PDF Layout

**Header**:
- "Cash Runway & Burn Rate Analysis"
- User tier: "Free" or "Pro"
- Generation timestamp: "Generated on November 18, 2025"

**Body Sections**:
1. **Current Cash Position** (table):
   - Cash Balance: $750,000
   - As of: November 18, 2025

2. **Monthly Cash Flow** (table):
   - Monthly Revenue: $50,000
   - Monthly Expenses: $100,000
   - Monthly Burn Rate: $50,000
   - Daily Burn Rate: $1,667

3. **Runway Analysis** (table):
   - Base Case Runway: 15.0 months
   - Runway End Date: February 18, 2027
   - Status: Healthy

4. **Scenario Analysis** (Pro tier only, table):
   - Upside Scenario (+20% revenue): 18.8 months
   - Base Case: 15.0 months
   - Downside Scenario (-20% revenue): 12.5 months

5. **Cash Runway Timeline Chart**:
   - Line chart showing cash declining over 15 months
   - Free tier: Base case only
   - Pro tier: Base/upside/downside scenarios

6. **Breakeven Analysis** (Pro tier only, table):
   - Breakeven Revenue Needed: $100,000/month
   - Current Revenue: $50,000/month
   - Revenue Gap: $50,000/month
   - Months to Breakeven: (if revenue growing) or "Not achievable at current growth"

7. **Recommendations** (if AI tier):
   - AI-generated insights and recommendations

8. **Warnings** (if any):
   - List all warnings with severity icons

**Footer**:
- Disclaimers: "This analysis is for informational purposes only and does not constitute financial advice. Actual cash flow may vary. Monitor cash position regularly and adjust plans accordingly."
- Version stamp: "Generated by CFO Calculator Suite v1.2.3 | Calculator: cash-runway v1.0.0 | Formulas: v1.0.0 | 2025-11-18"
- Watermark (Free tier only): Diagonal semi-transparent "Upgrade for clean exports"

### CSV/Excel Structure

**Metadata Headers**:
```csv
# Cash Runway & Burn Rate Analysis
# Version: v1.0.0
# Generated: 2025-11-18T21:30:00Z
# Tier: Pro
#
```

**Data Rows**:
```csv
Section,Field,Value
Cash Position,Current Balance,$750000
Cash Position,As of Date,2025-11-18
Monthly Cash Flow,Revenue,$50000
Monthly Cash Flow,Expenses,$100000
Monthly Cash Flow,Burn Rate,$50000
Monthly Cash Flow,Daily Burn,$1667
Runway Analysis,Base Case,15.0 months
Runway Analysis,End Date,2027-02-18
Runway Analysis,Status,Healthy
Scenario Analysis,Upside (+20% revenue),18.8 months
Scenario Analysis,Downside (-20% revenue),12.5 months
Breakeven Analysis,Revenue Needed,$100000
Breakeven Analysis,Current Revenue,$50000
Breakeven Analysis,Gap,$50000
```

**Excel Formatting**:
- Bold section headers
- Currency cells: $#,##0.00
- Date cells: MMM DD, YYYY
- Conditional formatting:
  - Runway < 3 months: Red
  - Runway 3-6 months: Yellow
  - Runway > 6 months: Green

---

## 10. Tier-Specific Behavior

### Free Tier

**What's Shown**:
- Single scenario (current cash position only)
- Basic runway calculation (base case)
- Monthly burn rate, runway months, end date
- Simple cash runway timeline chart (base case only)

**What's Gated**:
- **Scenario analysis**: Upside/downside scenarios locked with tooltip "Upgrade to Pro to see scenario analysis"
- **Breakeven analysis**: Locked
- **Historical burn trend**: Requires Pro
- **Multiple scenarios**: "Add Scenario" button triggers upgrade prompt
- **Clean exports**: PDF has watermark
- **AI insights**: "Get AI recommendations" button triggers AI tier upgrade prompt

**Upgrade Triggers**:
- Click "See Scenario Analysis" → `upgrade_prompt_shown` with `trigger_reason: "locked_metric"`
- Click "Breakeven Analysis" → `upgrade_prompt_shown` with `trigger_reason: "locked_metric"`
- Click "Add Scenario" → `upgrade_prompt_shown` with `trigger_reason: "add_scenario"`
- Click "Get AI recommendations" → `upgrade_prompt_shown` with `trigger_reason: "ai_request"`

### Pro Tier

**What Unlocks**:
- **Multiple scenarios** (up to 50): Track cash position over time, compare "what if" scenarios
- **Scenario analysis**: Upside (+20%), base case, downside (-20%) revenue scenarios
- **Breakeven analysis**: Revenue needed to reach zero burn, months to breakeven
- **Total funding needed**: Calculate cash needed to reach breakeven
- **Scenario comparison chart**: Multi-line chart showing base/upside/downside
- **Historical burn trend**: Track burn rate changes over time (if historical data provided)
- **Clean exports**: PDF with no watermark

**Still Gated** (requires AI tier):
- AI recommendations (when to fundraise, how to extend runway)
- AI scenario suggestions

### AI Tier

**What Unlocks**:
- **AI runway insights**: Click "Get recommendations" for guidance on fundraising timing
- **AI scenario suggestions**: "What if I cut expenses by 20%?" gets detailed analysis
- **AI fundraising timing**: "When should I start fundraising?" gets personalized recommendation
- **50 AI requests per month** (hard cap)

**Usage Quota**:
- Counter displayed: "23 of 50 AI requests remaining this month"
- Resets on 1st of each month

---

## Implementation Notes

**Priority**: High (founder/CFO awareness, AI showcase, viral potential)

**Estimated Effort**: 1.5 weeks
- Week 1: Core burn rate and runway calculations
- Week 2 (half): Scenario analysis, breakeven calculation, testing

**Dependencies**:
- Date calculation library (for runway end date)
- Scenario simulation engine (month-by-month cash flow projection)

**Related Calculators**:
- **Business Loan + DSCR**: Debt service affects burn rate
- **Breakeven & Contribution Margin**: Related breakeven concept
- **Business Valuation**: Cash runway affects valuation (short runway = lower valuation)

**Viral/Sharing Features**:
- "Share with co-founders" button (email or link sharing)
- "Export for board deck" PDF with clean formatting (Pro tier)
- Social sharing: "We have X months of runway" with chart image (encourages sharing)

---

## End of Calculator PDR

This calculator is complete and ready for implementation. All burn rate, runway, and scenario formulas are specified in detail.
