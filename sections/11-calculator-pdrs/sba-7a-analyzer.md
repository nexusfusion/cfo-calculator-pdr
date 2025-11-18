# SBA 7(a) Loan Analyzer

## 1. Calculator Overview

**Calculator Name**: SBA 7(a) Loan Analyzer

**Calculator Slug**: `sba-7a-analyzer`

**Primary Category**: Financing & Lending Intelligence

**Secondary Category**: None

**Business Role**:
- **SEO/traffic magnet**: High search volume for "SBA 7(a) calculator" and "SBA loan calculator"
- **Affiliate revenue driver**: High-intent users seeking SBA loans (partner with SBA lenders)
- **Pro upgrade driver**: SBA fee calculations and effective rate analysis drive conversions

**Primary Success Metric**: Affiliate referral rate (target: 8-12% of users click partner lender links)

**Version**: v1.0.0

**Formula Library Version**: v1.2.3

---

## 2. User Scenarios and Use Cases

### Who Uses This Calculator

**Primary Personas**:
- **Small business owners** applying for SBA 7(a) loans ($50k-$5M) for acquisition, expansion, or working capital
- **Business buyers** evaluating SBA financing for business acquisition (most common use case)
- **SBA-preferred lenders** pre-qualifying applications and estimating total borrower costs

**Secondary Personas**:
- **Business brokers** helping clients structure acquisition financing
- **SBDC advisors** assisting small businesses with SBA loan applications
- **Commercial real estate investors** using SBA 504 loans (alternative to 7(a))

### When They Use It (Decision Contexts)

**Timing**:
- **Before SBA application**: Planning phase, determining if SBA loan is affordable
- **After receiving term sheet**: Comparison phase, understanding true all-in costs including SBA guarantee fees
- **Comparing lender offers**: Multiple SBA lenders may offer different rates and fees
- **Acquisition due diligence**: Business buyers modeling acquisition financing scenarios

**Frequency**:
- **One-time use**: Most users apply for SBA loan once for specific purchase or expansion
- **Recurring use** (Pro tier): Business brokers and advisors run multiple scenarios per week

### What Questions It Answers

This calculator helps users answer:

