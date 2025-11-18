# Invoice Factoring / AR Financing Intelligence Calculator

## 1. Calculator Overview

**Calculator Name**: Invoice Factoring / AR Financing Intelligence Calculator

**Calculator Slug**: `invoice-factoring`

**Primary Category**: Financing & Lending Intelligence

**Secondary Category**: Cash Flow & Liquidity Intelligence

**Business Role**:
- **Niche traffic magnet**: Targeted searches ("invoice factoring calculator", "AR financing cost")
- **Affiliate revenue driver**: High-intent users (partner with factoring companies like Fundbox, BlueVine)
- **Pro upgrade driver**: Effective APR calculation and cost comparison drive conversions
- **B2B demo calculator**: White-label for factoring companies showing value to prospects

**Primary Success Metric**: Affiliate referral rate (target: 12-18% of users click factoring partner links)

**Version**: v1.0.0

**Formula Library Version**: v1.0.0

---

## 2. User Scenarios and Use Cases

### Who Uses This Calculator

**Primary Personas**:
- **Small business owners** with long payment terms (30-90 days) needing immediate cash
- **B2B service providers** (agencies, consultants, contractors) with large outstanding invoices
- **Manufacturing and wholesale distributors** with AR tied up in customer credit terms

**Secondary Personas**:
- **CFOs** evaluating factoring vs line of credit for working capital
- **Factoring companies** demonstrating costs to prospective clients
- **Business advisors** helping clients evaluate AR financing options

### When They Use It (Decision Contexts)

**Timing**:
- **Before factoring decision**: Evaluating if factoring cost is acceptable vs waiting for payment
- **Comparing factoring offers**: Multiple factoring companies offer different rates/terms
- **Comparing to alternatives**: Factoring vs line of credit vs business loan
- **Cash flow crisis**: Need immediate cash, evaluating all options
- **Recurring use**: Monthly factoring users evaluating ongoing costs

**Frequency**:
- **One-time use** (Free tier): Single invoice factoring decision
- **Recurring monthly** (Pro tier): Businesses that factor regularly tracking cumulative costs

### What Questions It Answers

This calculator helps users answer:

1. **How much cash will I get from factoring this invoice?** (Advance amount after fees)
2. **What is the factoring fee in dollars?** (Total cost of immediate cash)
3. **What is the effective APR of factoring?** (Annualized cost for comparison to loans)
4. **Is factoring cheaper than a line of credit?** (Cost comparison, Pro feature)
5. **What if I wait 30 days instead of factoring now?** (Time value of money analysis)

---

## 3. Inputs

### Input Field Definitions

| Field Name | Type | Units | Required | Validation | Default | Placeholder | Tooltip |
|------------|------|-------|----------|------------|---------|-------------|---------|
| invoice_amount | currency | dollars | Yes | min: 1, max: 10000000 | null | "50000" | "Total invoice amount you want to factor. Most factoring companies have minimums ($5k-$10k) and maximums." |
| factoring_advance_rate | percentage | percent | Yes | min: 50, max: 100 | 85 | "85" | "Percentage of invoice advanced immediately. Typical: 80-90%. Higher rates may have higher fees." |
| factoring_fee_percent | percentage | percent | Yes | min: 0, max: 10 | 3 | "3" | "Factoring fee as percentage of invoice. Typical: 1-5% depending on industry, customer creditworthiness, and payment terms. Lower for 30 days, higher for 90+ days." |
| days_to_payment | number | days | Yes | min: 1, max: 365 | 60 | "60" | "Expected days until customer pays invoice. Typical payment terms: 30, 60, or 90 days. Longer terms = higher factoring fees." |

### Input Groups

**Group 1: Invoice Details** (always visible)
- invoice_amount
- days_to_payment

**Group 2: Factoring Terms** (always visible)
- factoring_advance_rate
- factoring_fee_percent

**Group 3: Alternative Financing Comparison** (collapsed by default, Pro tier feature)
- **alternative_apr** (percentage): APR for alternative financing (line of credit, business loan). Required for cost comparison. Default: null
- **alternative_draw_fee** (currency): Upfront fee for alternative financing (e.g., LOC draw fee). Default: 0

### Optional Inputs

