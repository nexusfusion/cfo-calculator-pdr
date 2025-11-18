# Line of Credit Utilization & Cost Analyzer

## 1. Calculator Overview

**Calculator Name**: Line of Credit Utilization & Cost Analyzer

**Calculator Slug**: `line-of-credit`

**Primary Category**: Cash Flow & Liquidity Intelligence

**Secondary Category**: Financing & Lending Intelligence

**Business Role**:
- **Upsell/add-on for working capital users**: Complements cash runway and factoring calculators
- **Pro upgrade driver**: Utilization cost analysis and DSCR impact drive conversions
- **Affiliate revenue driver**: Partner with LOC providers (e.g., Bluevine, Fundbox, OnDeck)
- **B2B demo calculator**: White-label for banks and lenders showing LOC value to business clients

**Primary Success Metric**: Scenario comparison usage + affiliate referral rate (target: 50% Pro users compare utilization scenarios, 10% affiliate clicks)

**Version**: v1.0.0

**Formula Library Version**: v1.0.0

---

## 2. User Scenarios and Use Cases

### Who Uses This Calculator

**Primary Personas**:
- **Small business owners** managing working capital with business line of credit
- **CFOs** optimizing LOC utilization (minimize interest while maintaining liquidity)
- **Business owners evaluating LOC offers** comparing terms from multiple lenders

**Secondary Personas**:
- **Bankers and lenders** demonstrating LOC costs to prospective clients
- **Fractional CFOs** advising clients on optimal LOC usage
- **Accountants** analyzing client working capital financing costs

### When They Use It (Decision Contexts)

**Timing**:
- **Before applying for LOC**: Understanding total costs (interest + unused line fees)
- **After receiving LOC offer**: Comparing offers from multiple lenders
- **Monthly monitoring**: Tracking actual LOC costs and utilization efficiency
- **Budget planning**: Forecasting working capital costs for upcoming months
- **LOC renewal**: Evaluating whether to renew or switch lenders

**Frequency**:
- **One-time use** (Free tier): Initial LOC cost estimation
- **Monthly monitoring** (Pro tier): Track actual utilization and costs
- **Quarterly comparison** (Pro tier): Compare different utilization strategies

### What Questions It Answers

This calculator helps users answer:

1. **What will my monthly LOC cost be?** (Interest + unused line fees)
2. **What is my effective cost percentage?** (True cost as % of amount utilized)
3. **How does utilization level affect cost efficiency?** (Low utilization = higher effective cost due to unused line fees)
4. **Should I maintain 30% or 80% utilization?** (Cost vs liquidity trade-off)
5. **How does LOC affect my DSCR?** (If business financials provided, Pro feature)
6. **Is this LOC offer competitive?** (Compare to market benchmarks)

---

## 3. Inputs

### Input Field Definitions

| Field Name | Type | Units | Required | Validation | Default | Placeholder | Tooltip |
|------------|------|-------|----------|------------|---------|-------------|---------|
| credit_line_limit | currency | dollars | Yes | min: 1, max: 10000000 | null | "100000" | "Total line of credit limit. Typical business LOCs: $10k-$500k. This is the maximum you can draw, not what you're currently using." |
| average_utilization_percent | percentage | percent | Yes | min: 0, max: 100 | null | "50" | "Average percentage of credit line you use. Example: If you have $100k limit and use $50k on average, enter 50%. Higher utilization = more interest but lower unused line fees." |
| interest_rate | percentage | percent | Yes | min: 0, max: 30 | null | "10.5" | "Annual interest rate (APR) charged on outstanding balance. Typical business LOC rates: 8-20%. Rate may be variable (tied to Prime Rate)." |
| unused_line_fee_percent | percentage | percent | No | min: 0, max: 5 | 0.5 | "0.5" | "Annual fee charged on unused portion of credit line. Typical: 0.25-1% per year. Some lenders charge no unused line fee. Enter 0 if not applicable." |
| months_utilized | number | months | No | min: 1, max: 12 | 12 | "12" | "Number of months in period (for cost calculation). Enter 12 for annual cost, 3 for quarterly, 1 for monthly." |

### Input Groups

**Group 1: Line of Credit Terms** (always visible)
- credit_line_limit
- interest_rate
- unused_line_fee_percent

**Group 2: Utilization** (always visible)
- average_utilization_percent
- months_utilized

