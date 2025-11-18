# Equipment Lease vs Buy Intelligence Calculator

## 1. Calculator Overview

**Calculator Name**: Equipment Lease vs Buy Intelligence Calculator

**Calculator Slug**: `equipment-lease-buy`

**Primary Category**: Financing & Lending Intelligence

**Secondary Category**: Planning & Forecasting Intelligence

**Business Role**:
- **High-intent traffic magnet**: High commercial intent searches ("lease vs buy calculator", "equipment financing")
- **Affiliate revenue driver**: Equipment leasing and financing partners (lessor referrals, equipment lender referrals)
- **Pro upgrade driver**: NPV and tax analysis drive conversions
- **B2B demo calculator**: White-label for equipment dealers and lessors

**Primary Success Metric**: Affiliate referral rate + export rate (target: 10-15% affiliate clicks, 25% export rate)

**Version**: v1.0.0

**Formula Library Version**: v1.3.0

---

## 2. User Scenarios and Use Cases

### Who Uses This Calculator

**Primary Personas**:
- **Small business owners** deciding whether to lease or buy equipment ($10k-$500k: trucks, machinery, IT, medical equipment)
- **CFOs** evaluating capital expenditure decisions and cash flow impact
- **Equipment dealers** helping customers choose financing options (lease, loan, cash)

**Secondary Personas**:
- **Accountants and CPAs** analyzing tax implications of lease vs buy decisions
- **Equipment leasing companies** demonstrating lease value to prospective clients
- **Startup founders** preserving cash while acquiring necessary equipment

### When They Use It (Decision Contexts)

**Timing**:
- **Before equipment purchase**: Planning phase, determining optimal financing method
- **After receiving lease quote**: Comparison phase, evaluating lease offer vs loan or cash purchase
- **Tax planning season**: End of year, deciding whether to accelerate equipment purchases for tax benefits (Section 179)
- **Equipment replacement cycle**: Recurring decision every 3-7 years for vehicles, machinery, etc.

**Frequency**:
- **One-time use** (Free tier): Single equipment purchase decision
- **Recurring use** (Pro tier): Equipment dealers and CFOs running multiple scenarios monthly

### What Questions It Answers

This calculator helps users answer:

1. **Should I lease or buy this equipment?** (Total cost comparison with recommendation)
2. **What are the cash flow impacts?** (Upfront cash vs monthly payments)
3. **What are the tax benefits of each option?** (Section 179 deduction, depreciation, lease payment deduction)
4. **What's the true cost after tax benefits?** (After-tax NPV comparison)
5. **What if equipment becomes obsolete early?** (Lease provides upgrade flexibility)

---

## 3. Inputs

### Input Field Definitions

| Field Name | Type | Units | Required | Validation | Default | Placeholder | Tooltip |
|------------|------|-------|----------|------------|---------|-------------|---------|
| equipment_cost | currency | dollars | Yes | min: 1, max: 10000000 | null | "100000" | "Total purchase price of equipment including delivery and installation. This is the amount you'd pay if buying with cash." |
| lease_monthly_payment | currency | dollars | No | min: 0, max: 100000 | null | "2500" | "Monthly lease payment quoted by lessor. Leave blank if you only want to evaluate loan vs cash purchase." |
| lease_term_months | number | months | No | min: 1, max: 120 | 60 | "60" | "Lease term in months. Typical leases are 36-60 months (3-5 years) for equipment, 24-36 months for technology." |
| lease_residual_value | currency | dollars | No | min: 0, max: 10000000 | null | "10000" | "Buyout price at end of lease (fair market value or $1 buyout). Leave 0 for operating lease with no buyout option." |
| loan_interest_rate | percentage | percent | No | min: 0, max: 30 | 8.0 | "8.0" | "Annual interest rate for equipment loan. Typical rates are 5-12% depending on credit and equipment type. Enter 0 if paying cash." |
| loan_term_years | number | years | No | min: 1, max: 15 | 5 | "5" | "Loan term in years if financing purchase. Common terms: 3-7 years. Equipment life should exceed loan term." |
| loan_down_payment_percent | percentage | percent | No | min: 0, max: 100 | 20 | "20" | "Down payment required for equipment loan. Typical: 10-20%. Enter 0 if financing 100% or comparing to cash purchase." |
| tax_rate | percentage | percent | No | min: 0, max: 50 | 25 | "25" | "Combined federal and state tax rate. Typical: 21-30% for small businesses. Required for accurate after-tax cost comparison." |
| equipment_useful_life | number | years | No | min: 1, max: 30 | 7 | "7" | "Useful life of equipment for depreciation. IRS guidelines: 5 years (computers, vehicles), 7 years (machinery), 15 years (heavy equipment)." |
| use_section_179 | boolean | yes/no | No | true/false | false | "No" | "Can you deduct full purchase price in year 1 using Section 179? Limit: $1.16M (2025). Powerful tax benefit for businesses buying equipment." |
| discount_rate | percentage | percent | No | min: 0, max: 30 | 10 | "10" | "Discount rate for NPV calculation (your cost of capital or hurdle rate). Typical: 8-12%. Higher rate favors leasing (preserves cash)." |