**Advanced Options** (collapsed, for sophisticated users):
- **reserve_released_with_payment** (boolean): Does factor release reserve when customer pays? Most factors do. Default: Yes
- **additional_fees** (currency): Other fees (application, due diligence, wire fees). Typical: $100-$500. Default: 0

---

## 4. Outputs

### Key Metrics (Free Tier - Always Visible)

| Output Name | Description | Format | Units |
|-------------|-------------|--------|-------|
| advance_amount | Cash received immediately (invoice × advance rate) | "$#,##0.00" | dollars |
| factoring_cost | Total factoring fee (invoice × fee %) | "$#,##0.00" | dollars |
| net_proceeds | Advance minus factoring cost | "$#,##0.00" | dollars |
| reserve_amount | Amount held back until customer pays | "$#,##0.00" | dollars |

### Advanced Metrics (Pro Tier - Gated)

| Output Name | Description | Format | Units | Pro Only |
|-------------|-------------|--------|-------|----------|
| effective_apr | Annualized percentage rate (cost / advance / days × 365) | "#.00%" | percent | Yes |
| cost_per_day | Daily cost of factoring | "$#,##0.00" | dollars | Yes |
| total_fees_if_recurring | Annual cost if factoring monthly invoices | "$#,##0.00" | dollars | Yes |
| cost_vs_line_of_credit | Cost difference: factoring vs LOC | "$#,##0.00" | dollars | Yes |
| breakeven_days | Days at which factoring cost equals LOC cost | "# days" | days | Yes |
| net_proceeds_after_all_fees | Advance minus all fees (factoring + additional) | "$#,##0.00" | dollars | Yes |

### Charts and Visualizations

**Chart 1: Cash Flow Timeline** (Free tier, basic; Pro tier, enhanced)
- Type: Waterfall chart
- X-axis: Timeline (Day 0, Customer payment day)
- Y-axis: Cash flows
- Bars: Invoice amount → Advance → Factoring fee → Reserve released
- Purpose: Visualize cash flow timing with vs without factoring
- Tier: Free (basic), Pro (with alternative financing comparison)

**Chart 2: Cost Comparison** (Pro tier only)
- Type: Bar chart
- X-axis: Financing option (Factoring, Line of Credit, Wait for Payment)
- Y-axis: Total cost or effective APR
- Purpose: Compare factoring cost to alternatives
- Tier: Pro

**Chart 3: Effective APR Over Time** (Pro tier only)
- Type: Line chart
- X-axis: Days to payment (30, 60, 90, 120, 180)
- Y-axis: Effective APR
- Purpose: Show how APR increases with longer payment terms
- Tier: Pro

### Output Formatting Rules

- **Currency**: $#,##0.00 (two decimals)
- **Percentages**: #.00% (two decimals for APR precision)
- **Days**: Integer (no decimals)
- **APR**: Always annualized (multiply by 365 / days)

---

## 5. Formulas and Calculation Logic

### Formula 1: Advance Amount

**Purpose**: Calculate immediate cash received from factoring company

**Equation**:
```
advance_amount = invoice_amount × (factoring_advance_rate / 100)
```

**Variables**:
- invoice_amount: Total invoice value
- factoring_advance_rate: Percentage advanced immediately (typically 80-90%)
- advance_amount: Cash received today

**Source/Reference**: Standard factoring advance calculation

**Example**: $50,000 invoice × 85% = $42,500 advance

### Formula 2: Reserve Amount

**Purpose**: Calculate amount held back by factor until customer pays

**Equation**:
```
reserve_amount = invoice_amount - advance_amount
reserve_amount = invoice_amount × (1 - factoring_advance_rate / 100)
```

**Variables**:
- reserve_amount: Amount held back (released when customer pays)

**Source/Reference**: Standard factoring reserve calculation

**Example**: $50,000 invoice - $42,500 advance = $7,500 reserve

### Formula 3: Factoring Cost (Fee)

**Purpose**: Calculate total factoring fee in dollars

**Equation**:
```
factoring_cost = invoice_amount × (factoring_fee_percent / 100)
```

**Variables**:
- factoring_fee_percent: Factoring fee as percentage of invoice (typically 1-5%)
- factoring_cost: Dollar amount of fee

**Source/Reference**: Standard factoring fee calculation