1. **What are the total SBA guarantee fees?** (Most users don't know SBA charges 2-3.75% upfront fee)
2. **What's my true all-in cost including fees?** (Effective APR including guarantee fee and closing costs)
3. **How does SBA 7(a) compare to conventional loan?** (SBA has higher fees but lower down payment)
4. **Can I afford this monthly payment given my revenue?** (DSCR calculation for loan approval)
5. **Should I finance the guarantee fee or pay upfront?** (Compare cash vs financed fee scenarios)

---

## 3. Inputs

### Input Field Definitions

| Field Name | Type | Units | Required | Validation | Default | Placeholder | Tooltip |
|------------|------|-------|----------|------------|---------|-------------|---------|
| loan_amount | currency | dollars | Yes | min: 1, max: 5000000 | null | "250000" | "Total SBA 7(a) loan amount. SBA 7(a) loans range from $50,000 to $5 million. Most lenders prefer $150k minimum." |
| interest_rate | percentage | percent | Yes | min: 0, max: 30 | null | "8.5" | "Annual interest rate (APR). SBA sets maximum rates based on Prime Rate + spread. As of 2025, typical rates are 7.5-11.5%." |
| term_years | number | years | Yes | min: 1, max: 25 | 10 | "10" | "Loan term in years. SBA allows up to 10 years for working capital/equipment, up to 25 years for real estate. Most common: 10 years." |
| sba_guarantee_fee_percent | percentage | percent | No | min: 0, max: 3.75 | null | "3.75" | "SBA upfront guarantee fee. 2% for loans ≤$150k, 3% for $150k-$700k, 3.5% for $700k-$1M, 3.75% for $1M+. Leave blank for automatic calculation." |
| finance_guarantee_fee | boolean | yes/no | No | true/false | true | "Yes" | "If Yes, SBA guarantee fee is added to loan amount (most common). If No, fee is paid upfront in cash." |
| other_closing_costs | currency | dollars | No | min: 0, max: 100000 | null | "5000" | "Other closing costs (appraisal, legal, title, etc.). Typical range: $3,000-$10,000. Leave blank if unknown." |
| annual_revenue | currency | dollars | No | min: 0, max: 100000000 | null | "1500000" | "Total annual revenue (gross income before expenses). Required for DSCR calculation. Most lenders require DSCR ≥ 1.25." |
| annual_operating_expenses | currency | dollars | No | min: 0, max: 100000000 | null | "1200000" | "Total annual operating expenses (excluding debt service). Required for DSCR calculation. Do not include the loan payment you're calculating." |

### Input Groups

**Group 1: Loan Details** (always visible)
- loan_amount
- interest_rate
- term_years

**Group 2: SBA-Specific Fees** (always visible, distinguishes this from generic loan calculator)
- sba_guarantee_fee_percent
- finance_guarantee_fee
- other_closing_costs

**Group 3: Business Financials** (collapsed by default, expands for DSCR)
- annual_revenue
- annual_operating_expenses

### Optional Inputs

**Advanced Options** (collapsed, rarely used):
- **conventional_loan_rate** (percentage): Interest rate for conventional (non-SBA) loan comparison. Default: interest_rate + 1.5%
- **conventional_down_payment_percent** (percentage): Down payment required for conventional loan. Default: 20%
- **prepayment_penalty** (boolean): Does loan have prepayment penalty? Default: No (most SBA loans allow prepayment without penalty)

---

## 4. Outputs

### Key Metrics (Free Tier - Always Visible)

| Output Name | Description | Format | Units |
|-------------|-------------|--------|-------|
| monthly_payment | Principal and interest payment per month | "$#,##0.00" | dollars |
| total_sba_fees | SBA guarantee fee + other closing costs | "$#,##0.00" | dollars |
| total_interest | Total interest paid over loan term | "$#,##0.00" | dollars |
| total_amount_paid | Principal + interest + SBA fees | "$#,##0.00" | dollars |

### Advanced Metrics (Pro Tier - Gated)

| Output Name | Description | Format | Units | Pro Only |
|-------------|-------------|--------|-------|----------|
| effective_apr | True annual cost including SBA guarantee fee | "#.00%" | percent | Yes |
| dscr | Debt Service Coverage Ratio (if financials provided) | "#.00" | ratio | Yes |
| financed_vs_cash_fee_comparison | Cost difference: financing fee vs paying cash upfront | "$#,##0.00" | dollars | Yes |
| sba_vs_conventional_comparison | Total cost comparison (SBA vs conventional loan) | "Table" | mixed | Yes |
| guaranteed_portion | Amount of loan guaranteed by SBA (typically 75-85%) | "$#,##0.00" | dollars | Yes |
| annual_debt_service | Total annual loan payments (monthly × 12) | "$#,##0.00" | dollars | Yes |

### Charts and Visualizations

**Chart 1: Cost Breakdown** (Pro tier only)
- Type: Stacked bar chart
- X-axis: Cost components (Principal, Interest, SBA Fees, Other Fees)
- Y-axis: Dollars
- Purpose: Visualize how much of total cost is SBA-specific fees
- Tier: Pro

**Chart 2: SBA vs Conventional Comparison** (Pro tier only)
- Type: Side-by-side bar chart
- X-axis: Loan type (SBA 7(a), Conventional)
- Y-axis: Total cost, down payment required, monthly payment
- Purpose: Compare total economics of SBA vs conventional financing
- Tier: Pro

**Chart 3: Financed vs Cash Fee Comparison** (Pro tier only)
- Type: Line chart
- X-axis: Months
- Y-axis: Cumulative cost
- Purpose: Show cost difference over time between financing fee vs paying cash upfront
- Tier: Pro

### Output Formatting Rules

- **Currency**: $#,##0.00 (always two decimals, commas for thousands)
- **Percentages**: #.00% (two decimals)
- **Ratios**: #.00 (two decimals, no percent sign for DSCR)
- **SBA Guarantee Fee**: Always show both percent and dollar amount (e.g., "3.75% ($9,375)")

---

## 5. Formulas and Calculation Logic

### Formula 1: SBA Guarantee Fee Calculation

**Purpose**: Calculate upfront SBA guarantee fee based on loan amount tiers

**Equation**:
```
if loan_amount <= 150000:
    guarantee_fee_percent = 2.0
    guarantee_fee = loan_amount × 0.02

elif loan_amount <= 700000:
    guarantee_fee_percent = 3.0
    guarantee_fee = loan_amount × 0.03

elif loan_amount <= 1000000:
    guarantee_fee_percent = 3.5
    guarantee_fee = loan_amount × 0.035

else:  # loan_amount > 1000000
    guarantee_fee_percent = 3.75
    # For loans over $1M, fee is only charged on guaranteed portion (75% for loans >$1M)
    guaranteed_portion = loan_amount × 0.75
    guarantee_fee = guaranteed_portion × 0.0375
```

**Variables**:
- loan_amount: Base loan amount in dollars
- guarantee_fee_percent: SBA fee percentage (2%, 3%, 3.5%, or 3.75%)
- guarantee_fee: Dollar amount of SBA guarantee fee
- guaranteed_portion: For loans >$1M, SBA only guarantees 75%

**Source/Reference**: SBA Standard Operating Procedure (SOP) 50 10 7, updated annually. Current as of 2025.

### Formula 2: Effective Loan Amount (if fee is financed)

**Purpose**: Calculate total amount borrowed if SBA guarantee fee is financed

**Equation**:
```
if finance_guarantee_fee == true:
    effective_loan_amount = loan_amount + guarantee_fee
else:
    effective_loan_amount = loan_amount
```

**Note**: Most borrowers finance the guarantee fee (85-90% of SBA loans), so it's added to principal and interest is charged on total.

### Formula 3: Monthly Payment

**Purpose**: Calculate fixed monthly payment for fully amortizing loan

**Equation**:
```
P = effective_loan_amount (includes guarantee fee if financed)
r = monthly interest rate (annual rate / 12 / 100)
n = number of monthly payments (term_years × 12)

monthly_payment = P × [r × (1 + r)^n] / [(1 + r)^n - 1]

Special case (zero interest):
if interest_rate == 0:
    monthly_payment = P / n
```

**Source/Reference**: Standard amortization formula

### Formula 4: Effective APR (including guarantee fee)

**Purpose**: Calculate true annual cost including SBA guarantee fee amortized over loan term

**Equation**:
```
# Total cost over life of loan
total_interest = (monthly_payment × n) - loan_amount
total_fees = guarantee_fee + other_closing_costs
total_cost = total_interest + total_fees

# Effective APR calculation (IRR approach)
# Solve for APR where:
# PV (loan_amount) = PV of monthly payments + PV of fees
# This requires iterative solving (Newton-Raphson method)

# Simplified approximation:
effective_apr = (total_cost / loan_amount) / term_years × 100

# More accurate: Use IRR calculation
# effective_apr = IRR([-loan_amount, monthly_payment, monthly_payment, ...])
```

**Source/Reference**: Truth in Lending Act (TILA) APR calculation methodology

### Formula 5: DSCR (Debt Service Coverage Ratio)

**Purpose**: Calculate business's ability to cover loan payments with operating income

**Equation**:
```
annual_debt_service = monthly_payment × 12
net_operating_income = annual_revenue - annual_operating_expenses
dscr = net_operating_income / annual_debt_service
```

**Variables**:
- annual_debt_service: Total annual loan payments
- net_operating_income: EBITDA or operating income before debt service
- dscr: Ratio (SBA lenders typically require ≥ 1.25)

**Source/Reference**: SBA Lender Guide, standard debt service coverage ratio formula

### Formula 6: SBA vs Conventional Loan Comparison

**Purpose**: Compare total economics of SBA loan vs conventional financing

**Equation**:
```
# SBA Loan
sba_down_payment = 0  # SBA allows up to 90% LTV (10% down)
sba_financed_amount = loan_amount
sba_total_cost = (monthly_payment × n) + total_fees

# Conventional Loan (typically requires 20-30% down)
conventional_down_payment = loan_amount × conventional_down_payment_percent
conventional_financed_amount = loan_amount × (1 - conventional_down_payment_percent)
conventional_monthly_payment = [calculated using conventional_loan_rate]
conventional_total_cost = (conventional_monthly_payment × n)

# Comparison
cash_savings = conventional_down_payment - sba_down_payment
total_cost_difference = sba_total_cost - conventional_total_cost
```

### Step-by-Step Calculation Flow

1. **Validate inputs**: Check required fields, validate ranges
2. **Calculate SBA guarantee fee**: Use tiered percentage based on loan amount
3. **Determine effective loan amount**: Add guarantee fee if financed
4. **Convert annual rate to monthly**: `r = interest_rate / 12 / 100`
5. **Calculate number of payments**: `n = term_years × 12`
6. **Calculate monthly payment**: Use amortization formula
7. **Calculate total interest**: `(monthly_payment × n) - effective_loan_amount`
8. **Calculate total SBA fees**: `guarantee_fee + other_closing_costs`
9. **Calculate effective APR**: Include all fees in IRR calculation
10. **If financials provided, calculate DSCR**: `net_operating_income / annual_debt_service`
11. **If conventional comparison requested**: Calculate conventional loan costs
12. **Check warning conditions**: DSCR below minimum, high effective APR, etc.
13. **Return results with version stamp**

### Edge Cases and Handling

| Edge Case | Condition | Handling | Warning/Error |
|-----------|-----------|----------|---------------|
| Loan exactly $150k | loan_amount = 150000 | Use 2% fee (lower tier) | Info: "Loan at tier boundary. Fee is 2% ($3,000)." |
| Very large loan >$5M | loan_amount > 5000000 | Calculate as normal but warn | Warning: "SBA 7(a) limit is $5M. Loan may require SBA 504 or conventional financing." |
| Zero interest rate | interest_rate = 0 | monthly_payment = principal / n | Info: "Zero interest rate. Payment is principal only." |
| Fee paid in cash | finance_guarantee_fee = false | effective_loan_amount = loan_amount | Info: "Paying fee upfront saves interest costs." |
| Very high effective APR | effective_apr > 15% | Calculate as normal | Warning: "Effective APR is high. Consider shopping for better rates." |

### Formula Version

**Current Version**: v1.2.3

**Version History**:
- v1.0.0: Initial implementation (monthly payment, SBA fee calculation)
- v1.1.0: Added effective APR calculation including fees
- v1.2.0: Added SBA vs conventional comparison
- v1.2.3: Fixed tiered fee calculation for loans >$1M (fee on guaranteed portion only)

---

## 6. Warnings and Alerts

### Warning Definitions

| Warning Code | Condition | Message | Severity | Action |
|--------------|-----------|---------|----------|--------|
| DSCR_BELOW_SBA_MINIMUM | dscr < 1.25 | "DSCR of {dscr} is below the SBA lender minimum of 1.25. This loan is unlikely to be approved without additional collateral, guarantees, or business improvements." | danger | Increase revenue, reduce expenses, extend term, or reduce loan amount |
| HIGH_EFFECTIVE_APR | effective_apr > 12% | "Effective APR of {effective_apr}% is high for an SBA loan. Typical SBA rates are 7-11%. Shop for better rates or improve credit score." | warning | Compare multiple SBA lenders, improve creditworthiness |
| LARGE_GUARANTEE_FEE | guarantee_fee > 50000 | "SBA guarantee fee is ${guarantee_fee:,.0f}. Consider paying upfront (cash) to avoid paying interest on this fee over {term_years} years." | info | Compare financed vs cash fee scenarios (Pro feature) |
| LOAN_NEAR_SBA_LIMIT | loan_amount > 4500000 | "Loan amount is near the SBA 7(a) limit of $5M. Consider SBA 504 loan for real estate or equipment." | info | Evaluate SBA 504 as alternative (lower rates, longer terms for real estate) |
| SHORT_TERM_HIGH_PAYMENT | term_years < 7 && monthly_payment / (annual_revenue / 12) > 0.15 | "Short loan term results in high monthly payment ({monthly_payment:,.0f}). This is {percent}% of monthly revenue. Consider extending term to 10 years." | warning | Extend term to reduce monthly payment burden |
| CONVENTIONAL_MAY_BE_CHEAPER | sba_total_cost > conventional_total_cost + 10000 | "SBA loan total cost is ${cost_difference:,.0f} higher than conventional financing. SBA benefit is lower down payment, but if you have cash for 20% down, conventional may be cheaper." | info | Compare total economics (Pro feature shows detailed comparison) |
| NO_FINANCIALS_PROVIDED | annual_revenue == null or annual_operating_expenses == null | "Business financials not provided. DSCR calculation unavailable. SBA lenders require DSCR ≥ 1.25 for approval." | info | Add revenue and expenses to calculate DSCR |

### Warning Thresholds

**Configurable Thresholds** (can be adjusted per tenant for B2B):
- **DSCR minimum**: 1.25 (SBA standard, some lenders may accept 1.15 with strong collateral)
- **High effective APR**: 12% (above typical SBA range)

**Hard-Coded Thresholds** (SBA program rules, not configurable):
- **SBA 7(a) loan limit**: $5,000,000 (federal regulation)
- **SBA guarantee fee percentages**: 2%, 3%, 3.5%, 3.75% (SBA policy)
- **Maximum SBA interest rate**: Prime + 2.75% (SBA regulation, varies by loan size)

---

## 7. Golden Test Scenarios

### Scenario 1: Standard SBA 7(a) - Restaurant Equipment

**Description**: Typical SBA 7(a) loan for restaurant equipment purchase. Loan is $250k with 3% guarantee fee. DSCR is healthy (1.48).

**Inputs**:
```json
{
  "loan_amount": 250000,
  "interest_rate": 8.5,
  "term_years": 10,
  "sba_guarantee_fee_percent": 3.0,
  "finance_guarantee_fee": true,
  "other_closing_costs": 5000,
  "annual_revenue": 1500000,
  "annual_operating_expenses": 1200000
}
```

**Expected Outputs**:
```json
{
  "monthly_payment": {
    "value": 3138.74,
    "tolerance": 1,
    "unit": "USD",
    "note": "Based on $257,500 financed ($250k + $7,500 fee)"
  },
  "total_sba_fees": {
    "value": 12500,
    "tolerance": 0,
    "unit": "USD",
    "note": "$7,500 guarantee fee (3%) + $5,000 closing costs"
  },
  "total_interest": {
    "value": 119149.09,
    "tolerance": 100,
    "unit": "USD"
  },
  "total_amount_paid": {
    "value": 386649.09,
    "tolerance": 100,
    "unit": "USD",
    "note": "Principal ($250k) + interest ($119k) + fees ($12.5k)"
  },
  "effective_apr": {
    "value": 9.28,
    "tolerance": 0.1,
    "unit": "percent",
    "note": "Higher than nominal 8.5% due to financed guarantee fee"
  },
  "dscr": {
    "value": 1.48,
    "tolerance": 0.01,
    "unit": "ratio"
  },
  "annual_debt_service": {
    "value": 37664.88,
    "tolerance": 10,
    "unit": "USD"
  },
  "guaranteed_portion": {
    "value": 212500,
    "tolerance": 0,
    "unit": "USD",
    "note": "85% guaranteed for loans $150k-$700k"
  }
}
```

**Expected Warnings**: None (healthy scenario)

### Scenario 2: Large SBA 7(a) - Business Acquisition

**Description**: Large SBA 7(a) loan ($1M) for business acquisition. Guarantee fee is 3.75% but only charged on 75% guaranteed portion. DSCR is tight (1.22), below minimum.

**Inputs**:
```json
{
  "loan_amount": 1000000,
  "interest_rate": 9.0,
  "term_years": 10,
  "sba_guarantee_fee_percent": 3.75,
  "finance_guarantee_fee": true,
  "other_closing_costs": 8000,
  "annual_revenue": 2500000,
  "annual_operating_expenses": 2200000
}
```

**Expected Outputs**:
```json
{
  "monthly_payment": {
    "value": 12971.80,
    "tolerance": 2,
    "unit": "USD",
    "note": "Based on $1,028,125 financed ($1M + $28,125 fee)"
  },
  "total_sba_fees": {
    "value": 36125,
    "tolerance": 0,
    "unit": "USD",
    "note": "$28,125 guarantee fee (3.75% on 75% guaranteed portion) + $8,000 closing"
  },
  "total_interest": {
    "value": 528490.67,
    "tolerance": 200,
    "unit": "USD"
  },
  "total_amount_paid": {
    "value": 1564615.67,
    "tolerance": 200,
    "unit": "USD"
  },
  "effective_apr": {
    "value": 9.64,
    "tolerance": 0.1,
    "unit": "percent"
  },
  "dscr": {
    "value": 1.22,
    "tolerance": 0.01,
    "unit": "ratio"
  },
  "annual_debt_service": {
    "value": 155661.60,
    "tolerance": 20,
    "unit": "USD"
  },
  "guaranteed_portion": {
    "value": 750000,
    "tolerance": 0,
    "unit": "USD",
    "note": "75% guaranteed for loans >$700k"
  }
}
```

**Expected Warnings**:
- DSCR_BELOW_SBA_MINIMUM: "DSCR of 1.22 is below the SBA lender minimum of 1.25..."
- LARGE_GUARANTEE_FEE: "SBA guarantee fee is $28,125. Consider paying upfront (cash) to avoid paying interest..."

### Scenario 3: Small SBA 7(a) - Working Capital

**Description**: Small SBA 7(a) loan ($100k) for working capital. Lower guarantee fee (2%). Strong DSCR (2.15).

**Inputs**:
```json
{
  "loan_amount": 100000,
  "interest_rate": 10.5,
  "term_years": 7,
  "sba_guarantee_fee_percent": 2.0,
  "finance_guarantee_fee": true,
  "other_closing_costs": 3000,
  "annual_revenue": 800000,
  "annual_operating_expenses": 650000
}
```

**Expected Outputs**:
```json
{
  "monthly_payment": {
    "value": 1496.19,
    "tolerance": 1,
    "unit": "USD",
    "note": "Based on $102,000 financed ($100k + $2k fee)"
  },
  "total_sba_fees": {
    "value": 5000,
    "tolerance": 0,
    "unit": "USD",
    "note": "$2,000 guarantee fee (2%) + $3,000 closing costs"
  },
  "total_interest": {
    "value": 23680.19,
    "tolerance": 50,
    "unit": "USD"
  },
  "total_amount_paid": {
    "value": 128680.19,
    "tolerance": 50,
    "unit": "USD"
  },
  "effective_apr": {
    "value": 11.38,
    "tolerance": 0.1,
    "unit": "percent"
  },
  "dscr": {
    "value": 2.15,
    "tolerance": 0.01,
    "unit": "ratio"
  },
  "annual_debt_service": {
    "value": 17954.28,
    "tolerance": 10,
    "unit": "USD"
  },
  "guaranteed_portion": {
    "value": 85000,
    "tolerance": 0,
    "unit": "USD",
    "note": "85% guaranteed for loans ≤$150k"
  }
}
```

**Expected Warnings**:
- DSCR_ABOVE_OPTIMAL: "DSCR of 2.15 is well above lender requirements. You may be able to borrow more if needed."

---

## 8. AI Narrative Strategy (AI-Enabled)

### What AI Explains for This Calculator

**Primary Explanations**:
1. **What the SBA guarantee fee is and why it's charged** (most users don't understand this upfront cost)
2. **Whether financing the fee vs paying cash makes sense** (interest cost of financing fee over 10 years)
3. **How SBA 7(a) compares to conventional financing** (lower down payment vs higher total cost)
4. **What the DSCR means for loan approval** (SBA lenders require ≥ 1.25)

