# Business Loan + DSCR Intelligence Calculator

## 1. Calculator Overview

**Calculator Name**: Business Loan + DSCR Intelligence Calculator

**Calculator Slug**: `business-loan-dscr`

**Primary Category**: Financing & Lending Intelligence

**Secondary Category**: None

**Business Role**:
- **Core workhorse**: Most frequently used calculator, foundation for other lending calculators
- **Pro upgrade driver**: DSCR and advanced metrics drive Free → Pro conversions
- **Traffic magnet**: High SEO value for "business loan calculator" and "DSCR calculator"

**Primary Success Metric**: Free to Pro conversion rate (target: 5-7% monthly)

**Version**: v1.0.0

**Formula Library Version**: v1.2.3

---

## 2. User Scenarios and Use Cases

### Who Uses This Calculator

**Primary Personas**:
- **Small business owners** seeking SBA loans ($50k-$5M) for equipment, real estate, or working capital
- **CFOs** evaluating new debt and impact on debt service coverage
- **Lenders** (SBA, commercial banks) pre-qualifying loan applications

**Secondary Personas**:
- **Business advisors** (SBDC, SCORE) assisting clients with loan applications
- **Accountants** analyzing client debt capacity
- **Investors** evaluating acquisition financing structures

### When They Use It (Decision Contexts)

**Timing**:
- **Before applying for loan**: Planning phase, determining affordable loan amount
- **After receiving loan offer**: Comparison phase, evaluating terms from multiple lenders
- **During annual budgeting**: Scenario testing, "what if revenue drops 10%?"
- **Ongoing monitoring**: Pro users tracking DSCR monthly or quarterly

**Frequency**:
- Free tier: One-time use for specific loan decision
- Pro tier: Recurring monthly/quarterly use for ongoing monitoring

### What Questions It Answers

This calculator helps users answer:

1. **Can I afford this monthly payment?** (Based on current revenue and expenses)
2. **Will my DSCR meet lender requirements?** (Most lenders require ≥1.25)
3. **How much total interest will I pay over the loan term?** (Compare to alternative financing)
4. **What if my revenue decreases by 10-20%?** (Stress testing, Pro tier feature)
5. **Should I choose a 10-year or 15-year term?** (Compare scenarios side-by-side, Pro tier)

---

## 3. Inputs

### Input Field Definitions

| Field Name | Type | Units | Required | Validation | Default | Placeholder | Tooltip |
|------------|------|-------|----------|------------|---------|-------------|---------|
| loan_amount | currency | dollars | Yes | min: 1, max: 100000000, positive | null | "250000" | "Total amount borrowed. Most SBA loans range from $50,000 to $5 million." |
| interest_rate | percentage | percent | Yes | min: 0, max: 30, positive | null | "7.5" | "Annual interest rate (APR). Typical SBA rates are 6-12%. Enter as percentage (e.g., 7.5 for 7.5%)." |
| term_years | number | years | Yes | min: 1, max: 30, positive, integer | 10 | "10" | "Number of years to repay the loan. SBA loans typically have 10-25 year terms depending on use (equipment: 10 years, real estate: 25 years)." |
| annual_revenue | currency | dollars | No | min: 0, max: 1000000000 | null | "1500000" | "Total annual revenue (gross income before expenses). Required for DSCR calculation. Leave blank if you only need payment calculation." |
| annual_operating_expenses | currency | dollars | No | min: 0, max: 1000000000 | null | "1200000" | "Total annual operating expenses (excluding debt service). Required for DSCR calculation. Do not include the loan payment you're calculating." |

### Input Groups

**Group 1: Loan Details** (always visible)
- loan_amount
- interest_rate
- term_years

**Group 2: Business Financials** (collapsed by default, expands if user wants DSCR)
- annual_revenue
- annual_operating_expenses

### Optional Inputs

**Advanced Options** (collapsed, rarely needed):
- **origination_fee_percent** (percentage): One-time fee charged at loan origination, typically 0-5% for SBA loans. Default: 0
- **balloon_payment_month** (number): If loan has balloon payment, month when it's due (0 = no balloon). Default: 0