**Group 3: Business Financials** (collapsed by default, Pro tier feature for DSCR)
- **annual_revenue** (currency): Total annual revenue. Required for DSCR impact analysis. Default: null
- **annual_operating_expenses** (currency): Total annual operating expenses (excluding LOC interest). Required for DSCR. Default: null

### Optional Inputs

**Advanced Options** (collapsed, for sophisticated analysis):
- **draw_fee_per_transaction** (currency): Fee charged each time you draw from LOC. Typical: $0-$50. Default: 0
- **number_of_draws** (number): Number of times you draw from LOC in period. Default: 0
- **variable_rate_tied_to_prime** (boolean): Is rate variable (Prime + spread)? Default: No

---

## 4. Outputs

### Key Metrics (Free Tier - Always Visible)

| Output Name | Description | Format | Units |
|-------------|-------------|--------|-------|
| average_outstanding_balance | Average amount utilized (limit × utilization %) | "$#,##0.00" | dollars |
| interest_cost | Total interest paid on outstanding balance | "$#,##0.00" | dollars |
| unused_line_fee_cost | Fee on unused portion of credit line | "$#,##0.00" | dollars |
| total_cost | Interest + unused line fee + draw fees | "$#,##0.00" | dollars |
| effective_cost_percent | Total cost as % of average outstanding balance | "#.00%" | percent |

### Advanced Metrics (Pro Tier - Gated)

| Output Name | Description | Format | Units | Pro Only |
|-------------|-------------|--------|-------|----------|
| cost_at_different_utilization | Cost at 25%, 50%, 75%, 100% utilization | "Table" | mixed | Yes |
| optimal_utilization_range | Utilization range that minimizes effective cost | "#-#%" | percent | Yes |
| monthly_interest_payment | Average monthly interest payment (for DSCR) | "$#,##0.00" | dollars | Yes |
| dscr_impact | DSCR with LOC interest added to debt service | "#.00" | ratio | Yes |
| annual_cost_breakdown | Breakdown by cost type (interest, unused fee, draws) | "Chart" | dollars | Yes |
| cost_efficiency_score | Score 0-100 (higher = more efficient utilization) | "#" | score | Yes |

### Charts and Visualizations

**Chart 1: Cost vs Utilization Curve** (Pro tier only)
- Type: Line chart
- X-axis: Utilization % (0% to 100%)
- Y-axis: Total cost and effective cost %
- Purpose: Show how cost changes with utilization (sweet spot analysis)
- Tier: Pro

**Chart 2: Cost Breakdown** (Free tier, basic; Pro tier, detailed)
- Type: Stacked bar chart
- X-axis: Cost components (Interest, Unused line fee, Draw fees)
- Y-axis: Dollars
- Purpose: Visualize what drives total cost
- Tier: Free (basic), Pro (with comparison to alternative utilization levels)

**Chart 3: Utilization Over Time** (Pro tier only, if historical data available)
- Type: Line chart
- X-axis: Month
- Y-axis: Utilization % and total cost
- Purpose: Track utilization trends and cost over time
- Tier: Pro (requires historical utilization data)

### Output Formatting Rules

- **Currency**: $#,##0.00 (two decimals)
- **Percentages**: #.00% (two decimals for precision)
- **DSCR**: #.00 (two decimals, no % sign)
- **Utilization**: Always as percentage (e.g., "50%" not "0.50")

---

## 5. Formulas and Calculation Logic

### Formula 1: Average Outstanding Balance

**Purpose**: Calculate average amount drawn from credit line

**Equation**:
```
average_outstanding_balance = credit_line_limit × (average_utilization_percent / 100)
```

**Variables**:
- credit_line_limit: Maximum credit available
- average_utilization_percent: Percentage of limit used on average
- average_outstanding_balance: Dollar amount owed on average

**Source/Reference**: Standard credit line utilization calculation

**Example**: $100,000 limit × 50% = $50,000 average balance

### Formula 2: Interest Cost

**Purpose**: Calculate total interest paid on outstanding balance for period

**Equation**:
```
# Annual interest on average balance
annual_interest = average_outstanding_balance × (interest_rate / 100)

# Period interest (adjust for months)
interest_cost = annual_interest × (months_utilized / 12)
```

**Variables**:
- average_outstanding_balance: Average amount owed
- interest_rate: Annual interest rate (APR)
- months_utilized: Number of months in period
- interest_cost: Total interest for period