**Scenario Suggestions**:
- Alternative loan structures (longer term to reduce DSCR burden, pay fee in cash to reduce effective APR)
- Conventional vs SBA comparison (if user has 20% down payment available)
- Optimal loan amount for target DSCR (e.g., "What loan amount gives me DSCR = 1.50?")

### Prompt Template

**System Prompt**:
```
You are an SBA lending advisor helping small business owners understand SBA 7(a) loans and their total costs.

Your role is to:
- Explain SBA guarantee fees in plain language and why they exist
- Interpret whether the calculated DSCR meets SBA lender requirements
- Compare SBA financing to conventional loans (pros: lower down payment; cons: higher fees)
- Suggest whether financing the guarantee fee vs paying cash makes sense

Output constraints:
- Length: 150-250 words
- Tone: Professional but approachable, avoid jargon
- Format: Plain paragraphs (no bullet lists)
- Disclaimers: Always end with standard disclaimer

Prohibited behaviors:
- Never guarantee SBA loan approval (use "typically", "most SBA lenders require")
- Never recommend taking or not taking a loan (only explain metrics)
- Never use absolute language (avoid "always", "never", "must")
```

**User Prompt Template**:
```
The business is seeking an SBA 7(a) loan of ${loan_amount} at {interest_rate}% for {term_years} years. The SBA guarantee fee is {guarantee_fee_percent}% (${guarantee_fee}), which is {financed or paid in cash}. The calculated monthly payment is ${monthly_payment}, effective APR is {effective_apr}%, and DSCR is {dscr}. Explain what this means and whether the SBA loan is a good option.
```