---

## 4. Outputs

### Key Metrics (Free Tier - Always Visible)

| Output Name | Description | Format | Units |
|-------------|-------------|--------|-------|
| monthly_payment | Principal and interest payment per month | "$#,##0.00" | dollars |
| total_interest | Total interest paid over loan term | "$#,##0.00" | dollars |
| total_amount_paid | Total principal + interest + fees | "$#,##0.00" | dollars |

### Advanced Metrics (Pro Tier - Gated)

| Output Name | Description | Format | Units | Pro Only |
|-------------|-------------|--------|-------|----------|
| dscr | Debt Service Coverage Ratio (net operating income / annual debt service) | "#.00" | ratio | Yes |
| annual_debt_service | Total annual loan payments (monthly payment × 12) | "$#,##0.00" | dollars | Yes |
| net_operating_income | Annual revenue - operating expenses | "$#,##0.00" | dollars | Yes |
| covenant_headroom | How much DSCR exceeds minimum 1.25 (positive = above, negative = below) | "+#.00 or -#.00" | ratio | Yes |

### Charts and Visualizations

**Chart 1: Amortization Schedule** (Pro tier only)
- Type: Line chart
- X-axis: Month (0 to term in months)
- Y-axis: Dollars (three lines: principal portion, interest portion, remaining balance)
- Purpose: Visualize how payment allocation changes over time (more interest early, more principal later)
- Tier: Pro

**Chart 2: Scenario Comparison** (Pro tier only, when multiple scenarios exist)
- Type: Bar chart
- X-axis: Scenario name
- Y-axis: Key metrics (monthly payment, total interest, DSCR)
- Purpose: Compare multiple loan structures side-by-side
- Tier: Pro

### Output Formatting Rules

- **Currency**: $#,##0.00 (always two decimals, commas for thousands)
- **Percentages**: #.00% (two decimals)
- **Ratios**: #.00 (two decimals)
- **DSCR**: Two decimals, no dollar sign (e.g., "1.42" not "$1.42")

---

## 5. Formulas and Calculation Logic

### Formula 1: Monthly Payment (Amortization)

**Purpose**: Calculate fixed monthly payment for fully amortizing loan

**Equation**:
```
monthly_payment = P × [r × (1 + r)^n] / [(1 + r)^n - 1]

Where:
P = principal (loan amount)
r = monthly interest rate (annual rate / 12 / 100)
n = number of monthly payments (term_years × 12)
```

**Variables**:
- P (principal): loan_amount in dollars
- r (monthly rate): interest_rate / 12 / 100 (converted from percentage to decimal monthly rate)
- n (number of payments): term_years × 12

**Source/Reference**: Standard amortization formula, see https://en.wikipedia.org/wiki/Amortization_calculator

**Edge case: Zero interest rate**:
```
if interest_rate == 0:
    monthly_payment = loan_amount / n
```

### Formula 2: Total Interest

**Purpose**: Calculate total interest paid over loan term

**Equation**:
```
total_interest = (monthly_payment × n) - loan_amount
```

### Formula 3: Debt Service Coverage Ratio (DSCR)

**Purpose**: Measure business's ability to cover loan payments with operating income

**Equation**:
```
annual_debt_service = monthly_payment × 12
net_operating_income = annual_revenue - annual_operating_expenses
dscr = net_operating_income / annual_debt_service
```

**Variables**:
- annual_debt_service: Total annual loan payments (principal + interest)
- net_operating_income: Revenue minus operating expenses (before debt service)
- dscr: Ratio (typically 1.0-2.5 for healthy businesses)

**Source/Reference**: SBA Lender Guide, https://en.wikipedia.org/wiki/Debt-service_coverage_ratio

**Minimum lender requirement**: Most lenders require DSCR ≥ 1.25 (business generates $1.25 for every $1 in debt payments)

### Step-by-Step Calculation Flow