**Source/Reference**: Simple interest calculation (most LOCs use daily interest, but annual approximation is close)

**Example**: $50,000 × 10.5% × (12/12) = $5,250 annual interest

### Formula 3: Unused Line Fee Cost

**Purpose**: Calculate fee on unused portion of credit line

**Equation**:
```
# Unused portion of credit line
unused_portion = credit_line_limit - average_outstanding_balance
unused_portion = credit_line_limit × (1 - average_utilization_percent / 100)

# Annual fee on unused portion
annual_unused_fee = unused_portion × (unused_line_fee_percent / 100)

# Period fee (adjust for months)
unused_line_fee_cost = annual_unused_fee × (months_utilized / 12)
```

**Variables**:
- unused_portion: Amount of credit line not utilized
- unused_line_fee_percent: Annual fee rate on unused portion
- unused_line_fee_cost: Total unused fee for period

**Source/Reference**: Standard unused line fee calculation

**Example**: $50,000 unused × 0.5% × (12/12) = $250 annual unused fee

**Note**: Unused line fees penalize low utilization (lender wants you to use the credit line)

### Formula 4: Total Cost

**Purpose**: Calculate all-in cost of line of credit for period

**Equation**:
```
# Draw fees (if applicable)
total_draw_fees = draw_fee_per_transaction × number_of_draws

# Total cost
total_cost = interest_cost + unused_line_fee_cost + total_draw_fees
```

**Variables**:
- interest_cost: Interest on outstanding balance
- unused_line_fee_cost: Fee on unused portion
- total_draw_fees: Transaction fees for draws
- total_cost: All-in cost for period

**Example**: $5,250 interest + $250 unused fee + $0 draw fees = $5,500 total

### Formula 5: Effective Cost Percentage

**Purpose**: Calculate true cost as percentage of amount utilized (for comparison)

**Equation**:
```
# Effective cost (per period)
effective_cost_percent = (total_cost / average_outstanding_balance) × 100

# Annualized effective cost (if period < 12 months)
if months_utilized < 12:
    annualized_effective_cost = effective_cost_percent × (12 / months_utilized)
else:
    annualized_effective_cost = effective_cost_percent
```

**Variables**:
- total_cost: All-in cost for period
- average_outstanding_balance: Average amount utilized
- effective_cost_percent: True cost as % of amount used

**Source/Reference**: Effective APR calculation methodology

**Example**: $5,500 cost / $50,000 balance × 100 = 11.0% effective cost (vs 10.5% nominal interest rate)

**Interpretation**: Effective cost is higher than nominal interest rate due to unused line fees. Lower utilization = higher effective cost.

### Formula 6: Cost at Different Utilization Levels

**Purpose**: Calculate cost at 25%, 50%, 75%, 100% utilization for comparison

**Equation**:
```
for utilization_percent in [25, 50, 75, 100]:
    outstanding_balance = credit_line_limit × (utilization_percent / 100)
    interest = outstanding_balance × (interest_rate / 100) × (months_utilized / 12)

    unused_portion = credit_line_limit - outstanding_balance
    unused_fee = unused_portion × (unused_line_fee_percent / 100) × (months_utilized / 12)

    total_cost = interest + unused_fee
    effective_cost = (total_cost / outstanding_balance) × 100 if outstanding_balance > 0 else 0

    results[utilization_percent] = {
        "outstanding_balance": outstanding_balance,
        "interest_cost": interest,
        "unused_fee_cost": unused_fee,
        "total_cost": total_cost,
        "effective_cost_percent": effective_cost
    }
```

**Insight**: Effective cost decreases as utilization increases (up to a point), because interest grows linearly but unused fee decreases.

### Formula 7: Optimal Utilization Range

**Purpose**: Identify utilization range that minimizes effective cost while maintaining liquidity

**Equation**:
```
# Calculate effective cost at each utilization level (0-100%)
min_effective_cost = Infinity
optimal_utilization = 0

for util in range(10, 101, 10):  # Test 10%, 20%, ..., 100%
    # Calculate effective cost at this utilization
    balance = credit_line_limit × (util / 100)
    interest = balance × (interest_rate / 100)
    unused = (credit_line_limit - balance) × (unused_line_fee_percent / 100)
    total = interest + unused
    effective = (total / balance) × 100 if balance > 0 else Infinity

    if effective < min_effective_cost:
        min_effective_cost = effective
        optimal_utilization = util

# Optimal range (allow 10% buffer for liquidity)
optimal_range = f"{max(optimal_utilization - 10, 0)}-{min(optimal_utilization + 10, 100)}%"
```