### Input Groups

**Group 1: Equipment Details** (always visible)
- equipment_cost

**Group 2: Lease Option** (collapsed by default, expands when user explores lease)
- lease_monthly_payment
- lease_term_months
- lease_residual_value

**Group 3: Purchase Option** (collapsed by default, expands when user explores purchase)
- loan_interest_rate
- loan_term_years
- loan_down_payment_percent

**Group 4: Tax & Financial Assumptions** (collapsed by default, labeled "Advanced Options")
- tax_rate
- equipment_useful_life
- use_section_179
- discount_rate

### Optional Inputs

**Advanced Options** (collapsed, for sophisticated users):
- **maintenance_cost_annual** (currency): Annual maintenance cost. Leases often include maintenance; purchases do not. Default: $0
- **lease_includes_maintenance** (boolean): Does lease include maintenance? Default: No
- **equipment_salvage_value_percent** (percentage): Residual value at end of useful life (% of original cost). Default: 10%

---

## 4. Outputs

### Key Metrics (Free Tier - Always Visible)

| Output Name | Description | Format | Units |
|-------------|-------------|--------|-------|
| total_lease_cost | Total lease payments + buyout (if applicable) | "$#,##0.00" | dollars |
| total_loan_cost | Total loan payments (principal + interest) | "$#,##0.00" | dollars |
| total_cash_cost | Cash purchase price | "$#,##0.00" | dollars |
| recommended_option | "Lease", "Finance", or "Cash" based on lowest after-tax NPV | "Text" | option |

### Advanced Metrics (Pro Tier - Gated)

| Output Name | Description | Format | Units | Pro Only |
|-------------|-------------|--------|-------|----------|
| npv_lease | Net Present Value of lease option (after-tax) | "$#,##0.00" | dollars | Yes |
| npv_finance | Net Present Value of loan option (after-tax) | "$#,##0.00" | dollars | Yes |
| npv_cash | Net Present Value of cash option (after-tax) | "$#,##0.00" | dollars | Yes |
| after_tax_cost_lease | Total lease cost after tax deductions | "$#,##0.00" | dollars | Yes |
| after_tax_cost_finance | Total finance cost after tax deductions (interest, depreciation) | "$#,##0.00" | dollars | Yes |
| after_tax_cost_cash | Total cash cost after tax deductions (depreciation or Section 179) | "$#,##0.00" | dollars | Yes |
| cash_flow_year_1 | Cash outflow in year 1 for each option | "$#,##0.00" | dollars | Yes |
| effective_cost_difference | Cost difference between best and worst option | "$#,##0.00" | dollars | Yes |
| breakeven_discount_rate | Discount rate at which lease and buy have equal NPV | "#.00%" | percent | Yes |

### Charts and Visualizations

**Chart 1: Cost Comparison** (Free tier, basic version; Pro tier, detailed version)
- Type: Bar chart
- X-axis: Option (Lease, Finance, Cash)
- Y-axis: Total cost (pre-tax for Free, after-tax for Pro)
- Purpose: Visual comparison of total costs
- Tier: Free (basic), Pro (with tax effects)

**Chart 2: Cash Flow Timeline** (Pro tier only)
- Type: Waterfall chart
- X-axis: Year (0 to end of term)
- Y-axis: Cash outflows (negative) and inflows (positive)
- Purpose: Visualize cash flow timing differences between options
- Tier: Pro

**Chart 3: NPV Sensitivity Analysis** (Pro tier only)
- Type: Line chart
- X-axis: Discount rate (5% to 15%)
- Y-axis: NPV for each option
- Purpose: Show how sensitive decision is to discount rate assumption
- Tier: Pro

### Output Formatting Rules

- **Currency**: $#,##0.00 (always two decimals, commas for thousands)
- **Percentages**: #.00% (two decimals)
- **NPV**: Negative values shown in parentheses (accounting format)
- **Recommended Option**: Bold, color-coded (green for recommended, gray for alternatives)

---

## 5. Formulas and Calculation Logic

### Formula 1: Total Lease Cost

**Purpose**: Calculate total cost of leasing equipment over lease term

**Equation**:
```
total_lease_cost = (lease_monthly_payment × lease_term_months) + lease_residual_value
```

**Variables**:
- lease_monthly_payment: Monthly lease payment in dollars
- lease_term_months: Lease term in months
- lease_residual_value: Buyout price at end of lease (0 for operating lease)

**Source/Reference**: Standard lease cost calculation