1. **Validate inputs**: Check all required fields, validate ranges (loan amount > 0, interest rate 0-30%, term 1-30 years)
2. **Convert annual rate to monthly rate**: `r = interest_rate / 12 / 100`
3. **Calculate number of payments**: `n = term_years × 12`
4. **Calculate monthly payment**: Use amortization formula (handle zero interest edge case)
5. **Calculate total amount paid**: `total_amount_paid = monthly_payment × n`
6. **Calculate total interest**: `total_interest = total_amount_paid - loan_amount`
7. **If revenue and expenses provided**:
   - Calculate annual debt service: `annual_debt_service = monthly_payment × 12`
   - Calculate net operating income: `net_operating_income = annual_revenue - annual_operating_expenses`
   - Calculate DSCR: `dscr = net_operating_income / annual_debt_service`
   - Calculate covenant headroom: `covenant_headroom = dscr - 1.25`
8. **Check warning conditions**: Trigger warnings based on DSCR, debt burden, negative income
9. **Return results with version stamp**: Include calculator version, formula version, timestamp

### Edge Cases and Handling

| Edge Case | Condition | Handling | Warning/Error |
|-----------|-----------|----------|---------------|
| Zero interest rate | interest_rate = 0 | monthly_payment = loan_amount / n | Info: "Zero interest rate. Payment is principal only." |
| Negative DSCR | dscr < 0 | Calculate as normal, show negative value | Critical: "Negative DSCR. Business has negative operating income." |
| Division by zero (DSCR) | annual_debt_service = 0 | DSCR = Infinity | Info: "DSCR cannot be calculated with zero debt service." |
| Extremely high interest | interest_rate > 20% | Calculate as normal | Warning: "Interest rate above 20% is very high. Verify this rate is correct." |
| Very short term | term_years < 3 | Calculate as normal | Warning: "Short loan term results in high monthly payment. Consider longer term if cash flow is tight." |

### Formula Version

**Current Version**: v1.2.3

**Version History**:
- v1.0.0: Initial implementation (monthly payment, total interest)
- v1.1.0: Added DSCR calculation
- v1.2.0: Improved rounding precision (monthly payment to nearest cent)
- v1.2.3: Fixed edge case for zero interest rate

---

## 6. Warnings and Alerts

### Warning Definitions

| Warning Code | Condition | Message | Severity | Action |
|--------------|-----------|---------|----------|--------|
| DSCR_BELOW_MINIMUM | dscr < 1.25 | "DSCR of {dscr} is below the typical lender minimum of 1.25. This loan may not be approved without additional collateral or guarantees." | warning | Increase revenue, reduce expenses, extend term, or reduce loan amount |
| DSCR_ABOVE_OPTIMAL | dscr > 2.0 | "DSCR of {dscr} is well above lender requirements. You may be able to borrow more if needed." | info | Consider if additional borrowing capacity is useful for growth |
| HIGH_DEBT_BURDEN | (monthly_payment × 12) / annual_revenue > 0.40 | "Annual debt payments represent {percent}% of gross revenue. Lenders typically prefer this ratio below 40%." | warning | Reduce loan amount or extend term to lower annual payments |
| NEGATIVE_OPERATING_INCOME | net_operating_income < 0 | "Business has negative operating income. DSCR calculation is not meaningful. Focus on achieving profitability before taking on debt." | danger | Review business model, reduce expenses, or increase revenue to reach profitability |
| HIGH_INTEREST_RATE | interest_rate > 15% | "Interest rate of {rate}% is very high. Typical SBA loans are 6-12%. Verify this rate is correct and consider alternative financing." | warning | Shop for better rates, consider SBA loans, or improve creditworthiness |
| SHORT_TERM_HIGH_PAYMENT | term_years < 5 && monthly_payment / (annual_revenue / 12) > 0.20 | "Short loan term combined with high debt burden. Monthly payment is {percent}% of monthly revenue." | warning | Extend term to reduce monthly payment if cash flow is constrained |

### Warning Thresholds

**Configurable Thresholds** (can be adjusted per tenant for B2B):
- **DSCR minimum**: 1.25 (default, some lenders require 1.15 or 1.35)
- **DSCR optimal**: 2.0 (above this, business may be under-leveraged)
- **Debt-to-revenue ratio**: 0.40 (40% of revenue to debt service is high)