**Example**: $50,000 × 3% = $1,500 factoring fee

### Formula 4: Net Proceeds

**Purpose**: Calculate net cash received after factoring fee

**Equation**:
```
# When customer pays, factor releases reserve minus fee
net_proceeds = advance_amount + (reserve_amount - factoring_cost)

# Simplified
net_proceeds = invoice_amount - factoring_cost

# If additional fees exist
net_proceeds_after_all_fees = net_proceeds - additional_fees
```

**Variables**:
- net_proceeds: Total cash received (advance now + reserve later - fee)

**Example**: $42,500 advance + ($7,500 reserve - $1,500 fee) = $48,500 net proceeds

### Formula 5: Effective APR

**Purpose**: Calculate annualized cost of factoring for comparison to loans

**Equation**:
```
# Effective APR formula
# APR = (Fee / Advance) × (365 / Days) × 100

effective_apr = (factoring_cost / advance_amount) × (365 / days_to_payment) × 100
```

**Variables**:
- factoring_cost: Dollar amount of fee
- advance_amount: Cash received today (denominator is advance, not invoice)
- days_to_payment: Days until customer pays (factor earns return over this period)
- effective_apr: Annualized percentage rate

**Source/Reference**: APR calculation methodology (Truth in Lending Act)

**Example**: ($1,500 fee / $42,500 advance) × (365 / 60 days) × 100 = 21.47% APR

**Interpretation**:
- 10-15% APR = Competitive factoring (good customer creditworthiness, 30-day terms)
- 15-25% APR = Average factoring (60-90 day terms)
- 25-40% APR = Expensive factoring (risky customers, long terms, or small invoices)
- 40%+ APR = Very expensive (consider alternatives)

### Formula 6: Cost per Day

**Purpose**: Calculate daily cost of factoring (useful for timing decisions)

**Equation**:
```
cost_per_day = factoring_cost / days_to_payment
```

**Example**: $1,500 fee / 60 days = $25 per day

**Use case**: "If customer pays 10 days early, I save $250"

### Formula 7: Recurring Annual Cost (if factoring monthly)

**Purpose**: Calculate annual cost if business factors invoices regularly

**Equation**:
```
# Assume factoring one invoice per month
annual_invoices = 12
total_fees_if_recurring = factoring_cost × annual_invoices

# More accurate: account for seasonal variation
# If user provides average monthly invoice amount
average_monthly_invoice = invoice_amount  # assume provided invoice is typical
annual_factoring_cost = (average_monthly_invoice × factoring_fee_percent / 100) × 12
```

**Example**: $1,500 fee per invoice × 12 months = $18,000 annual factoring cost

### Formula 8: Cost vs Line of Credit Comparison

**Purpose**: Compare factoring cost to line of credit for same time period

**Equation**:
```
# Factoring cost
factoring_total_cost = factoring_cost + additional_fees

# Line of credit cost (for same amount and time period)
# LOC interest = Principal × APR × (Days / 365)
loc_interest_cost = advance_amount × (alternative_apr / 100) × (days_to_payment / 365)
loc_total_cost = loc_interest_cost + alternative_draw_fee

# Cost difference
cost_difference = factoring_total_cost - loc_total_cost

# Percentage difference
percent_difference = (cost_difference / loc_total_cost) × 100

if cost_difference > 0:
    recommendation = "Line of credit is cheaper by ${:,.0f}".format(abs(cost_difference))
elif cost_difference < 0:
    recommendation = "Factoring is cheaper by ${:,.0f}".format(abs(cost_difference))
else:
    recommendation = "Costs are approximately equal"
```

**Source/Reference**: Time-adjusted cost comparison methodology

### Formula 9: Breakeven Days

**Purpose**: Calculate at what payment delay factoring and LOC have equal cost

**Equation**:
```
# Solve for days where factoring_cost = LOC_cost
# factoring_cost = fixed (e.g., 3% of invoice)
# LOC_cost = advance_amount × (alternative_apr / 100) × (days / 365)

# Solve: factoring_cost = advance_amount × (alternative_apr / 100) × (days / 365)
# days = (factoring_cost × 365) / (advance_amount × alternative_apr / 100)

breakeven_days = (factoring_cost × 365) / (advance_amount × alternative_apr / 100)
```