**Expected AI Response** (150-250 words):
```
An SBA 7(a) loan of $250,000 carries a 3% guarantee fee ($7,500), which is the price the SBA charges for guaranteeing 85% of the loan to the lender. This guarantee allows lenders to offer more favorable terms (lower down payment, longer terms) than conventional loans. In this case, financing the fee adds it to the principal, increasing the effective APR from 8.5% to 9.28%.

The monthly payment of $3,139 results in annual debt service of $37,665. With business revenue of $1,500,000 and operating expenses of $1,200,000, the net operating income is $300,000, giving a DSCR of 1.48. This is comfortably above the SBA lender minimum of 1.25, indicating strong ability to cover loan payments.

SBA 7(a) loans typically require only 10-20% down payment compared to 20-30% for conventional loans. However, the guarantee fee and potentially higher interest rates mean the total cost may be higher. The trade-off is preserving cash for working capital while accepting higher financing costs. For this scenario, the DSCR suggests the business can comfortably afford the payment.

This analysis is for informational purposes only and does not constitute financial, legal, or professional advice. Consult qualified professionals for decisions affecting your business.
```

---

## 9. Export Format Details

### PDF Layout

**Header**:
- "SBA 7(a) Loan Analyzer"
- User tier: "Free" or "Pro"
- Generation timestamp: "Generated on November 18, 2025"