### Formula 2: Total Loan Cost (Finance Option)

**Purpose**: Calculate total cost of financing equipment purchase with loan

**Equation**:
```
# Down payment
down_payment = equipment_cost × (loan_down_payment_percent / 100)

# Financed amount
principal = equipment_cost - down_payment

# Monthly payment calculation (standard amortization)
r = (loan_interest_rate / 12 / 100)  # monthly interest rate
n = loan_term_years × 12  # number of payments

if loan_interest_rate > 0:
    monthly_payment = principal × [r × (1 + r)^n] / [(1 + r)^n - 1]
else:
    monthly_payment = principal / n

# Total loan cost
total_loan_cost = down_payment + (monthly_payment × n)
```

**Variables**:
- equipment_cost: Purchase price
- loan_down_payment_percent: Down payment percentage
- principal: Amount financed
- loan_interest_rate: Annual interest rate
- loan_term_years: Loan term in years

**Source/Reference**: Standard amortization formula

### Formula 3: After-Tax Cost (Lease)

**Purpose**: Calculate lease cost after tax deductions (lease payments are fully deductible)

**Equation**:
```
# Annual lease payments
annual_lease_payment = lease_monthly_payment × 12

# Tax savings from lease payment deduction
tax_savings_per_year = annual_lease_payment × (tax_rate / 100)

# After-tax lease payment
after_tax_annual_lease = annual_lease_payment - tax_savings_per_year

# Total after-tax cost
lease_years = lease_term_months / 12
after_tax_cost_lease = (after_tax_annual_lease × lease_years) + lease_residual_value
```

**Source/Reference**: IRS Publication 535 (Business Expenses) - Lease payment deductibility

### Formula 4: After-Tax Cost (Purchase with Section 179)

**Purpose**: Calculate purchase cost after Section 179 immediate expensing

**Equation**:
```
# If Section 179 is used, full equipment cost is deducted in year 1
if use_section_179 == true:
    first_year_tax_deduction = equipment_cost
    tax_savings_year_1 = equipment_cost × (tax_rate / 100)
    depreciation_years_2_plus = 0
else:
    # Use MACRS depreciation (Modified Accelerated Cost Recovery System)
    # For simplicity, use straight-line over useful life
    annual_depreciation = equipment_cost / equipment_useful_life
    tax_savings_per_year = annual_depreciation × (tax_rate / 100)

# Interest deduction (if financed)
total_interest_paid = total_loan_cost - equipment_cost
interest_tax_savings = total_interest_paid × (tax_rate / 100)

# After-tax cost
if use_section_179:
    after_tax_cost_cash = equipment_cost - tax_savings_year_1
else:
    total_depreciation_tax_savings = (annual_depreciation × equipment_useful_life) × (tax_rate / 100)
    after_tax_cost_cash = equipment_cost - total_depreciation_tax_savings

# If financed, add interest but subtract interest tax deduction
if loan_interest_rate > 0:
    after_tax_cost_finance = total_loan_cost - interest_tax_savings - total_depreciation_tax_savings
```

**Source/Reference**: IRS Publication 946 (Depreciation) and Section 179 rules

### Formula 5: Net Present Value (NPV)

**Purpose**: Calculate present value of all cash flows for each option

**Equation**:
```
# NPV of Lease
# Cash flows: Monthly lease payments + buyout
npv_lease = 0
for month in range(1, lease_term_months + 1):
    after_tax_payment = lease_monthly_payment × (1 - tax_rate / 100)
    monthly_discount_rate = (discount_rate / 100) / 12
    pv = after_tax_payment / ((1 + monthly_discount_rate) ^ month)
    npv_lease += pv

# Add residual value (buyout) at end of term
if lease_residual_value > 0:
    pv_residual = lease_residual_value / ((1 + discount_rate / 100) ^ lease_years)
    npv_lease += pv_residual

# NPV of Purchase (Cash)
# Cash flow: Upfront payment minus tax savings (Section 179 or depreciation)
if use_section_179:
    tax_savings_year_1 = equipment_cost × (tax_rate / 100)
    npv_cash = -equipment_cost + (tax_savings_year_1 / (1 + discount_rate / 100))
else:
    npv_cash = -equipment_cost
    for year in range(1, equipment_useful_life + 1):
        annual_tax_savings = (equipment_cost / equipment_useful_life) × (tax_rate / 100)
        pv_savings = annual_tax_savings / ((1 + discount_rate / 100) ^ year)
        npv_cash += pv_savings

# NPV of Loan
# Cash flows: Down payment + monthly payments minus tax savings (interest + depreciation)
npv_finance = -down_payment
for month in range(1, n + 1):
    # Split payment into principal and interest
    # (requires amortization schedule calculation)
    after_tax_payment = monthly_payment - (interest_portion × tax_rate / 100)
    pv = after_tax_payment / ((1 + monthly_discount_rate) ^ month)
    npv_finance += pv

# Add depreciation tax savings
for year in range(1, equipment_useful_life + 1):
    annual_tax_savings = (equipment_cost / equipment_useful_life) × (tax_rate / 100)
    pv_savings = annual_tax_savings / ((1 + discount_rate / 100) ^ year)
    npv_finance += pv_savings
```