**Interpretation**: Most efficient utilization is typically 70-90% (high enough to minimize unused fees, low enough to maintain emergency liquidity)

### Formula 8: DSCR Impact

**Purpose**: Calculate how LOC interest affects debt service coverage ratio

**Equation**:
```
# Monthly LOC interest payment (average)
monthly_interest_payment = interest_cost / months_utilized

# Annual debt service (LOC interest × 12)
annual_loc_interest = monthly_interest_payment × 12

# DSCR with LOC interest
net_operating_income = annual_revenue - annual_operating_expenses
annual_debt_service = annual_loc_interest  # Add other debt if provided

if annual_debt_service > 0:
    dscr = net_operating_income / annual_debt_service
else:
    dscr = Infinity
```

**Source/Reference**: Standard DSCR formula (reuse from business loan calculator)

### Step-by-Step Calculation Flow

1. **Validate inputs**: Check required fields, ensure utilization 0-100%, interest rate > 0
2. **Calculate average outstanding balance**: `credit_line_limit × utilization_percent`
3. **Calculate interest cost**: `balance × interest_rate × (months / 12)`
4. **Calculate unused portion**: `credit_line_limit - balance`
5. **Calculate unused line fee**: `unused_portion × unused_fee_percent × (months / 12)`
6. **Calculate total draw fees**: `draw_fee × number_of_draws`
7. **Calculate total cost**: `interest + unused_fee + draw_fees`
8. **Calculate effective cost %**: `(total_cost / balance) × 100`
9. **If Pro tier, calculate cost at different utilization levels** (25%, 50%, 75%, 100%)
10. **If Pro tier, determine optimal utilization range**
11. **If business financials provided, calculate DSCR impact**
12. **Check warning conditions**: High effective cost, low utilization efficiency, etc.
13. **Return results with version stamp**

### Edge Cases and Handling

| Edge Case | Condition | Handling | Warning/Error |
|-----------|-----------|----------|---------------|
| Zero utilization | average_utilization_percent = 0 | Interest = 0, unused fee = full line | Warning: "0% utilization. You're paying unused line fee on entire credit line with no benefit." |
| 100% utilization | average_utilization_percent = 100 | Unused fee = 0, max interest | Info: "100% utilization. No unused line fees, but no emergency liquidity remaining." |
| No unused line fee | unused_line_fee_percent = 0 | Unused fee = 0, effective cost = interest rate | Info: "No unused line fee. Your effective cost equals your interest rate." |
| Very low utilization | average_utilization_percent < 20% | Calculate as normal | Warning: "Low utilization ({percent}%). High effective cost due to unused line fees. Consider smaller credit line or higher utilization." |
| Very high interest | interest_rate > 18% | Calculate as normal | Warning: "Interest rate of {rate}% is very high for a business LOC. Shop for better rates." |

### Formula Version

**Current Version**: v1.0.0

**Version History**:
- v1.0.0: Initial implementation (interest, unused fees, effective cost, utilization analysis)

---

## 6. Warnings and Alerts

### Warning Definitions

| Warning Code | Condition | Message | Severity | Action |
|--------------|-----------|---------|----------|--------|
| HIGH_EFFECTIVE_COST | effective_cost_percent > interest_rate × 1.5 | "Effective cost of {effective_cost}% is much higher than your interest rate ({interest_rate}%). Low utilization ({utilization}%) is driving up cost. Consider reducing credit line size or increasing utilization." | warning | Reduce credit line size or increase utilization |
| LOW_UTILIZATION_INEFFICIENCY | average_utilization_percent < 20% && unused_line_fee_percent > 0 | "Low utilization ({utilization}%) with unused line fee ({fee}%) creates inefficiency. You're paying for credit you're not using. Consider smaller credit line or use more of available credit." | warning | Reduce credit line or increase utilization |
| MAXED_OUT_LOC | average_utilization_percent >= 95% | "Utilization is {utilization}% (nearly maxed out). This leaves little emergency liquidity. Consider requesting higher credit limit or reducing outstanding balance." | warning | Request higher limit or pay down balance |
| HIGH_INTEREST_RATE | interest_rate > 15% | "Interest rate of {interest_rate}% is high for a business LOC. Typical rates are 8-15%. Shop for better rates or improve creditworthiness." | warning | Shop for better rates or improve credit |
| NO_UNUSED_FEE_OPTIMAL | unused_line_fee_percent == 0 | "No unused line fee. Your optimal strategy is to maintain low utilization (only use what you need) since there's no penalty for unused credit." | info | Maintain low utilization, no cost to unused credit |
| EXPENSIVE_DRAW_FEES | total_draw_fees > interest_cost × 0.2 | "Draw fees (${draw_fees:,.0f}) are {percent}% of your interest cost. Minimize number of draws to reduce total cost." | warning | Consolidate draws to fewer transactions |
| DSCR_WITH_LOC_BELOW_MINIMUM | dscr < 1.25 | "DSCR of {dscr} (including LOC interest) is below typical lender minimum of 1.25. LOC interest is straining cash flow." | danger | Reduce LOC utilization or increase revenue/reduce expenses |