**Interpretation**: If customer pays before breakeven days, LOC is cheaper. After breakeven, factoring may be cheaper (fixed cost vs accumulating interest).

### Step-by-Step Calculation Flow

1. **Validate inputs**: Check required fields, ensure advance rate 50-100%, fee 0-10%
2. **Calculate advance amount**: `invoice_amount × advance_rate`
3. **Calculate reserve amount**: `invoice_amount - advance_amount`
4. **Calculate factoring cost**: `invoice_amount × factoring_fee_percent`
5. **Calculate net proceeds**: `invoice_amount - factoring_cost`
6. **Calculate effective APR**: `(factoring_cost / advance_amount) × (365 / days) × 100`
7. **Calculate cost per day**: `factoring_cost / days_to_payment`
8. **If alternative APR provided** (Pro tier):
   - Calculate LOC cost for same period
   - Calculate cost difference
   - Calculate breakeven days
   - Determine recommendation
9. **If recurring use indicated** (Pro tier):
   - Calculate annual factoring cost (fee × 12)
10. **Check warning conditions**: High APR, expensive vs alternatives, etc.
11. **Return results with version stamp**

### Edge Cases and Handling

| Edge Case | Condition | Handling | Warning/Error |
|-----------|-----------|----------|---------------|
| 100% advance rate | factoring_advance_rate = 100 | Reserve = 0, unusual but calculate | Info: "100% advance rate is uncommon. Verify factoring terms." |
| Very short term | days_to_payment < 15 | Calculate as normal, APR will be very high | Warning: "Short payment term results in high effective APR. Consider waiting for payment." |
| Very long term | days_to_payment > 120 | Calculate as normal | Warning: "Long payment term ({days} days). Factoring may be very expensive. Consider alternative financing." |
| Low advance rate | factoring_advance_rate < 70% | Calculate as normal | Warning: "Low advance rate ({rate}%). Most factors offer 80-90%. Shop for better terms." |
| Very high fee | factoring_fee_percent > 5% | Calculate as normal | Warning: "High factoring fee ({fee}%). Typical fees are 1-5%. Shop for better rates." |

### Formula Version

**Current Version**: v1.0.0

**Version History**:
- v1.0.0: Initial implementation (advance, fee, effective APR, cost comparison)

---

## 6. Warnings and Alerts

### Warning Definitions

| Warning Code | Condition | Message | Severity | Action |
|--------------|-----------|---------|----------|--------|
| HIGH_EFFECTIVE_APR | effective_apr > 30% | "Effective APR of {apr}% is very high. Typical factoring is 15-25% APR. Consider shopping for better rates or alternative financing (line of credit, business loan)." | warning | Shop for better factoring rates or explore LOC |
| VERY_HIGH_APR | effective_apr > 50% | "Effective APR of {apr}% is extremely high. This is predatory pricing. Strongly consider alternative financing or waiting for customer payment." | danger | Avoid this factoring offer, explore alternatives |
| LOW_ADVANCE_RATE | factoring_advance_rate < 75% | "Advance rate of {rate}% is low. Most factoring companies offer 80-90%. You're leaving ${amount:,.0f} on the table. Shop for better terms." | warning | Negotiate higher advance rate or find different factor |
| FACTORING_MORE_EXPENSIVE | cost_difference > 500 && factoring_total_cost > loc_total_cost | "Factoring is ${cost_difference:,.0f} more expensive than a line of credit for this invoice. If you have LOC access, use it instead." | warning | Use line of credit instead of factoring |
| LOC_MORE_EXPENSIVE | cost_difference < -500 && loc_total_cost > factoring_total_cost | "Factoring is ${cost_difference:,.0f} cheaper than a line of credit for this short-term need. Factoring may be the better option." | info | Factoring is cost-effective for this situation |
| LONG_PAYMENT_TERM | days_to_payment > 90 | "Payment term of {days} days is long (typical: 30-60 days). Factoring cost increases with longer terms. Consider negotiating shorter payment terms with customers." | warning | Negotiate shorter payment terms or extend credit more carefully |
| RECURRING_HIGH_COST | total_fees_if_recurring > 10000 | "If factoring monthly, annual cost would be ${annual_cost:,.0f}. This is expensive for recurring working capital. Consider line of credit or improving collections." | warning | Explore LOC, improve collections, or negotiate better terms |