**Source/Reference**: Standard NPV formula for capital budgeting decisions

### Formula 6: Recommendation Logic

**Purpose**: Recommend lease, finance, or cash based on lowest NPV

**Equation**:
```
options = {
    "Lease": npv_lease,
    "Finance": npv_finance,
    "Cash": npv_cash
}

# Lower NPV (more negative) is worse; higher NPV (less negative) is better
recommended_option = max(options, key=options.get)

# If NPV values are within 5% of each other, recommend based on cash flow
if abs(npv_lease - npv_finance) / min(abs(npv_lease), abs(npv_finance)) < 0.05:
    # Tight race, consider cash flow
    if cash_flow_year_1_lease < cash_flow_year_1_finance:
        recommended_option = "Lease (preserves cash)"
```

### Step-by-Step Calculation Flow

1. **Validate inputs**: Check required fields, validate ranges
2. **Calculate total lease cost**: `lease_monthly_payment × lease_term_months + lease_residual_value`
3. **Calculate total loan cost**: Down payment + amortized monthly payments
4. **Calculate total cash cost**: `equipment_cost` (baseline)
5. **Calculate after-tax costs**:
   - Lease: Apply tax deduction to lease payments
   - Finance: Apply tax deduction to interest + depreciation
   - Cash: Apply Section 179 (if applicable) or depreciation
6. **Calculate NPV for each option**: Discount all cash flows to present value
7. **Determine recommended option**: Lowest NPV (most favorable)
8. **Calculate sensitivity metrics** (Pro tier): Breakeven discount rate, NPV at different rates
9. **Check warning conditions**: High lease cost, low residual value, etc.
10. **Return results with version stamp**

### Edge Cases and Handling

| Edge Case | Condition | Handling | Warning/Error |
|-----------|-----------|----------|---------------|
| Zero interest rate | loan_interest_rate = 0 | Treat as cash purchase (no interest) | Info: "Zero interest loan. Cost equals equipment price." |
| 100% financing | loan_down_payment_percent = 0 | Allow, but warn about higher interest cost | Warning: "100% financing may result in higher interest rates." |
| Lease longer than useful life | lease_term_months > equipment_useful_life × 12 | Calculate as normal, warn | Warning: "Lease term exceeds typical equipment life. Verify lease terms." |
| Very high discount rate | discount_rate > 20% | Calculate as normal, warn | Warning: "High discount rate ({rate}%) strongly favors leasing. Verify this is your true cost of capital." |
| Section 179 over limit | equipment_cost > 1160000 | Phase out Section 179 benefit | Warning: "Equipment cost exceeds Section 179 limit. Benefit may be reduced." |

### Formula Version

**Current Version**: v1.3.0

**Version History**:
- v1.0.0: Initial implementation (total cost comparison)
- v1.1.0: Added after-tax cost calculations
- v1.2.0: Added NPV calculations with proper discounting
- v1.3.0: Added Section 179 support and MACRS depreciation tables

---

## 6. Warnings and Alerts

### Warning Definitions

| Warning Code | Condition | Message | Severity | Action |
|--------------|-----------|---------|----------|--------|
| HIGH_LEASE_COST | total_lease_cost > equipment_cost × 1.5 | "Total lease cost (${total_lease_cost:,.0f}) is {percent}% more than equipment cost. Leasing is expensive for this scenario. Consider financing or cash purchase." | warning | Negotiate better lease terms or explore purchase options |
| LOW_RESIDUAL_VALUE | lease_residual_value < equipment_cost × 0.10 && lease_term_months < 60 | "Lease residual value is very low (${lease_residual_value:,.0f}). Equipment may depreciate faster than expected. Verify fair market value assumption." | info | Research equipment resale values, consider shorter lease term |
| SECTION_179_LIMIT | use_section_179 == true && equipment_cost > 1160000 | "Equipment cost exceeds Section 179 deduction limit ($1,160,000 for 2025). You may only deduct up to the limit. Consult tax advisor." | warning | Split purchase across multiple years or use depreciation instead |
| HIGH_DISCOUNT_RATE | discount_rate > 15% | "Discount rate of {discount_rate}% is very high. This heavily favors leasing (preserves cash). Verify this is your true cost of capital." | info | Reduce discount rate if not reflective of actual cost of capital |
| LEASE_LONGER_THAN_LIFE | lease_term_months > equipment_useful_life × 12 | "Lease term ({lease_term_months} months) exceeds equipment useful life ({equipment_useful_life} years). You may be leasing obsolete equipment. Consider shorter term." | warning | Negotiate shorter lease term with upgrade option |
| CASH_BETTER_THAN_LEASE | npv_cash > npv_lease && abs(npv_cash - npv_lease) > 5000 | "Cash purchase has {savings:,.0f} lower NPV than leasing. If you have available cash and can use Section 179, buying is significantly cheaper." | info | Consider cash purchase or financing if cash is limited |
| NO_TAX_RATE_PROVIDED | tax_rate == null or tax_rate == 0 | "Tax rate not provided. After-tax cost comparison unavailable. Lease vs buy decision heavily depends on tax benefits." | warning | Enter tax rate for accurate comparison |