**Body Sections**:
1. **Loan Details** (table):
   - Loan Amount: $250,000
   - Interest Rate: 8.5%
   - Term: 10 years
   - SBA Guarantee Fee: 3.0% ($7,500)
   - Guarantee Fee: Financed / Paid in Cash
   - Other Closing Costs: $5,000

2. **Key Results** (table):
   - Monthly Payment: $3,138.74
   - Total Interest: $119,149.09
   - Total SBA Fees: $12,500.00
   - Total Amount Paid: $386,649.09

3. **Business Financials** (table, if provided):
   - Annual Revenue: $1,500,000
   - Operating Expenses: $1,200,000

4. **Advanced Results** (Pro tier only, table):
   - Effective APR: 9.28%
   - Debt Service Coverage Ratio: 1.48
   - Annual Debt Service: $37,664.88
   - Net Operating Income: $300,000
   - SBA Guaranteed Portion: $212,500 (85%)

5. **SBA vs Conventional Comparison** (Pro tier only, table):
   - Comparison of total costs, down payment requirements, monthly payments

6. **Cost Breakdown Chart** (Pro tier only):
   - Stacked bar showing principal, interest, SBA fees, other fees

7. **Warnings** (if any):
   - List all warnings with severity icons

**Footer**:
- Disclaimers: "This calculator is for informational purposes only and does not constitute financial advice. SBA loan approval is subject to lender underwriting. Consult an SBA-preferred lender for specific loan offers."
- Version stamp: "Generated by CFO Calculator Suite v1.2.3 | Calculator: sba-7a-analyzer v1.0.0 | Formulas: v1.2.3 | 2025-11-18"
- Watermark (Free tier only): Diagonal semi-transparent "Upgrade for clean exports"