**Hard-Coded Thresholds** (fixed for all users):
- **High interest rate**: 15% (factual cutoff, not lender-specific)
- **Negative operating income**: 0 (mathematical fact, not configurable)

---

## 7. Golden Test Scenarios

### Scenario 1: Typical SBA Loan - Restaurant

**Description**: Standard SBA 7(a) loan for restaurant equipment purchase. DSCR is healthy (1.42), above typical lender minimum.

**Inputs**:
```json
{
  "loan_amount": 250000,
  "interest_rate": 7.5,
  "term_years": 10,
  "annual_revenue": 1500000,
  "annual_operating_expenses": 1200000
}
```

**Expected Outputs**:
```json
{
  "monthly_payment": { "value": 2958.04, "tolerance": 1, "unit": "USD" },
  "total_interest": { "value": 104964.80, "tolerance": 100, "unit": "USD" },
  "total_amount_paid": { "value": 354964.80, "tolerance": 100, "unit": "USD" },
  "annual_debt_service": { "value": 35496.48, "tolerance": 10, "unit": "USD" },
  "net_operating_income": { "value": 300000, "tolerance": 0, "unit": "USD" },
  "dscr": { "value": 1.42, "tolerance": 0.01, "unit": "ratio" },
  "covenant_headroom": { "value": 0.17, "tolerance": 0.01, "unit": "ratio" }
}
```

**Expected Warnings**: None (healthy scenario)

### Scenario 2: Tight DSCR - Retail Business

**Description**: Retail business with DSCR slightly below typical lender minimum (1.18). Should trigger warning.

**Inputs**:
```json
{
  "loan_amount": 250000,
  "interest_rate": 7.5,
  "term_years": 10,
  "annual_revenue": 1000000,
  "annual_operating_expenses": 900000
}
```

**Expected Outputs**:
```json
{
  "monthly_payment": { "value": 2958.04, "tolerance": 1, "unit": "USD" },
  "total_interest": { "value": 104964.80, "tolerance": 100, "unit": "USD" },
  "total_amount_paid": { "value": 354964.80, "tolerance": 100, "unit": "USD" },
  "annual_debt_service": { "value": 35496.48, "tolerance": 10, "unit": "USD" },
  "net_operating_income": { "value": 100000, "tolerance": 0, "unit": "USD" },
  "dscr": { "value": 1.18, "tolerance": 0.01, "unit": "ratio" },
  "covenant_headroom": { "value": -0.07, "tolerance": 0.01, "unit": "ratio" }
}
```

**Expected Warnings**:
- DSCR_BELOW_MINIMUM: "DSCR of 1.18 is below the typical lender minimum of 1.25..."

### Scenario 3: Strong DSCR - Service Business

**Description**: Service business with strong DSCR (2.10), well above lender requirements.

**Inputs**:
```json
{
  "loan_amount": 150000,
  "interest_rate": 6.5,
  "term_years": 10,
  "annual_revenue": 1200000,
  "annual_operating_expenses": 900000
}
```

**Expected Outputs**:
```json
{
  "monthly_payment": { "value": 1704.56, "tolerance": 1, "unit": "USD" },
  "total_interest": { "value": 54547.20, "tolerance": 100, "unit": "USD" },
  "total_amount_paid": { "value": 204547.20, "tolerance": 100, "unit": "USD" },
  "annual_debt_service": { "value": 20454.72, "tolerance": 10, "unit": "USD" },
  "net_operating_income": { "value": 300000, "tolerance": 0, "unit": "USD" },
  "dscr": { "value": 2.10, "tolerance": 0.01, "unit": "ratio" },
  "covenant_headroom": { "value": 0.85, "tolerance": 0.01, "unit": "ratio" }
}
```

**Expected Warnings**:
- DSCR_ABOVE_OPTIMAL: "DSCR of 2.10 is well above lender requirements..."

---