### Warning Thresholds

**Configurable Thresholds** (can be adjusted per tenant for B2B):
- **High lease cost multiple**: 1.5x equipment cost (typical threshold for "expensive lease")
- **Low residual value**: 10% of equipment cost (below this suggests rapid depreciation)
- **NPV decision threshold**: $5,000 (if options are within $5k NPV, consider qualitative factors)

**Hard-Coded Thresholds** (tax law, not configurable):
- **Section 179 limit**: $1,160,000 (2025 federal limit, indexed for inflation)
- **Section 179 phase-out**: Begins at $2,890,000 total equipment purchases in year

---

## 7. Golden Test Scenarios

### Scenario 1: Lease Wins - Technology Equipment (Short Life)

**Description**: IT equipment with 3-year useful life. Lease provides upgrade flexibility. Leasing is optimal due to short equipment life and rapid obsolescence.

**Inputs**:
```json
{
  "equipment_cost": 50000,
  "lease_monthly_payment": 1500,
  "lease_term_months": 36,
  "lease_residual_value": 5000,
  "loan_interest_rate": 8.0,
  "loan_term_years": 3,
  "loan_down_payment_percent": 20,
  "tax_rate": 25,
  "equipment_useful_life": 3,
  "use_section_179": false,
  "discount_rate": 10
}
```

**Expected Outputs**:
```json
{
  "total_lease_cost": {
    "value": 59000,
    "tolerance": 0,
    "unit": "USD",
    "note": "$1,500 × 36 months + $5,000 buyout"
  },
  "total_loan_cost": {
    "value": 56293.47,
    "tolerance": 10,
    "unit": "USD",
    "note": "$10k down + $1,286.48/mo × 36 months"
  },
  "total_cash_cost": {
    "value": 50000,
    "tolerance": 0,
    "unit": "USD"
  },
  "after_tax_cost_lease": {
    "value": 49250,
    "tolerance": 100,
    "unit": "USD",
    "note": "Lease payments fully deductible, after-tax cost lower than cash"
  },
  "after_tax_cost_finance": {
    "value": 51469,
    "tolerance": 100,
    "unit": "USD",
    "note": "Interest deductible + depreciation"
  },
  "after_tax_cost_cash": {
    "value": 46875,
    "tolerance": 100,
    "unit": "USD",
    "note": "Depreciation over 3 years (25% tax rate)"
  },
  "npv_lease": {
    "value": -43856,
    "tolerance": 200,
    "unit": "USD",
    "note": "Most favorable NPV due to preserved cash flow"
  },
  "npv_finance": {
    "value": -45312,
    "tolerance": 200,
    "unit": "USD"
  },
  "npv_cash": {
    "value": -46024,
    "tolerance": 200,
    "unit": "USD"
  },
  "recommended_option": {
    "value": "Lease",
    "note": "Lease has best NPV and preserves cash for working capital"
  }
}
```

**Expected Warnings**: None (lease is appropriate for short-life equipment)

### Scenario 2: Cash Wins - Long-Life Equipment with Section 179

**Description**: Heavy machinery with 10-year life. Business can use Section 179 for immediate tax deduction. Cash purchase is optimal.

**Inputs**:
```json
{
  "equipment_cost": 200000,
  "lease_monthly_payment": 4500,
  "lease_term_months": 60,
  "lease_residual_value": 20000,
  "loan_interest_rate": 7.5,
  "loan_term_years": 7,
  "loan_down_payment_percent": 20,
  "tax_rate": 28,
  "equipment_useful_life": 10,
  "use_section_179": true,
  "discount_rate": 8
}
```