### CSV/Excel Structure

**Metadata Headers**:
```csv
# SBA 7(a) Loan Analyzer
# Version: v1.0.0
# Generated: 2025-11-18T21:30:00Z
# Tier: Pro
#
```

**Data Rows**:
```csv
Section,Field,Value
Loan Details,Loan Amount,$250000
Loan Details,Interest Rate,8.5%
Loan Details,Term,10 years
Loan Details,SBA Guarantee Fee,3.0% ($7500)
Loan Details,Guarantee Fee Financed,Yes
Loan Details,Other Closing Costs,$5000
Business Financials,Annual Revenue,$1500000
Business Financials,Operating Expenses,$1200000
Key Results,Monthly Payment,$3138.74
Key Results,Total Interest,$119149.09
Key Results,Total SBA Fees,$12500.00
Key Results,Total Amount Paid,$386649.09
Advanced Results,Effective APR,9.28%
Advanced Results,DSCR,1.48
Advanced Results,Annual Debt Service,$37664.88
Advanced Results,Net Operating Income,$300000
Advanced Results,SBA Guaranteed Portion,$212500
```

**Excel Formatting**:
- Bold section headers
- Currency cells: $#,##0.00
- Percentage cells: 0.00%
- Alternating row colors (light gray for even rows)
- Conditional formatting: DSCR < 1.25 (red), DSCR 1.25-1.75 (yellow), DSCR > 1.75 (green)