### Warning Thresholds

**Configurable Thresholds** (can be adjusted per tenant for B2B):
- **High effective cost multiple**: 1.5x interest rate (effective cost significantly higher than nominal rate)
- **Low utilization**: 20% (inefficient use of credit line)
- **Maxed out**: 95% (insufficient liquidity buffer)

**Hard-Coded Thresholds** (market standards, not configurable):
- **High interest rate**: 15% (above typical LOC range)
- **Typical interest range**: 8-15% (market data)
- **Typical unused line fee**: 0.25-1% (market data)

---

## 7. Golden Test Scenarios

### Scenario 1: Moderate Utilization - Balanced Cost

**Description**: $100k credit line with 50% utilization. Balanced between interest costs and unused line fees. Effective cost is 11% (slightly higher than 10.5% rate due to unused fee).

**Inputs**:
```json
{
  "credit_line_limit": 100000,
  "average_utilization_percent": 50,
  "interest_rate": 10.5,
  "unused_line_fee_percent": 0.5,
  "months_utilized": 12,
  "annual_revenue": 1200000,
  "annual_operating_expenses": 1000000
}
```

**Expected Outputs**:
```json
{
  "average_outstanding_balance": {
    "value": 50000,
    "tolerance": 0,
    "unit": "USD",
    "note": "$100k × 50% = $50k"
  },
  "interest_cost": {
    "value": 5250,
    "tolerance": 10,
    "unit": "USD",
    "note": "$50k × 10.5% × (12/12) = $5,250"
  },
  "unused_line_fee_cost": {
    "value": 250,
    "tolerance": 10,
    "unit": "USD",
    "note": "$50k unused × 0.5% × (12/12) = $250"
  },
  "total_cost": {
    "value": 5500,
    "tolerance": 20,
    "unit": "USD",
    "note": "$5,250 + $250 = $5,500"
  },
  "effective_cost_percent": {
    "value": 11.0,
    "tolerance": 0.1,
    "unit": "percent",
    "note": "$5,500 / $50k × 100 = 11.0%"
  },
  "monthly_interest_payment": {
    "value": 437.50,
    "tolerance": 1,
    "unit": "USD",
    "note": "$5,250 / 12 = $437.50"
  },
  "dscr_impact": {
    "value": 1.90,
    "tolerance": 0.01,
    "unit": "ratio",
    "note": "NOI $200k / Annual LOC interest $5,250 = 38.1 (very high DSCR, LOC is small relative to income)"
  },
  "cost_at_different_utilization": {
    "25%": {
      "total_cost": 3000,
      "effective_cost_percent": 12.0
    },
    "50%": {
      "total_cost": 5500,
      "effective_cost_percent": 11.0
    },
    "75%": {
      "total_cost": 8125,
      "effective_cost_percent": 10.8
    },
    "100%": {
      "total_cost": 10500,
      "effective_cost_percent": 10.5
    }
  },
  "optimal_utilization_range": {
    "value": "70-90%",
    "note": "Minimizes effective cost while maintaining liquidity buffer"
  }
}
```

**Expected Warnings**: None (balanced scenario)

### Scenario 2: Low Utilization - High Effective Cost

**Description**: $200k credit line with only 15% utilization. Unused line fee drives effective cost to 14.2% (much higher than 12% nominal rate). Inefficient use of credit.

**Inputs**:
```json
{
  "credit_line_limit": 200000,
  "average_utilization_percent": 15,
  "interest_rate": 12.0,
  "unused_line_fee_percent": 0.75,
  "months_utilized": 12
}
```