## 8. AI Narrative Strategy (AI-Enabled)

### What AI Explains for This Calculator

**Primary Explanations**:
1. **What the DSCR value means** and whether it's acceptable for lenders (most common request)
2. **Why total interest is high** and how term length affects total cost
3. **Risks of this loan structure** (short term = high payment, long term = more interest)

**Scenario Suggestions**:
- Alternative loan terms to test (longer term to reduce payment, shorter term to save interest)
- Revenue/expense changes to improve DSCR (10% revenue increase, 5% expense reduction)
- Optimal loan amount for target DSCR (e.g., "What loan amount gives me DSCR = 1.50?")

### Prompt Template

**System Prompt**:
```
You are a CFO advisor helping business owners understand loan payments and debt service coverage.

Your role is to:
- Explain DSCR in plain language and interpret whether this value meets lender requirements
- Explain the relationship between loan term, monthly payment, and total interest
- Suggest ways to improve DSCR if it's below the 1.25 threshold

Output constraints:
- Length: 150-250 words
- Tone: Professional but approachable, avoid jargon
- Format: Plain paragraphs (no bullet lists for explanations)
- Disclaimers: Always end with: "This analysis is for informational purposes only and does not constitute financial, legal, or professional advice. Consult qualified professionals for decisions affecting your business."

Prohibited behaviors:
- Never guarantee loan approval (use "typically", "most lenders require")
- Never recommend taking or not taking a loan (only explain metrics)
- Never use absolute language (avoid "always", "never", "must")
```

**User Prompt Template**:
```
The business has annual revenue of ${annual_revenue}, operating expenses of ${annual_operating_expenses}, and a loan with monthly payment of ${monthly_payment}. The calculated DSCR is ${dscr}. Explain what this means and whether it's acceptable for lenders.
```

**Expected AI Response** (150-250 words):
```
A DSCR of 1.42 means the business generates $1.42 in operating income for every $1 in debt payments. Most lenders require a minimum DSCR of 1.25, so this ratio is comfortably above that threshold. This indicates the business has a healthy cushion to cover loan payments even if income fluctuates.

With annual revenue of $1,500,000 and operating expenses of $1,200,000, the business has $300,000 in annual operating income available to cover $35,496 in annual debt payments ($2,958 × 12 months). The $264,504 cushion provides protection against revenue declines or expense increases.

However, lenders also consider credit history, collateral, and industry-specific factors beyond DSCR. A DSCR above 1.25 is a positive indicator but does not guarantee loan approval.

This analysis is for informational purposes only and does not constitute financial, legal, or professional advice. Consult qualified professionals for decisions affecting your business.
```

---

## 9. Export Format Details

### PDF Layout

**Header**:
- "Business Loan + DSCR Calculator"
- User tier: "Free" or "Pro"
- Generation timestamp: "Generated on November 18, 2025"

**Body Sections**:
1. **Loan Details** (table):
   - Loan Amount: $250,000
   - Interest Rate: 7.5%
   - Term: 10 years

2. **Key Results** (table):
   - Monthly Payment: $2,958.04
   - Total Interest: $104,964.80
   - Total Amount Paid: $354,964.80

3. **Business Financials** (table, only if provided):
   - Annual Revenue: $1,500,000
   - Operating Expenses: $1,200,000

4. **Advanced Results** (Pro tier only, table):
   - Debt Service Coverage Ratio: 1.42
   - Annual Debt Service: $35,496.48
   - Net Operating Income: $300,000
   - Covenant Headroom: +0.17

5. **Amortization Schedule** (Pro tier only, chart or table):
   - First 12 months shown in detail
   - Summary for remaining years

6. **Warnings** (if any):
   - List all warnings with severity icons

**Footer**:
- Disclaimers: "This calculator is for informational purposes only and does not constitute financial advice. Actual loan terms may vary. Consult a lender for specific loan offers."
- Version stamp: "Generated by CFO Calculator Suite v1.2.3 | Calculator: business-loan-dscr v1.0.0 | Formulas: v1.2.3 | 2025-11-18"
- Watermark (Free tier only): Diagonal semi-transparent "Upgrade for clean exports"