---

## 10. Tier-Specific Behavior

### Free Tier

**What's Shown**:
- Single scenario (cannot save or create multiple)
- Loan details inputs (loan amount, interest rate, term, SBA fee, closing costs)
- Business financials inputs (revenue, expenses) - but DSCR output is locked
- Key metrics: monthly payment, total interest, total SBA fees, total amount paid
- Basic warnings (high effective APR, DSCR warning if applicable)

**What's Gated**:
- **Multiple scenarios**: "Add Scenario" button shows Pro badge, triggers upgrade prompt
- **Effective APR**: Shown as locked with tooltip "Upgrade to Pro to see effective APR including all fees"
- **DSCR and advanced metrics**: Shown as locked with tooltip "Upgrade to Pro to see DSCR analysis"
- **SBA vs Conventional comparison**: Chart preview shown with blur effect + "Pro" badge
- **Cost breakdown chart**: Teaser preview with "Pro" badge
- **Clean exports**: PDF has watermark
- **AI narratives**: "Explain SBA fees" button triggers AI tier upgrade prompt

**Upgrade Triggers**:
- Click "Add Scenario" → `upgrade_prompt_shown` with `trigger_reason: "add_scenario"`
- Click locked Effective APR → `upgrade_prompt_shown` with `trigger_reason: "locked_metric"`
- Click "Compare to Conventional Loan" → `upgrade_prompt_shown` with `trigger_reason: "comparison_chart"`
- Click "Explain SBA fees" → `upgrade_prompt_shown` with `trigger_reason: "ai_request"`