### Warning Thresholds

**Configurable Thresholds** (can be adjusted per tenant for B2B):
- **High effective APR**: 30% (above typical factoring range)
- **Very high APR**: 50% (predatory pricing)
- **Low advance rate**: 75% (below market standard)

**Hard-Coded Thresholds** (industry standards, not configurable):
- **Typical APR range**: 15-25% (factual market data)
- **Typical advance rate**: 80-90% (factual market data)
- **Typical fee range**: 1-5% (factual market data)

---

## 7. Golden Test Scenarios

### Scenario 1: Standard Factoring - 60-Day Terms

**Description**: Typical factoring scenario. 60-day payment terms, 85% advance, 3% fee. Effective APR is 21.5% (moderate).

**Inputs**:
```json
{
  "invoice_amount": 50000,
  "factoring_advance_rate": 85,
  "factoring_fee_percent": 3,
  "days_to_payment": 60,
  "alternative_apr": 12,
  "alternative_draw_fee": 0
}
```

**Expected Outputs**:
```json
{
  "advance_amount": {
    "value": 42500,
    "tolerance": 0,
    "unit": "USD",
    "note": "$50,000 × 85% = $42,500"
  },
  "reserve_amount": {
    "value": 7500,
    "tolerance": 0,
    "unit": "USD",
    "note": "$50,000 - $42,500 = $7,500"
  },
  "factoring_cost": {
    "value": 1500,
    "tolerance": 0,
    "unit": "USD",
    "note": "$50,000 × 3% = $1,500"
  },
  "net_proceeds": {
    "value": 48500,
    "tolerance": 0,
    "unit": "USD",
    "note": "$50,000 - $1,500 = $48,500"
  },
  "effective_apr": {
    "value": 21.47,
    "tolerance": 0.1,
    "unit": "percent",
    "note": "($1,500 / $42,500) × (365 / 60) × 100 = 21.47%"
  },
  "cost_per_day": {
    "value": 25.00,
    "tolerance": 0.1,
    "unit": "USD",
    "note": "$1,500 / 60 days = $25/day"
  },
  "total_fees_if_recurring": {
    "value": 18000,
    "tolerance": 0,
    "unit": "USD",
    "note": "$1,500 × 12 months = $18,000 annual"
  },
  "cost_vs_line_of_credit": {
    "factoring_cost": 1500,
    "loc_cost": 840,
    "cost_difference": 660,
    "note": "LOC: $42,500 × 12% × (60/365) = $840. Factoring $660 more expensive."
  },
  "breakeven_days": {
    "value": 106,
    "tolerance": 1,
    "unit": "days",
    "note": "After 106 days, LOC would cost same as factoring"
  }
}
```

**Expected Warnings**:
- FACTORING_MORE_EXPENSIVE: "Factoring is $660 more expensive than a line of credit for this invoice..."

### Scenario 2: Expensive Factoring - 90-Day Terms, High Fee

**Description**: Expensive factoring scenario. 90-day terms, 80% advance, 5% fee. Effective APR is 50.7% (very high).

**Inputs**:
```json
{
  "invoice_amount": 30000,
  "factoring_advance_rate": 80,
  "factoring_fee_percent": 5,
  "days_to_payment": 90,
  "alternative_apr": 15,
  "alternative_draw_fee": 100
}
```

**Expected Outputs**:
```json
{
  "advance_amount": {
    "value": 24000,
    "tolerance": 0,
    "unit": "USD",
    "note": "$30,000 × 80% = $24,000"
  },
  "reserve_amount": {
    "value": 6000,
    "tolerance": 0,
    "unit": "USD"
  },
  "factoring_cost": {
    "value": 1500,
    "tolerance": 0,
    "unit": "USD",
    "note": "$30,000 × 5% = $1,500"
  },
  "net_proceeds": {
    "value": 28500,
    "tolerance": 0,
    "unit": "USD"
  },
  "effective_apr": {
    "value": 50.69,
    "tolerance": 0.1,
    "unit": "percent",
    "note": "($1,500 / $24,000) × (365 / 90) × 100 = 50.69%"
  },
  "cost_per_day": {
    "value": 16.67,
    "tolerance": 0.1,
    "unit": "USD"
  },
  "total_fees_if_recurring": {
    "value": 18000,
    "tolerance": 0,
    "unit": "USD"
  },
  "cost_vs_line_of_credit": {
    "factoring_cost": 1500,
    "loc_cost": 987,
    "cost_difference": 513,
    "note": "LOC: $24,000 × 15% × (90/365) + $100 fee = $987. Factoring $513 more expensive."
  }
}
```