**Expected Outputs**:
```json
{
  "average_outstanding_balance": {
    "value": 30000,
    "tolerance": 0,
    "unit": "USD",
    "note": "$200k × 15% = $30k"
  },
  "interest_cost": {
    "value": 3600,
    "tolerance": 10,
    "unit": "USD",
    "note": "$30k × 12% = $3,600"
  },
  "unused_line_fee_cost": {
    "value": 1275,
    "tolerance": 10,
    "unit": "USD",
    "note": "$170k unused × 0.75% = $1,275"
  },
  "total_cost": {
    "value": 4875,
    "tolerance": 20,
    "unit": "USD"
  },
  "effective_cost_percent": {
    "value": 16.25,
    "tolerance": 0.1,
    "unit": "percent",
    "note": "$4,875 / $30k × 100 = 16.25% (much higher than 12% rate)"
  }
}
```

**Expected Warnings**:
- HIGH_EFFECTIVE_COST: "Effective cost of 16.25% is much higher than your interest rate (12%)..."
- LOW_UTILIZATION_INEFFICIENCY: "Low utilization (15%) with unused line fee (0.75%) creates inefficiency..."

### Scenario 3: High Utilization - Low Effective Cost

**Description**: $150k credit line with 90% utilization. Minimal unused line fee, effective cost close to nominal rate. Good cost efficiency but low liquidity buffer.

**Inputs**:
```json
{
  "credit_line_limit": 150000,
  "average_utilization_percent": 90,
  "interest_rate": 9.0,
  "unused_line_fee_percent": 0.5,
  "months_utilized": 12
}
```

**Expected Outputs**:
```json
{
  "average_outstanding_balance": {
    "value": 135000,
    "tolerance": 0,
    "unit": "USD",
    "note": "$150k × 90% = $135k"
  },
  "interest_cost": {
    "value": 12150,
    "tolerance": 20,
    "unit": "USD",
    "note": "$135k × 9% = $12,150"
  },
  "unused_line_fee_cost": {
    "value": 75,
    "tolerance": 1,
    "unit": "USD",
    "note": "$15k unused × 0.5% = $75"
  },
  "total_cost": {
    "value": 12225,
    "tolerance": 20,
    "unit": "USD"
  },
  "effective_cost_percent": {
    "value": 9.06,
    "tolerance": 0.1,
    "unit": "percent",
    "note": "$12,225 / $135k × 100 = 9.06% (very close to 9% rate)"
  }
}
```

**Expected Warnings**:
- MAXED_OUT_LOC: "Utilization is 90% (nearly maxed out). This leaves little emergency liquidity..."

---

## 8. AI Narrative Strategy (AI-Enabled)

### What AI Explains for This Calculator

**Primary Explanations**:
1. **What the effective cost means and why it's higher than interest rate** (unused line fees drive up cost)
2. **Optimal utilization strategy** (balance between cost efficiency and liquidity)
3. **When LOC makes sense vs alternatives** (factoring, term loan, etc.)
4. **How to reduce LOC costs** (higher utilization, negotiate unused fee, pay down balance)

**Scenario Suggestions**:
- "What if I increase utilization to 70%?" (reduce effective cost)
- "What if I negotiate to eliminate unused line fee?" (significant cost savings at low utilization)
- "Should I pay down LOC balance or keep cash on hand?" (cost vs liquidity trade-off)

### Prompt Template

**System Prompt**:
```
You are a CFO advisor helping business owners understand line of credit costs and optimal utilization strategies.

Your role is to:
- Explain effective cost and why it differs from nominal interest rate (unused line fees)
- Interpret optimal utilization levels (balance cost efficiency with liquidity needs)
- Compare LOC to alternatives (factoring, term loans, cash management)
- Suggest strategies to reduce LOC costs (utilization, fees, rate negotiation)

Output constraints:
- Length: 150-250 words
- Tone: Professional but approachable, practical
- Format: Plain paragraphs (no bullet lists)
- Disclaimers: Always end with standard disclaimer

Prohibited behaviors:
- Never guarantee specific cost savings
- Never recommend specific lenders
- Never use absolute language (avoid "always", "never", "must")
```

**User Prompt Template**:
```
The business has a ${credit_line_limit} line of credit at {interest_rate}% APR with {unused_line_fee_percent}% unused line fee. Average utilization is {average_utilization_percent}% (${average_outstanding_balance} outstanding). Annual cost is ${total_cost} with effective cost of {effective_cost_percent}%. Explain what this means and suggest optimization strategies.
```