### Pro Tier

**What Unlocks**:
- **Multiple scenarios** (up to 50): Create, save, rename, delete scenarios
- **All advanced metrics**: Effective APR, DSCR, guaranteed portion, annual debt service
- **SBA vs Conventional comparison**: Full interactive comparison with side-by-side metrics
- **Cost breakdown chart**: Stacked bar showing how much is principal vs interest vs SBA fees
- **Financed vs Cash Fee comparison**: Chart showing cost difference over time
- **Scenario comparison**: Side-by-side view of multiple SBA loan structures
- **Clean exports**: PDF with no watermark
- **Sensitivity analysis**: "What if" scenarios (different terms, cash vs financed fee)

**Still Gated** (requires AI tier):
- AI explanations for SBA fees and loan approval likelihood
- AI scenario suggestions

### AI Tier

**What Unlocks**:
- **AI explanations**: Click "Explain SBA fees" to get plain-language explanation
- **AI comparison insights**: "Should I choose SBA or conventional?" gets AI recommendation
- **AI scenario suggestions**: Click "Get suggestions" for alternative loan structures
- **50 AI requests per month** (hard cap)

**Usage Quota**:
- Counter displayed: "23 of 50 AI requests remaining this month"
- Resets on 1st of each month

---

## Implementation Notes

**Priority**: High (SEO value + affiliate revenue driver)

**Estimated Effort**: 2.5 weeks
- Week 1: SBA fee calculation logic, tiered fee structure
- Week 2: Effective APR calculation, SBA vs conventional comparison
- Week 3 (half): Testing, golden scenarios, SBA lender affiliate integration

**Dependencies**:
- Shared amortization formula library (reuse from business-loan-dscr calculator)
- DSCR calculation component (reuse from business-loan-dscr)
- IRR/effective APR calculation library (new, can be reused for other calculators)

**Related Calculators**:
- **Business Loan + DSCR Calculator**: Simpler version without SBA-specific fees
- **Equipment Lease vs Buy**: Alternative financing option for equipment
- **Line of Credit**: Alternative working capital financing

**Affiliate Integration**:
- Partner with SBA-preferred lenders (e.g., Lendio, Fundera, SmartBiz)
- "Find SBA Lenders" button below results → affiliate referral
- Track referral conversions for revenue attribution

---

## End of Calculator PDR

This calculator is complete and ready for implementation. All SBA-specific formulas, tiered fee structures, and comparison logic are specified in detail.