**Expected Warnings**:
- VERY_HIGH_APR: "Effective APR of 50.69% is extremely high. This is predatory pricing..."
- LOW_ADVANCE_RATE: "Advance rate of 80% is low. Most factoring companies offer 80-90%..."
- LONG_PAYMENT_TERM: "Payment term of 90 days is long..."
- FACTORING_MORE_EXPENSIVE: "Factoring is $513 more expensive than a line of credit..."

### Scenario 3: Competitive Factoring - 30-Day Terms

**Description**: Competitive factoring scenario. Short 30-day terms, 90% advance, 1.5% fee. Effective APR is 20.3% (competitive).

**Inputs**:
```json
{
  "invoice_amount": 100000,
  "factoring_advance_rate": 90,
  "factoring_fee_percent": 1.5,
  "days_to_payment": 30,
  "alternative_apr": 10,
  "alternative_draw_fee": 0
}
```

**Expected Outputs**:
```json
{
  "advance_amount": {
    "value": 90000,
    "tolerance": 0,
    "unit": "USD"
  },
  "reserve_amount": {
    "value": 10000,
    "tolerance": 0,
    "unit": "USD"
  },
  "factoring_cost": {
    "value": 1500,
    "tolerance": 0,
    "unit": "USD",
    "note": "$100,000 × 1.5% = $1,500"
  },
  "net_proceeds": {
    "value": 98500,
    "tolerance": 0,
    "unit": "USD"
  },
  "effective_apr": {
    "value": 20.28,
    "tolerance": 0.1,
    "unit": "percent",
    "note": "($1,500 / $90,000) × (365 / 30) × 100 = 20.28%"
  },
  "cost_per_day": {
    "value": 50.00,
    "tolerance": 0.1,
    "unit": "USD"
  },
  "cost_vs_line_of_credit": {
    "factoring_cost": 1500,
    "loc_cost": 740,
    "cost_difference": 760,
    "note": "LOC: $90,000 × 10% × (30/365) = $740"
  }
}
```

**Expected Warnings**:
- FACTORING_MORE_EXPENSIVE: "Factoring is $760 more expensive than a line of credit for this invoice..."

---

## 8. AI Narrative Strategy (AI-Enabled)

### What AI Explains for This Calculator

**Primary Explanations**:
1. **What the effective APR means and whether it's reasonable** (industry context, comparison to alternatives)
2. **When factoring makes sense vs waiting or using LOC** (trade-offs: immediate cash vs cost)
3. **How to negotiate better factoring terms** (advance rate, fee, payment terms)
4. **Recurring cost implications** (annual cost if factoring monthly)

**Scenario Suggestions**:
- "What if I negotiate the fee down to 2%?" (cost savings)
- "What if I can get customers to pay in 45 days instead of 60?" (reduce factoring cost)
- "Should I use factoring or wait for payment?" (cash flow vs cost trade-off)

### Prompt Template

**System Prompt**:
```
You are a CFO advisor helping business owners understand invoice factoring costs and whether factoring makes sense for their situation.

Your role is to:
- Explain effective APR in plain language and interpret whether factoring cost is reasonable
- Compare factoring to alternatives (line of credit, waiting for payment)
- Suggest when factoring makes sense (cash flow need vs cost)
- Explain trade-offs (immediate cash vs higher cost than traditional financing)

Output constraints:
- Length: 150-250 words
- Tone: Professional but approachable, practical
- Format: Plain paragraphs (no bullet lists)
- Disclaimers: Always end with standard disclaimer

Prohibited behaviors:
- Never recommend specific factoring companies
- Never guarantee factoring approval
- Never use absolute language (avoid "always", "never", "must")
```