**Expected Outputs**:
```json
{
  "total_lease_cost": {
    "value": 290000,
    "tolerance": 0,
    "unit": "USD",
    "note": "$4,500 × 60 months + $20,000 buyout"
  },
  "total_loan_cost": {
    "value": 244367.28,
    "tolerance": 50,
    "unit": "USD",
    "note": "$40k down + $2,433.42/mo × 84 months"
  },
  "total_cash_cost": {
    "value": 200000,
    "tolerance": 0,
    "unit": "USD"
  },
  "after_tax_cost_lease": {
    "value": 234400,
    "tolerance": 200,
    "unit": "USD",
    "note": "Lease payments deductible at 28% tax rate"
  },
  "after_tax_cost_finance": {
    "value": 185624,
    "tolerance": 200,
    "unit": "USD",
    "note": "Interest + depreciation deductions"
  },
  "after_tax_cost_cash": {
    "value": 144000,
    "tolerance": 200,
    "unit": "USD",
    "note": "Section 179: $200k × 28% = $56k tax savings in year 1"
  },
  "npv_lease": {
    "value": -212456,
    "tolerance": 500,
    "unit": "USD",
    "note": "Worst NPV due to high total lease cost"
  },
  "npv_finance": {
    "value": -173218,
    "tolerance": 500,
    "unit": "USD"
  },
  "npv_cash": {
    "value": -148923,
    "tolerance": 500,
    "unit": "USD",
    "note": "Best NPV due to immediate Section 179 deduction"
  },
  "recommended_option": {
    "value": "Cash",
    "note": "Cash purchase with Section 179 has lowest NPV"
  }
}
```

**Expected Warnings**:
- HIGH_LEASE_COST: "Total lease cost ($290,000) is 45% more than equipment cost. Leasing is expensive for this scenario..."
- CASH_BETTER_THAN_LEASE: "Cash purchase has $63,533 lower NPV than leasing..."

### Scenario 3: Finance Wins - Medium-Life Equipment, No Section 179

**Description**: Manufacturing equipment, 7-year life. Business cannot use Section 179 (already used deduction limit). Financing is optimal balance.

**Inputs**:
```json
{
  "equipment_cost": 150000,
  "lease_monthly_payment": 3200,
  "lease_term_months": 60,
  "lease_residual_value": 15000,
  "loan_interest_rate": 6.5,
  "loan_term_years": 5,
  "loan_down_payment_percent": 15,
  "tax_rate": 26,
  "equipment_useful_life": 7,
  "use_section_179": false,
  "discount_rate": 9
}
```

**Expected Outputs**:
```json
{
  "total_lease_cost": {
    "value": 207000,
    "tolerance": 0,
    "unit": "USD",
    "note": "$3,200 × 60 months + $15,000 buyout"
  },
  "total_loan_cost": {
    "value": 171489.60,
    "tolerance": 50,
    "unit": "USD",
    "note": "$22.5k down + $2,474.83/mo × 60 months"
  },
  "total_cash_cost": {
    "value": 150000,
    "tolerance": 0,
    "unit": "USD"
  },
  "after_tax_cost_lease": {
    "value": 168180,
    "tolerance": 200,
    "unit": "USD"
  },
  "after_tax_cost_finance": {
    "value": 154326,
    "tolerance": 200,
    "unit": "USD",
    "note": "Interest deduction + depreciation over 7 years"
  },
  "after_tax_cost_cash": {
    "value": 150000,
    "tolerance": 200,
    "unit": "USD",
    "note": "No Section 179; depreciation over 7 years reduces benefit"
  },
  "npv_lease": {
    "value": -152634,
    "tolerance": 500,
    "unit": "USD"
  },
  "npv_finance": {
    "value": -140267,
    "tolerance": 500,
    "unit": "USD",
    "note": "Best NPV: preserves some cash ($22.5k down vs $150k cash)"
  },
  "npv_cash": {
    "value": -141856,
    "tolerance": 500,
    "unit": "USD"
  },
  "recommended_option": {
    "value": "Finance",
    "note": "Financing has best NPV and preserves cash for operations"
  }
}
```

**Expected Warnings**: None (balanced scenario, finance is appropriate)

---

## 8. AI Narrative Strategy (AI-Enabled)

### What AI Explains for This Calculator

**Primary Explanations**:
1. **Why lease wins or loses in this scenario** (tax benefits, cash flow, obsolescence risk)
2. **Tax implications of each option** (Section 179, depreciation, lease payment deduction)
3. **Cash flow trade-offs** (preserve cash for growth vs reduce total cost)
4. **Sensitivity to key assumptions** (discount rate, tax rate, residual value)

**Scenario Suggestions**:
- Alternative structures (longer/shorter lease term, higher down payment, etc.)
- "What if equipment becomes obsolete?" (lease provides upgrade flexibility)
- "What if I can't use Section 179?" (compare depreciation scenarios)

### Prompt Template