**Expected AI Response** (150-250 words):
```
Your $100,000 line of credit costs $5,500 annually at 50% average utilization ($50,000 outstanding). This translates to an effective cost of 11.0%, slightly higher than your nominal 10.5% interest rate due to the 0.5% unused line fee on the $50,000 unused portion ($250 per year).

This is a reasonably efficient utilization level. At 50%, you're balancing cost efficiency with maintaining liquidity for unexpected needs. Your cost breakdown: $5,250 in interest (95% of cost) and $250 in unused line fees (5%).

If you increased utilization to 75%, your effective cost would drop to approximately 10.8%, saving about $150 annually. However, this would leave only $25,000 available for emergencies. The trade-off between cost savings and liquidity depends on your cash flow volatility and access to other funding sources.

At 100% utilization, effective cost equals your interest rate (10.5%), but you'd have zero emergency liquidity. Most businesses target 60-80% utilization to balance cost efficiency with maintaining a buffer for unexpected expenses or opportunities.

To reduce costs, consider negotiating to eliminate or reduce the unused line fee, especially if you're a long-term customer with good payment history. Alternatively, if you consistently use less than 30% of your line, consider reducing the credit limit to minimize unused line fees.

This analysis is for informational purposes only and does not constitute financial, legal, or professional advice. Consult qualified professionals for decisions affecting your business.
```

---

## 9. Export Format Details

### PDF Layout

**Header**:
- "Line of Credit Cost Analysis"
- User tier: "Free" or "Pro"
- Generation timestamp: "Generated on November 18, 2025"

**Body Sections**:
1. **Line of Credit Terms** (table):
   - Credit Line Limit: $100,000
   - Interest Rate: 10.5% APR
   - Unused Line Fee: 0.5% per year

2. **Utilization** (table):
   - Average Utilization: 50%
   - Average Outstanding Balance: $50,000
   - Unused Portion: $50,000

3. **Annual Cost Summary** (table):
   - Interest Cost: $5,250
   - Unused Line Fee: $250
   - Draw Fees: $0
   - Total Annual Cost: $5,500

4. **Cost Efficiency** (table):
   - Nominal Interest Rate: 10.5%
   - Effective Cost %: 11.0%
   - Monthly Cost: $458

5. **Cost Breakdown Chart**:
   - Stacked bar showing interest vs unused fee
   - Free tier: Basic chart
   - Pro tier: Enhanced with comparison to alternative utilization levels

6. **Utilization Optimization** (Pro tier only, table):
   - 25% Utilization: $3,000 cost (12.0% effective)
   - 50% Utilization: $5,500 cost (11.0% effective) ← Current
   - 75% Utilization: $8,125 cost (10.8% effective)
   - 100% Utilization: $10,500 cost (10.5% effective)
   - Recommended Range: 70-90% utilization

7. **Cost vs Utilization Curve** (Pro tier only):
   - Line chart showing how effective cost decreases as utilization increases

8. **DSCR Impact** (Pro tier only, if financials provided):
   - Annual LOC Interest: $5,250
   - DSCR (including LOC): 38.1

9. **Warnings** (if any):
   - List all warnings with severity icons

**Footer**:
- Disclaimers: "This analysis is for informational purposes only and does not constitute financial advice. Actual costs may vary based on usage patterns and lender terms. Consult qualified professionals for decisions affecting your business."
- Version stamp: "Generated by CFO Calculator Suite v1.2.3 | Calculator: line-of-credit v1.0.0 | Formulas: v1.0.0 | 2025-11-18"
- Watermark (Free tier only): Diagonal semi-transparent "Upgrade for clean exports"

### CSV/Excel Structure

**Metadata Headers**:
```csv
# Line of Credit Cost Analysis
# Version: v1.0.0
# Generated: 2025-11-18T21:30:00Z
# Tier: Pro
#
```