**User Prompt Template**:
```
The business has a ${invoice_amount} invoice with {days_to_payment}-day payment terms. Factoring terms: {factoring_advance_rate}% advance, {factoring_fee_percent}% fee. This results in ${advance_amount} immediate cash, ${factoring_cost} fee, effective APR of {effective_apr}%. Alternative line of credit costs {alternative_apr}% APR. Explain whether factoring makes sense.
```

**Expected AI Response** (150-250 words):
```
For your $50,000 invoice with 60-day payment terms, factoring would provide $42,500 in immediate cash (85% advance) with a total fee of $1,500 (3%). The effective APR is 21.5%, which is moderate for invoice factoring—typical rates are 15-25% for 60-day terms.

Comparing to your line of credit at 12% APR, the LOC would cost $840 for the same 60-day period, making factoring $660 more expensive. If you have available LOC capacity, it's the cheaper option. However, if your LOC is maxed out or you don't qualify for one, factoring provides access to working capital that might otherwise be unavailable.

Factoring makes most sense when you have an urgent cash need (payroll, supplies, growth opportunity) and can't wait 60 days for payment. The cost of factoring ($1,500) should be weighed against the opportunity cost or consequence of not having immediate cash.

If you factor invoices regularly (monthly), the annual cost would be approximately $18,000. At that volume, you should explore establishing a line of credit or improving your collections process to reduce days to payment. Many businesses use factoring temporarily while building credit for traditional financing.

Consider negotiating better payment terms with customers (45 days instead of 60) to reduce factoring costs or reliance on factoring.

This analysis is for informational purposes only and does not constitute financial, legal, or professional advice. Consult qualified professionals for decisions affecting your business.
```

---

## 9. Export Format Details

### PDF Layout

**Header**:
- "Invoice Factoring Cost Analysis"
- User tier: "Free" or "Pro"
- Generation timestamp: "Generated on November 18, 2025"

**Body Sections**:
1. **Invoice Details** (table):
   - Invoice Amount: $50,000
   - Payment Terms: 60 days

2. **Factoring Terms** (table):
   - Advance Rate: 85%
   - Factoring Fee: 3%

3. **Cash Flow Summary** (table):
   - Immediate Advance: $42,500
   - Reserve (held until payment): $7,500
   - Factoring Fee: $1,500
   - Net Proceeds: $48,500

4. **Cost Analysis** (table):
   - Effective APR: 21.47%
   - Cost per Day: $25.00
   - Total Fee: $1,500

5. **Cash Flow Timeline** (chart):
   - Waterfall showing: Invoice → Advance → Fee → Reserve released
   - Free tier: Basic chart
   - Pro tier: Enhanced with alternative financing comparison

6. **Alternative Financing Comparison** (Pro tier only, table):
   - Factoring Cost: $1,500 (APR: 21.47%)
   - Line of Credit Cost: $840 (APR: 12%)
   - Cost Difference: $660 (factoring more expensive)
   - Recommendation: Use line of credit if available

7. **Recurring Cost Analysis** (Pro tier only, table):
   - Monthly Invoice: $50,000
   - Monthly Fee: $1,500
   - Annual Cost (if factoring monthly): $18,000

8. **Warnings** (if any):
   - List all warnings with severity icons

**Footer**:
- Disclaimers: "This analysis is for informational purposes only and does not constitute financial advice. Factoring costs and terms vary by provider, industry, and customer creditworthiness. Consult multiple factoring companies and qualified professionals for decisions affecting your business."
- Version stamp: "Generated by CFO Calculator Suite v1.2.3 | Calculator: invoice-factoring v1.0.0 | Formulas: v1.0.0 | 2025-11-18"
- Watermark (Free tier only): Diagonal semi-transparent "Upgrade for clean exports"

### CSV/Excel Structure

**Metadata Headers**:
```csv
# Invoice Factoring Cost Analysis
# Version: v1.0.0
# Generated: 2025-11-18T21:30:00Z
# Tier: Pro
#
```