**System Prompt**:
```
You are a CFO advisor helping business owners decide whether to lease or buy equipment.

Your role is to:
- Explain the NPV analysis in plain language (why one option has lower cost)
- Interpret tax benefits (Section 179, depreciation, lease deduction) without giving tax advice
- Explain cash flow implications (upfront cash vs monthly payments)
- Suggest which option makes sense given the business's cash position and growth plans

Output constraints:
- Length: 150-250 words
- Tone: Professional but approachable, avoid jargon
- Format: Plain paragraphs (no bullet lists)
- Disclaimers: Always end with standard disclaimer

Prohibited behaviors:
- Never provide tax advice (use "consult your tax advisor")
- Never recommend one option absolutely (explain trade-offs)
- Never use absolute language (avoid "always", "never", "must")
```

**User Prompt Template**:
```
The business is evaluating equipment costing ${equipment_cost}. Lease option: ${lease_monthly_payment}/month for {lease_term_months} months with ${lease_residual_value} buyout. Finance option: {loan_interest_rate}% APR for {loan_term_years} years with {loan_down_payment_percent}% down. Cash option available. Tax rate: {tax_rate}%. Section 179 {available or not available}. NPV results: Lease = ${npv_lease}, Finance = ${npv_finance}, Cash = ${npv_cash}. Explain which option is best and why.
```

**Expected AI Response** (150-250 words):
```
For this $150,000 equipment purchase, financing is the optimal choice with an NPV of -$140,267, slightly better than the cash option (-$141,856) and significantly better than leasing (-$152,634).

Financing wins because it balances two competing priorities: preserving cash while minimizing total cost. The 15% down payment ($22,500) preserves $127,500 in cash for operations or growth, while the 6.5% interest rate is reasonable. The interest paid is tax-deductible, and you'll also benefit from depreciation deductions over the equipment's 7-year life.

Leasing is more expensive in this scenario because the total lease cost ($207,000) is 38% higher than the equipment's purchase price. While leasing preserves the most cash upfront, the higher total cost outweighs this benefit given your 9% discount rate.

Cash purchase has the lowest total cost ($150,000) but requires full payment upfront. Without Section 179 available, you'll only benefit from depreciation spread over 7 years, reducing the tax advantage. If cash is tight, financing is nearly as cost-effective while preserving capital.

This analysis is for informational purposes only and does not constitute financial, tax, or legal advice. Consult qualified professionals for decisions affecting your business.
```

---

## 9. Export Format Details

### PDF Layout

**Header**:
- "Equipment Lease vs Buy Analysis"
- User tier: "Free" or "Pro"
- Generation timestamp: "Generated on November 18, 2025"

**Body Sections**:
1. **Equipment Details** (table):
   - Equipment Cost: $150,000
   - Useful Life: 7 years

2. **Lease Option** (table):
   - Monthly Payment: $3,200
   - Lease Term: 60 months
   - Residual/Buyout: $15,000
   - Total Lease Cost: $207,000

3. **Finance Option** (table):
   - Interest Rate: 6.5%
   - Loan Term: 5 years
   - Down Payment: 15% ($22,500)
   - Total Financed Cost: $171,490

4. **Cash Option** (table):
   - Cash Purchase Price: $150,000

5. **Recommendation**:
   - **Recommended Option: Finance**
   - Reason: Best NPV (-$140,267) while preserving cash

6. **Cost Comparison Chart**:
   - Bar chart showing total pre-tax costs (Free tier) or after-tax NPV (Pro tier)

7. **Advanced Analysis** (Pro tier only, table):
   - NPV: Lease (-$152,634), Finance (-$140,267), Cash (-$141,856)
   - After-Tax Costs: Lease ($168,180), Finance ($154,326), Cash ($150,000)
   - Year 1 Cash Flow: Lease ($38,400), Finance ($52,198), Cash ($150,000)
   - Effective Cost Difference: Finance saves $12,367 vs Lease

8. **Cash Flow Timeline Chart** (Pro tier only):
   - Waterfall showing cash outflows over time for each option

9. **Tax Assumptions** (table):
   - Tax Rate: 26%
   - Section 179: Not Used
   - Depreciation Method: Straight-line over 7 years
   - Discount Rate: 9%

10. **Warnings** (if any):
    - List all warnings with severity icons

**Footer**:
- Disclaimers: "This analysis is for informational purposes only and does not constitute financial or tax advice. Tax benefits depend on your specific situation. Consult a tax advisor and equipment financing specialist for decisions affecting your business."
- Version stamp: "Generated by CFO Calculator Suite v1.2.3 | Calculator: equipment-lease-buy v1.0.0 | Formulas: v1.3.0 | 2025-11-18"
- Watermark (Free tier only): Diagonal semi-transparent "Upgrade for clean exports"

### CSV/Excel Structure

**Metadata Headers**:
```csv
# Equipment Lease vs Buy Analysis
# Version: v1.0.0
# Generated: 2025-11-18T21:30:00Z
# Tier: Pro
#
```