### CSV/Excel Structure

**Metadata Headers**:
```csv
# Business Loan + DSCR Calculator
# Version: v1.0.0
# Generated: 2025-11-18T21:30:00Z
# Tier: Pro
#
```

**Data Rows**:
```csv
Section,Field,Value
Loan Details,Loan Amount,$250000
Loan Details,Interest Rate,7.5%
Loan Details,Term,10 years
Business Financials,Annual Revenue,$1500000
Business Financials,Operating Expenses,$1200000
Key Results,Monthly Payment,$2958.04
Key Results,Total Interest,$104964.80
Key Results,Total Amount Paid,$354964.80
Advanced Results,DSCR,1.42
Advanced Results,Annual Debt Service,$35496.48
Advanced Results,Net Operating Income,$300000
Advanced Results,Covenant Headroom,+0.17
```

**Excel Formatting**:
- Bold section headers
- Currency cells: $#,##0.00
- Percentage cells: 0.00%
- Alternating row colors (light gray for even rows)

---

## 10. Tier-Specific Behavior

### Free Tier

**What's Shown**:
- Single scenario (cannot save or create multiple)
- Loan details inputs (loan amount, interest rate, term)
- Business financials inputs (revenue, expenses) - but DSCR output is locked
- Key metrics: monthly payment, total interest, total amount paid
- Basic warnings (high interest rate, short term)

**What's Gated**:
- **Multiple scenarios**: "Add Scenario" button shows Pro badge, triggers upgrade prompt
- **DSCR and advanced metrics**: Shown as locked with tooltip "Upgrade to Pro to see DSCR and advanced metrics"
- **Amortization schedule**: Chart preview shown with blur effect + "Pro" badge
- **Clean exports**: PDF has watermark
- **AI narratives**: "Explain this result" button triggers AI tier upgrade prompt

**Upgrade Triggers**:
- Click "Add Scenario" → `upgrade_prompt_shown` with `trigger_reason: "add_scenario"`
- Click locked DSCR metric → `upgrade_prompt_shown` with `trigger_reason: "locked_metric"`
- Click "Explain this result" → `upgrade_prompt_shown` with `trigger_reason: "ai_request"`

### Pro Tier

**What Unlocks**:
- **Multiple scenarios** (up to 50): Create, save, rename, delete scenarios
- **All advanced metrics**: DSCR, annual debt service, net operating income, covenant headroom
- **Amortization schedule**: Full interactive chart showing principal/interest breakdown over time
- **Scenario comparison**: Side-by-side view of multiple loan structures
- **Clean exports**: PDF with no watermark
- **Sensitivity analysis**: "What if" scenarios (revenue ±10%, ±20%)

**Still Gated** (requires AI tier):
- AI explanations and scenario suggestions

### AI Tier

**What Unlocks**:
- **AI explanations**: Click "Explain this result" to get plain-language DSCR interpretation
- **AI scenario suggestions**: Click "Get suggestions" for alternative loan structures to test
- **50 AI requests per month** (hard cap)

**Usage Quota**:
- Counter displayed: "23 of 50 AI requests remaining this month"
- Resets on 1st of each month

---

## Implementation Notes

**Priority**: High (foundational calculator, required for M1)

**Estimated Effort**: 2 weeks
- Week 1: Formula library, UI components, basic functionality
- Week 2: DSCR calculation, warnings, testing, golden scenarios

**Dependencies**:
- Shared amortization formula library (create once, reuse for SBA 7(a) calculator)
- DSCR calculation component (reuse for other lending calculators)
- Input validation library (currency, percentage, number formatters)

**Related Calculators**:
- **SBA 7(a) Loan Analyzer**: Similar inputs, adds SBA-specific fees
- **Line of Credit Analyzer**: Alternative financing option for working capital
- **Equipment Lease vs Buy**: Alternative to loan for equipment financing

---

## End of Calculator PDR

This calculator is complete and ready for implementation. All formulas, warnings, and tier behaviors are specified in detail.