**Data Rows**:
```csv
Section,Field,Value
Invoice Details,Invoice Amount,$50000
Invoice Details,Payment Terms,60 days
Factoring Terms,Advance Rate,85%
Factoring Terms,Factoring Fee,3%
Cash Flow,Immediate Advance,$42500
Cash Flow,Reserve,$7500
Cash Flow,Factoring Fee,$1500
Cash Flow,Net Proceeds,$48500
Cost Analysis,Effective APR,21.47%
Cost Analysis,Cost per Day,$25.00
Cost Analysis,Total Fee,$1500
Alternative Financing,Factoring Cost,$1500
Alternative Financing,LOC Cost,$840
Alternative Financing,Cost Difference,$660
Alternative Financing,Recommendation,Use LOC if available
Recurring Analysis,Monthly Fee,$1500
Recurring Analysis,Annual Cost,$18000
```

**Excel Formatting**:
- Bold section headers
- Currency cells: $#,##0.00
- Percentage cells: 0.00%
- Conditional formatting:
  - Effective APR < 20%: Green
  - APR 20-30%: Yellow
  - APR > 30%: Red

---

## 10. Tier-Specific Behavior

### Free Tier

**What's Shown**:
- Single invoice analysis (cannot save or compare multiple invoices)
- Invoice amount, advance rate, fee, payment days inputs
- Basic metrics: Advance amount, factoring cost, net proceeds, reserve amount
- Basic cash flow timeline chart

**What's Gated**:
- **Effective APR**: Locked with tooltip "Upgrade to Pro to see effective APR and cost comparison"
- **Cost vs line of credit**: Locked
- **Recurring cost analysis**: Locked
- **Breakeven days**: Locked
- **Multiple invoice comparison**: "Add Invoice" button triggers upgrade prompt
- **Clean exports**: PDF has watermark
- **AI insights**: "Get factoring recommendations" button triggers AI tier upgrade prompt

**Upgrade Triggers**:
- Click "See Effective APR" → `upgrade_prompt_shown` with `trigger_reason: "locked_metric"`
- Click "Compare to LOC" → `upgrade_prompt_shown` with `trigger_reason: "cost_comparison"`
- Click "Add Invoice" → `upgrade_prompt_shown` with `trigger_reason: "add_scenario"`
- Click "Get recommendations" → `upgrade_prompt_shown` with `trigger_reason: "ai_request"`

### Pro Tier

**What Unlocks**:
- **Multiple invoices** (up to 50): Track and compare factoring costs across invoices
- **Effective APR**: Full APR calculation for comparison to other financing
- **Cost vs line of credit**: Detailed comparison showing which is cheaper
- **Breakeven days**: Calculate when factoring and LOC costs are equal
- **Recurring cost analysis**: Annual cost if factoring monthly
- **Cost per day**: Daily factoring cost for timing decisions
- **Invoice comparison table**: Side-by-side comparison of multiple factoring scenarios
- **Clean exports**: PDF with no watermark

**Still Gated** (requires AI tier):
- AI factoring recommendations
- AI scenario suggestions

### AI Tier

**What Unlocks**:
- **AI factoring insights**: "Is this a good factoring deal?" gets detailed analysis
- **AI alternative recommendations**: "Should I factor or wait?" gets trade-off analysis
- **AI negotiation tips**: "How can I negotiate better terms?" gets suggestions
- **50 AI requests per month** (hard cap)

**Usage Quota**:
- Counter displayed: "23 of 50 AI requests remaining this month"
- Resets on 1st of each month

---

## Implementation Notes

**Priority**: Medium (niche traffic, affiliate revenue)

**Estimated Effort**: 1 week
- Days 1-3: Core factoring calculations (advance, fee, APR)
- Days 4-5: Cost comparison logic (vs LOC, breakeven days)
- Days 6-7: Testing, affiliate integration

**Dependencies**:
- APR calculation library (reusable from SBA calculator)
- Date/time calculation for payment terms

**Related Calculators**:
- **Line of Credit**: Direct alternative to factoring
- **Cash Runway**: Factoring affects cash runway (immediate cash infusion)
- **Business Loan + DSCR**: Alternative working capital financing

**Affiliate Integration**:
- Partner with factoring companies (e.g., Fundbox, BlueVine, Triumph Business Capital)
- Partner with AR financing platforms (e.g., C2FO, Taulia)
- "Find Factoring Partners" button below results → affiliate referral
- Track referral conversions for revenue attribution

---

## End of Calculator PDR

This calculator is complete and ready for implementation. All factoring, APR, and cost comparison formulas are specified in detail.