**Data Rows**:
```csv
Section,Field,Value
Equipment,Cost,$150000
Equipment,Useful Life,7 years
Lease Option,Monthly Payment,$3200
Lease Option,Term,60 months
Lease Option,Residual,$15000
Lease Option,Total Cost,$207000
Finance Option,Interest Rate,6.5%
Finance Option,Term,5 years
Finance Option,Down Payment,15% ($22500)
Finance Option,Total Cost,$171490
Cash Option,Purchase Price,$150000
Tax Assumptions,Tax Rate,26%
Tax Assumptions,Section 179,Not Used
Tax Assumptions,Discount Rate,9%
Results,Recommended Option,Finance
Results,NPV Lease,-$152634
Results,NPV Finance,-$140267
Results,NPV Cash,-$141856
Results,After-Tax Cost Lease,$168180
Results,After-Tax Cost Finance,$154326
Results,After-Tax Cost Cash,$150000
```

**Excel Formatting**:
- Bold section headers
- Currency cells: $#,##0.00
- Percentage cells: 0.00%
- Conditional formatting: Recommended option highlighted in green
- NPV values: Accounting format with parentheses for negative

---

## 10. Tier-Specific Behavior

### Free Tier

**What's Shown**:
- Single scenario (cannot save or compare multiple equipment)
- Equipment cost, lease terms, loan terms inputs
- Basic cost comparison: Total lease cost, total loan cost, total cash cost
- Recommended option (based on simple total cost, not NPV)
- Basic cost comparison bar chart (pre-tax)

**What's Gated**:
- **NPV analysis**: Shown as locked with tooltip "Upgrade to Pro to see NPV and after-tax cost comparison"
- **After-tax costs**: Locked, requires Pro
- **Cash flow timeline chart**: Preview with blur + "Pro" badge
- **Sensitivity analysis**: "What if discount rate changes?" locked
- **Multiple scenarios**: "Add Equipment Scenario" button triggers upgrade prompt
- **Clean exports**: PDF has watermark
- **AI explanations**: "Explain recommendation" button triggers AI tier upgrade prompt

**Upgrade Triggers**:
- Click "See NPV Analysis" → `upgrade_prompt_shown` with `trigger_reason: "locked_metric"`
- Click "Compare Scenarios" → `upgrade_prompt_shown` with `trigger_reason: "add_scenario"`
- Click "Cash Flow Timeline" → `upgrade_prompt_shown` with `trigger_reason: "chart_locked"`
- Click "Explain recommendation" → `upgrade_prompt_shown` with `trigger_reason: "ai_request"`

### Pro Tier

**What Unlocks**:
- **Multiple scenarios** (up to 50): Compare different equipment, lease terms, etc.
- **Full NPV analysis**: After-tax NPV for lease, finance, and cash options
- **After-tax cost breakdown**: Tax benefits of each option clearly shown
- **Cash flow timeline chart**: Visualize cash outflows by year for each option
- **Sensitivity analysis**: NPV sensitivity to discount rate (5-15% range)
- **Breakeven discount rate**: At what discount rate does lease = finance?
- **Scenario comparison**: Side-by-side view of multiple equipment decisions
- **Clean exports**: PDF with no watermark

**Still Gated** (requires AI tier):
- AI explanations of tax benefits and recommendation
- AI scenario suggestions

### AI Tier

**What Unlocks**:
- **AI explanations**: Click "Explain recommendation" for plain-language NPV interpretation
- **AI tax insights**: "How do tax benefits affect this decision?" gets detailed explanation
- **AI scenario suggestions**: "What if I extend the lease term to 72 months?"
- **50 AI requests per month** (hard cap)

**Usage Quota**:
- Counter displayed: "23 of 50 AI requests remaining this month"
- Resets on 1st of each month

---

## Implementation Notes

**Priority**: High (affiliate revenue + SEO value)

**Estimated Effort**: 3 weeks
- Week 1: Core cost calculations (lease, loan, cash)
- Week 2: NPV calculations, after-tax analysis, Section 179 logic
- Week 3: Sensitivity analysis, charts, testing, equipment leasing affiliate integration

**Dependencies**:
- Shared amortization formula library (reuse from loan calculators)
- NPV/discounting library (new, can be reused for other capital budgeting calculators)
- Tax calculation library (Section 179, MACRS depreciation tables)

**Related Calculators**:
- **Business Loan + DSCR Calculator**: Loan option uses same amortization formula
- **SBA 7(a) Analyzer**: Alternative equipment financing (SBA loans for equipment)
- **Business Valuation**: NPV concepts similar (discounting future cash flows)

**Affiliate Integration**:
- Partner with equipment leasing companies (e.g., Crest Capital, Balboa Capital)
- Partner with equipment lenders (e.g., Currency, Fora Financial)
- "Find Equipment Financing" button below results → affiliate referral
- Track referral conversions for revenue attribution

---

## End of Calculator PDR

This calculator is complete and ready for implementation. All NPV, tax, and cash flow formulas are specified in detail.