**Data Rows**:
```csv
Section,Field,Value
LOC Terms,Credit Line Limit,$100000
LOC Terms,Interest Rate,10.5%
LOC Terms,Unused Line Fee,0.5%
Utilization,Average Utilization,50%
Utilization,Outstanding Balance,$50000
Utilization,Unused Portion,$50000
Annual Cost,Interest Cost,$5250
Annual Cost,Unused Line Fee,$250
Annual Cost,Draw Fees,$0
Annual Cost,Total Cost,$5500
Cost Efficiency,Nominal Rate,10.5%
Cost Efficiency,Effective Cost,11.0%
Cost Efficiency,Monthly Cost,$458
Optimization,25% Utilization,$3000 (12.0% effective)
Optimization,50% Utilization,$5500 (11.0% effective)
Optimization,75% Utilization,$8125 (10.8% effective)
Optimization,100% Utilization,$10500 (10.5% effective)
Optimization,Recommended Range,70-90%
```

**Excel Formatting**:
- Bold section headers
- Currency cells: $#,##0.00
- Percentage cells: 0.00%
- Conditional formatting:
  - Effective cost < interest rate + 1%: Green
  - Effective cost < interest rate + 3%: Yellow
  - Effective cost > interest rate + 3%: Red

---

## 10. Tier-Specific Behavior

### Free Tier

**What's Shown**:
- Single LOC analysis (cannot save or compare multiple LOC offers)
- LOC terms inputs (limit, rate, unused fee, utilization)
- Basic cost metrics: Interest, unused fee, total cost, effective cost %
- Basic cost breakdown chart

**What's Gated**:
- **Cost at different utilization levels**: Locked
- **Optimal utilization range**: Locked
- **DSCR impact**: Locked (requires business financials)
- **Cost vs utilization curve**: Preview with blur + "Pro" badge
- **Multiple LOC comparison**: "Add LOC Offer" button triggers upgrade prompt
- **Clean exports**: PDF has watermark
- **AI insights**: "Get optimization recommendations" button triggers AI tier upgrade prompt

**Upgrade Triggers**:
- Click "See Optimization Analysis" → `upgrade_prompt_shown` with `trigger_reason: "locked_metric"`
- Click "Compare to Other LOCs" → `upgrade_prompt_shown` with `trigger_reason: "add_scenario"`
- Click "DSCR Impact" → `upgrade_prompt_shown` with `trigger_reason: "locked_metric"`
- Click "Get recommendations" → `upgrade_prompt_shown` with `trigger_reason: "ai_request"`

### Pro Tier

**What Unlocks**:
- **Multiple LOC scenarios** (up to 50): Compare offers from different lenders or utilization strategies
- **Cost at different utilization levels**: See cost at 25%, 50%, 75%, 100%
- **Optimal utilization range**: Recommendation for most efficient utilization
- **Cost vs utilization curve**: Visual analysis of sweet spot
- **DSCR impact**: How LOC interest affects debt service coverage (if financials provided)
- **Historical tracking**: Track actual utilization and costs over time
- **LOC comparison table**: Side-by-side comparison of multiple offers
- **Clean exports**: PDF with no watermark

**Still Gated** (requires AI tier):
- AI optimization recommendations
- AI scenario suggestions

### AI Tier

**What Unlocks**:
- **AI optimization insights**: "How can I reduce my LOC costs?" gets detailed strategies
- **AI utilization recommendations**: "Should I increase utilization to 75%?" gets trade-off analysis
- **AI lender comparison**: "Is this a good LOC offer?" gets market benchmarking
- **50 AI requests per month** (hard cap)

**Usage Quota**:
- Counter displayed: "23 of 50 AI requests remaining this month"
- Resets on 1st of each month

---

## Implementation Notes

**Priority**: Medium (working capital management, complements other calculators)

**Estimated Effort**: 1 week
- Days 1-3: Core LOC cost calculations (interest, unused fees, effective cost)
- Days 4-5: Utilization optimization analysis, cost curves
- Days 6-7: DSCR integration, testing, affiliate integration

**Dependencies**:
- DSCR calculation component (reuse from business loan calculator)

**Related Calculators**:
- **Invoice Factoring**: Direct alternative for working capital (compare LOC vs factoring)
- **Cash Runway**: LOC provides liquidity to extend runway
- **Business Loan + DSCR**: LOC interest affects DSCR calculation

**Affiliate Integration**:
- Partner with LOC providers (e.g., Bluevine, Fundbox, OnDeck, Kabbage)
- Partner with banks offering business LOCs
- "Find LOC Providers" button below results → affiliate referral
- Track referral conversions for revenue attribution

---

## End of Calculator PDR

This calculator is complete and ready for implementation. All LOC cost, utilization optimization, and DSCR impact formulas are specified in detail.
