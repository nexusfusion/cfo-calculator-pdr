# Simple Business Valuation (EBITDA/Revenue Multiple)

## 1. Calculator Overview

**Calculator Name**: Simple Business Valuation (EBITDA/Revenue Multiple)

**Calculator Slug**: `business-valuation`

**Primary Category**: Valuation & Deal Intelligence

**Secondary Category**: Planning & Forecasting Intelligence

**Business Role**:
- **Founders/investor interest driver**: High emotional value (answers "what is my business worth?")
- **AI narrative showcase**: Excellent use case for explaining valuation context and ranges
- **Pro upgrade driver**: Sensitivity analysis and industry comparisons drive conversions
- **B2B demo calculator**: White-label for business brokers, M&A advisors, investors

**Primary Success Metric**: AI narrative usage rate + export rate (target: 60% AI usage, 40% export rate)

**Version**: v1.0.0

**Formula Library Version**: v1.0.0

---

## 2. User Scenarios and Use Cases

### Who Uses This Calculator

**Primary Personas**:
- **Business owners** considering selling or curious about business value
- **Startup founders** tracking valuation trajectory (for fundraising or exit planning)
- **Business brokers and M&A advisors** providing quick valuation estimates to clients

**Secondary Personas**:
- **Investors** conducting preliminary valuation analysis of acquisition targets
- **Lenders** evaluating business value for asset-based lending
- **Accountants** providing annual valuation estimates for estate planning or partnership agreements

### When They Use It (Decision Contexts)

**Timing**:
- **Before selling business**: Initial valuation to set asking price expectations
- **During fundraising**: Understanding implied valuation from revenue/EBITDA multiples
- **After major milestone**: Checking if business value increased (new customer, revenue growth, etc.)
- **Annual planning**: Tracking business value over time (Pro users monitor quarterly/annually)
- **Partner buyout**: Estimating value for partnership restructuring

**Frequency**:
- **One-time use** (Free tier): Curiosity about current business value
- **Quarterly tracking** (Pro tier): Monitor valuation trajectory
- **Annual benchmarking** (Pro tier): Compare to industry comps

### What Questions It Answers

This calculator helps users answer:

1. **What is my business worth?** (Valuation range based on industry multiples)
2. **What multiple should I use?** (Revenue vs EBITDA, industry-specific ranges)
3. **How sensitive is valuation to revenue/EBITDA changes?** (Sensitivity analysis, Pro feature)
4. **Am I getting a fair offer?** (Compare offer to calculated valuation range)
5. **What would my business be worth if I grew revenue 20%?** (Growth scenarios, Pro feature)

---

## 3. Inputs

### Input Field Definitions

| Field Name | Type | Units | Required | Validation | Default | Placeholder | Tooltip |
|------------|------|-------|----------|------------|---------|-------------|---------|
| annual_revenue | currency | dollars | Yes | min: 1, max: 1000000000 | null | "5000000" | "Total annual revenue (trailing 12 months or TTM). Use actual revenue, not projected. Most accurate if audited financials." |
| annual_ebitda | currency | dollars | Yes | min: -10000000, max: 1000000000 | null | "750000" | "EBITDA (Earnings Before Interest, Taxes, Depreciation, Amortization). If you don't have EBITDA, enter net operating income or owner's discretionary earnings (ODE). Can be negative for early-stage businesses." |
| industry_revenue_multiple_low | number | multiple | No | min: 0, max: 20 | null | "0.5" | "Low end of industry revenue multiple range. Example: retail 0.3-0.8x, SaaS 4-8x. See industry benchmarks or consult business broker." |
| industry_revenue_multiple_high | number | multiple | No | min: 0, max: 20 | null | "1.5" | "High end of industry revenue multiple range. Higher multiples for businesses with recurring revenue, strong growth, or unique IP." |
| industry_ebitda_multiple_low | number | multiple | No | min: 0, max: 30 | null | "3" | "Low end of industry EBITDA multiple range. Example: restaurants 2-4x, SaaS 8-15x. Profitable businesses typically valued on EBITDA." |
| industry_ebitda_multiple_high | number | multiple | No | min: 0, max: 30 | null | "5" | "High end of industry EBITDA multiple range. Higher multiples for businesses with strong margins, growth, and predictable cash flows." |

### Input Groups

**Group 1: Business Financials** (always visible)
- annual_revenue
- annual_ebitda

**Group 2: Industry Multiples** (always visible, but with helper/lookup tool)
- industry_revenue_multiple_low
- industry_revenue_multiple_high
- industry_ebitda_multiple_low
- industry_ebitda_multiple_high

**Group 3: Growth & Adjustments** (collapsed by default, Pro tier feature)
- **revenue_growth_rate** (percentage): Expected annual revenue growth rate. Used for forward valuation. Default: 0
- **ebitda_margin_target** (percentage): Target EBITDA margin if improving profitability. Default: current margin
- **discretionary_addbacks** (currency): Owner expenses to add back (personal car, family payroll, etc.). Default: 0

### Optional Inputs

**Advanced Options** (collapsed, for sophisticated users):
- **industry_category** (dropdown): Pre-fill industry multiples from database. Options: SaaS, E-commerce, Retail, Manufacturing, Services, Healthcare, etc.
- **synergy_premium** (percentage): Premium for strategic buyer vs financial buyer. Default: 0

---

## 4. Outputs

### Key Metrics (Free Tier - Always Visible)

| Output Name | Description | Format | Units |
|-------------|-------------|--------|-------|
| valuation_range_revenue_based | Valuation using revenue multiples (low and high) | "$#,##0 - $#,##0" | dollars |
| valuation_range_ebitda_based | Valuation using EBITDA multiples (low and high) | "$#,##0 - $#,##0" | dollars |
| midpoint_valuation | Average of revenue and EBITDA midpoint valuations | "$#,##0" | dollars |
| ebitda_margin | EBITDA as percentage of revenue | "#.0%" | percent |

### Advanced Metrics (Pro Tier - Gated)

| Output Name | Description | Format | Units | Pro Only |
|-------------|-------------|--------|-------|----------|
| sensitivity_to_revenue_growth | Valuation change if revenue grows 10%, 20%, 30% | "Table" | dollars | Yes |
| sensitivity_to_ebitda_margin | Valuation change if EBITDA margin improves 5%, 10% | "Table" | dollars | Yes |
| recommended_valuation_method | Revenue vs EBITDA (which is more appropriate) | "Text" | method | Yes |
| valuation_per_employee | Valuation divided by number of employees (if provided) | "$#,##0" | dollars | Yes |
| implied_exit_multiple | If current offer provided, implied multiple | "#.0x" | multiple | Yes |
| adjusted_ebitda | EBITDA after discretionary addbacks | "$#,##0" | dollars | Yes |

### Charts and Visualizations

**Chart 1: Valuation Range** (Free tier, basic; Pro tier, enhanced)
- Type: Range bar chart
- X-axis: Valuation method (Revenue-based, EBITDA-based)
- Y-axis: Valuation range (low to high)
- Purpose: Visualize valuation ranges from both methods
- Tier: Free (basic), Pro (with sensitivity scenarios)

**Chart 2: Sensitivity Analysis** (Pro tier only)
- Type: Tornado chart or table
- X-axis: Valuation change
- Y-axis: Variables (revenue growth, EBITDA margin, multiple range)
- Purpose: Show which variables have biggest impact on valuation
- Tier: Pro

**Chart 3: Industry Comparison** (Pro tier only)
- Type: Box plot or range chart
- X-axis: Industry categories
- Y-axis: Typical valuation multiples
- Purpose: Compare your business to industry benchmarks
- Tier: Pro (requires industry database)

### Output Formatting Rules

- **Currency**: $#,##0 (no decimals for large valuations, e.g., "$2,500,000" not "$2,500,000.00")
- **Multiples**: #.0x (one decimal, e.g., "4.5x" not "4.50x")
- **Percentages**: #.0% (one decimal for margins)
- **Ranges**: Always show low-high (e.g., "$2,000,000 - $3,500,000")

---

## 5. Formulas and Calculation Logic

### Formula 1: Revenue-Based Valuation

**Purpose**: Calculate business value using revenue multiples

**Equation**:
```
valuation_low_revenue = annual_revenue × industry_revenue_multiple_low
valuation_high_revenue = annual_revenue × industry_revenue_multiple_high

valuation_range_revenue_based = (valuation_low_revenue, valuation_high_revenue)
midpoint_revenue_valuation = (valuation_low_revenue + valuation_high_revenue) / 2
```

**Variables**:
- annual_revenue: Trailing 12-month revenue
- industry_revenue_multiple_low/high: Industry-specific revenue multiples
- valuation_range_revenue_based: Valuation range using revenue method

**Source/Reference**: Standard revenue multiple valuation (used for high-growth, unprofitable businesses)

**Example**: $5M revenue × 0.5-1.5x = $2.5M - $7.5M valuation range

**When to use**: Revenue multiples are most appropriate for:
- Early-stage or high-growth businesses (not yet profitable)
- SaaS and subscription businesses (recurring revenue)
- Businesses where revenue is more predictable than profit

### Formula 2: EBITDA-Based Valuation

**Purpose**: Calculate business value using EBITDA multiples

**Equation**:
```
# Adjust EBITDA for discretionary addbacks (owner expenses)
adjusted_ebitda = annual_ebitda + discretionary_addbacks

valuation_low_ebitda = adjusted_ebitda × industry_ebitda_multiple_low
valuation_high_ebitda = adjusted_ebitda × industry_ebitda_multiple_high

valuation_range_ebitda_based = (valuation_low_ebitda, valuation_high_ebitda)
midpoint_ebitda_valuation = (valuation_low_ebitda + valuation_high_ebitda) / 2
```

**Variables**:
- adjusted_ebitda: EBITDA after owner expense addbacks
- industry_ebitda_multiple_low/high: Industry-specific EBITDA multiples
- valuation_range_ebitda_based: Valuation range using EBITDA method

**Source/Reference**: Standard EBITDA multiple valuation (used for profitable, mature businesses)

**Example**: $750k EBITDA × 3-5x = $2.25M - $3.75M valuation range

**When to use**: EBITDA multiples are most appropriate for:
- Profitable businesses with stable cash flows
- Mature businesses (not high-growth)
- Traditional industries (manufacturing, services, retail)
- Businesses where profit is more important than revenue growth

### Formula 3: Midpoint Valuation (Blended)

**Purpose**: Calculate single-point valuation estimate (average of both methods)

**Equation**:
```
midpoint_revenue_valuation = (valuation_low_revenue + valuation_high_revenue) / 2
midpoint_ebitda_valuation = (valuation_low_ebitda + valuation_high_ebitda) / 2

# Blended midpoint (average of both methods)
midpoint_valuation = (midpoint_revenue_valuation + midpoint_ebitda_valuation) / 2

# Alternative: Weight based on which method is more appropriate
if ebitda_margin > 15% and annual_ebitda > 0:
    # Profitable business: weight EBITDA method more heavily
    midpoint_valuation = (midpoint_revenue_valuation × 0.3) + (midpoint_ebitda_valuation × 0.7)
else:
    # Unprofitable or low-margin business: weight revenue method more
    midpoint_valuation = (midpoint_revenue_valuation × 0.7) + (midpoint_ebitda_valuation × 0.3)
```

**Source/Reference**: Weighted average valuation methodology

### Formula 4: EBITDA Margin

**Purpose**: Calculate EBITDA margin (profitability metric)

**Equation**:
```
ebitda_margin = (annual_ebitda / annual_revenue) × 100
```

**Interpretation**:
- 20%+ = Excellent margin (SaaS, software, high-value services)
- 10-20% = Good margin (most profitable businesses)
- 5-10% = Thin margin (retail, low-margin services)
- 0-5% = Very thin margin (breakeven or barely profitable)
- Negative = Not profitable (early-stage, high-growth, or struggling)

### Formula 5: Sensitivity to Revenue Growth

**Purpose**: Calculate how valuation changes with revenue growth

**Equation**:
```
# Test revenue growth scenarios: +10%, +20%, +30%
for growth_rate in [0.10, 0.20, 0.30]:
    projected_revenue = annual_revenue × (1 + growth_rate)

    # Assume EBITDA margin stays constant (conservative)
    projected_ebitda = projected_revenue × (ebitda_margin / 100)

    # Recalculate valuations with projected financials
    revenue_based_valuation = projected_revenue × midpoint_revenue_multiple
    ebitda_based_valuation = projected_ebitda × midpoint_ebitda_multiple

    blended_valuation = (revenue_based_valuation + ebitda_based_valuation) / 2

    valuation_increase = blended_valuation - midpoint_valuation
    percent_increase = (valuation_increase / midpoint_valuation) × 100

    sensitivity_results[growth_rate] = {
        "projected_revenue": projected_revenue,
        "projected_ebitda": projected_ebitda,
        "new_valuation": blended_valuation,
        "valuation_increase": valuation_increase,
        "percent_increase": percent_increase
    }
```

**Insight**: Revenue growth has multiplicative impact on valuation (10% revenue growth → 10%+ valuation increase)

### Formula 6: Sensitivity to EBITDA Margin Improvement

**Purpose**: Calculate how valuation changes if profitability improves

**Equation**:
```
# Test margin improvement scenarios: +5%, +10% (absolute percentage points)
for margin_improvement in [5, 10]:
    new_ebitda_margin = ebitda_margin + margin_improvement
    projected_ebitda = annual_revenue × (new_ebitda_margin / 100)

    # Recalculate EBITDA-based valuation
    ebitda_based_valuation = projected_ebitda × midpoint_ebitda_multiple

    # Blended valuation (assume revenue stays constant)
    blended_valuation = (midpoint_revenue_valuation + ebitda_based_valuation) / 2

    valuation_increase = blended_valuation - midpoint_valuation
    percent_increase = (valuation_increase / midpoint_valuation) × 100

    sensitivity_results[margin_improvement] = {
        "new_ebitda_margin": new_ebitda_margin,
        "projected_ebitda": projected_ebitda,
        "new_valuation": blended_valuation,
        "valuation_increase": valuation_increase
    }
```

**Insight**: Margin improvement has significant impact on EBITDA-based valuation

### Formula 7: Recommended Valuation Method

**Purpose**: Determine whether revenue or EBITDA method is more appropriate

**Equation**:
```
# Decision logic
if annual_ebitda < 0:
    recommended_method = "Revenue-based"
    rationale = "Business is not profitable. Revenue multiples are more appropriate."

elif ebitda_margin < 10%:
    recommended_method = "Revenue-based"
    rationale = "Low EBITDA margin. Revenue multiples are more reliable for low-margin businesses."

elif ebitda_margin > 20% and revenue_growth_rate < 15%:
    recommended_method = "EBITDA-based"
    rationale = "Profitable, mature business. EBITDA multiples are more appropriate."

elif revenue_growth_rate > 30%:
    recommended_method = "Revenue-based"
    rationale = "High-growth business. Revenue multiples capture growth potential better than EBITDA."

else:
    recommended_method = "Blended (both methods)"
    rationale = "Use average of revenue and EBITDA valuations for balanced estimate."
```

**Source/Reference**: Industry best practices for valuation methodology selection

### Formula 8: Implied Exit Multiple (if offer provided)

**Purpose**: Calculate implied multiple from acquisition offer

**Equation**:
```
# If user provides current offer amount
if current_offer_amount:
    implied_revenue_multiple = current_offer_amount / annual_revenue
    implied_ebitda_multiple = current_offer_amount / annual_ebitda if annual_ebitda > 0 else "N/A"

    # Compare to industry ranges
    if implied_revenue_multiple < industry_revenue_multiple_low:
        offer_assessment = "Below range (low offer)"
    elif implied_revenue_multiple > industry_revenue_multiple_high:
        offer_assessment = "Above range (strong offer)"
    else:
        offer_assessment = "Within range (fair offer)"
```

### Step-by-Step Calculation Flow

1. **Validate inputs**: Check required fields, ensure revenue > 0, EBITDA can be negative
2. **Calculate EBITDA margin**: `(EBITDA / revenue) × 100`
3. **Adjust EBITDA** (if addbacks provided): `EBITDA + discretionary_addbacks`
4. **Calculate revenue-based valuation**: `revenue × revenue_multiple_low/high`
5. **Calculate EBITDA-based valuation**: `adjusted_EBITDA × ebitda_multiple_low/high`
6. **Calculate midpoint valuation**: Weighted average of both methods
7. **Determine recommended method**: Based on profitability and growth
8. **If Pro tier, calculate sensitivity analysis**:
   - Revenue growth scenarios (+10%, +20%, +30%)
   - EBITDA margin improvement (+5%, +10%)
9. **If offer provided, calculate implied multiples**
10. **Check warning conditions**: Negative EBITDA, unrealistic multiples, etc.
11. **Return results with version stamp**

### Edge Cases and Handling

| Edge Case | Condition | Handling | Warning/Error |
|-----------|-----------|----------|---------------|
| Negative EBITDA | annual_ebitda < 0 | EBITDA valuation = "Not applicable", use revenue only | Info: "Business is not profitable. Valuation based on revenue multiples only." |
| Zero revenue | annual_revenue = 0 | Error | Error: "Revenue cannot be zero. Enter trailing 12-month revenue." |
| EBITDA > Revenue | annual_ebitda > annual_revenue | Error | Error: "EBITDA cannot exceed revenue. Check inputs." |
| Very low margin | ebitda_margin < 5% | Calculate as normal | Warning: "Low EBITDA margin ({margin}%). Revenue multiples may be more appropriate." |
| Very high multiple | industry_ebitda_multiple_high > 15 | Calculate as normal | Warning: "EBITDA multiple above 15x is rare. Verify industry benchmarks." |

### Formula Version

**Current Version**: v1.0.0

**Version History**:
- v1.0.0: Initial implementation (revenue and EBITDA multiples, sensitivity analysis)

---

## 6. Warnings and Alerts

### Warning Definitions

| Warning Code | Condition | Message | Severity | Action |
|--------------|-----------|---------|----------|--------|
| NEGATIVE_EBITDA | annual_ebitda < 0 | "Business is not profitable (EBITDA: ${ebitda:,.0f}). Valuation based on revenue multiples only. Focus on path to profitability to increase valuation." | info | Improve profitability or highlight growth potential |
| LOW_EBITDA_MARGIN | ebitda_margin < 10% && annual_ebitda > 0 | "Low EBITDA margin ({margin}%). Revenue multiples may be more appropriate. Buyers will focus on revenue stability and growth potential." | info | Improve margins or emphasize revenue growth |
| HIGH_EBITDA_MARGIN | ebitda_margin > 30% | "Excellent EBITDA margin ({margin}%). EBITDA multiples will likely drive higher valuation. Emphasize profitability to buyers." | success | Highlight strong margins in sale process |
| UNREALISTIC_REVENUE_MULTIPLE | industry_revenue_multiple_high > 10 | "Revenue multiple above 10x is rare (except for high-growth SaaS). Verify industry benchmarks or justify with exceptional growth/IP." | warning | Verify multiples with industry data or advisor |
| UNREALISTIC_EBITDA_MULTIPLE | industry_ebitda_multiple_high > 20 | "EBITDA multiple above 20x is extremely rare. Typically only for exceptional businesses. Verify industry benchmarks." | warning | Verify multiples with M&A advisor |
| WIDE_VALUATION_RANGE | (valuation_high - valuation_low) / valuation_low > 1.0 | "Valuation range is wide ({range_percent}% spread). Narrow range by using tighter industry multiples or focus on single valuation method." | info | Consult business broker for more precise multiples |
| OFFER_BELOW_RANGE | current_offer_amount < valuation_low_revenue && current_offer_amount < valuation_low_ebitda | "Current offer (${offer:,.0f}) is below calculated valuation range. Consider negotiating higher or getting additional offers." | warning | Negotiate or shop to other buyers |
| OFFER_ABOVE_RANGE | current_offer_amount > valuation_high_revenue && current_offer_amount > valuation_high_ebitda | "Current offer (${offer:,.0f}) is above calculated valuation range. Strong offer, but verify buyer's ability to close." | success | Accept offer if strategic fit is good |

### Warning Thresholds

**Configurable Thresholds** (can be adjusted per tenant for B2B):
- **Low EBITDA margin**: 10% (varies by industry)
- **High EBITDA margin**: 30% (exceptional profitability)

**Hard-Coded Thresholds** (market standards, not configurable):
- **Unrealistic revenue multiple**: 10x (rare except SaaS/tech)
- **Unrealistic EBITDA multiple**: 20x (rare for any industry)
- **Wide range**: 100% spread (indicates high uncertainty)

---

## 7. Golden Test Scenarios

### Scenario 1: Profitable Service Business - EBITDA Valuation

**Description**: Profitable service business with healthy 15% EBITDA margin. EBITDA valuation is most appropriate. Midpoint valuation: $3.0M.

**Inputs**:
```json
{
  "annual_revenue": 5000000,
  "annual_ebitda": 750000,
  "industry_revenue_multiple_low": 0.5,
  "industry_revenue_multiple_high": 1.5,
  "industry_ebitda_multiple_low": 3,
  "industry_ebitda_multiple_high": 5,
  "discretionary_addbacks": 0
}
```

**Expected Outputs**:
```json
{
  "ebitda_margin": {
    "value": 15.0,
    "tolerance": 0.1,
    "unit": "percent",
    "note": "$750k / $5M = 15%"
  },
  "valuation_range_revenue_based": {
    "low": 2500000,
    "high": 7500000,
    "note": "$5M × 0.5-1.5x"
  },
  "valuation_range_ebitda_based": {
    "low": 2250000,
    "high": 3750000,
    "note": "$750k × 3-5x"
  },
  "midpoint_valuation": {
    "value": 3000000,
    "tolerance": 50000,
    "unit": "USD",
    "note": "Weighted toward EBITDA method (profitable business)"
  },
  "recommended_valuation_method": {
    "value": "EBITDA-based",
    "rationale": "Profitable, mature business. EBITDA multiples are more appropriate."
  },
  "sensitivity_to_revenue_growth": {
    "10%": {
      "new_valuation": 3300000,
      "valuation_increase": 300000
    },
    "20%": {
      "new_valuation": 3600000,
      "valuation_increase": 600000
    },
    "30%": {
      "new_valuation": 3900000,
      "valuation_increase": 900000
    }
  },
  "sensitivity_to_ebitda_margin": {
    "5% improvement": {
      "new_ebitda_margin": 20.0,
      "new_valuation": 3500000,
      "valuation_increase": 500000
    },
    "10% improvement": {
      "new_ebitda_margin": 25.0,
      "new_valuation": 4000000,
      "valuation_increase": 1000000
    }
  }
}
```

**Expected Warnings**: None (healthy scenario)

### Scenario 2: High-Growth SaaS - Revenue Valuation

**Description**: Unprofitable but high-growth SaaS business. Revenue multiples are appropriate (4-8x). Midpoint valuation: $18M.

**Inputs**:
```json
{
  "annual_revenue": 3000000,
  "annual_ebitda": -500000,
  "industry_revenue_multiple_low": 4,
  "industry_revenue_multiple_high": 8,
  "industry_ebitda_multiple_low": 10,
  "industry_ebitda_multiple_high": 15
}
```

**Expected Outputs**:
```json
{
  "ebitda_margin": {
    "value": -16.7,
    "tolerance": 0.1,
    "unit": "percent",
    "note": "-$500k / $3M = -16.7% (unprofitable)"
  },
  "valuation_range_revenue_based": {
    "low": 12000000,
    "high": 24000000,
    "note": "$3M × 4-8x"
  },
  "valuation_range_ebitda_based": {
    "value": "Not applicable",
    "note": "Negative EBITDA, cannot use EBITDA multiples"
  },
  "midpoint_valuation": {
    "value": 18000000,
    "tolerance": 100000,
    "unit": "USD",
    "note": "Based on revenue multiples only"
  },
  "recommended_valuation_method": {
    "value": "Revenue-based",
    "rationale": "Business is not profitable. Revenue multiples are more appropriate."
  }
}
```

**Expected Warnings**:
- NEGATIVE_EBITDA: "Business is not profitable (EBITDA: -$500,000). Valuation based on revenue multiples only..."

### Scenario 3: Retail Business - Low Margin, Revenue Valuation

**Description**: Profitable retail business but low 5% EBITDA margin. Revenue multiples more appropriate. Midpoint valuation: $1.8M.

**Inputs**:
```json
{
  "annual_revenue": 10000000,
  "annual_ebitda": 500000,
  "industry_revenue_multiple_low": 0.3,
  "industry_revenue_multiple_high": 0.8,
  "industry_ebitda_multiple_low": 2,
  "industry_ebitda_multiple_high": 4
}
```

**Expected Outputs**:
```json
{
  "ebitda_margin": {
    "value": 5.0,
    "tolerance": 0.1,
    "unit": "percent"
  },
  "valuation_range_revenue_based": {
    "low": 3000000,
    "high": 8000000,
    "note": "$10M × 0.3-0.8x"
  },
  "valuation_range_ebitda_based": {
    "low": 1000000,
    "high": 2000000,
    "note": "$500k × 2-4x"
  },
  "midpoint_valuation": {
    "value": 3750000,
    "tolerance": 100000,
    "unit": "USD",
    "note": "Weighted toward revenue method (low-margin business)"
  },
  "recommended_valuation_method": {
    "value": "Revenue-based",
    "rationale": "Low EBITDA margin. Revenue multiples are more reliable for low-margin businesses."
  }
}
```

**Expected Warnings**:
- LOW_EBITDA_MARGIN: "Low EBITDA margin (5.0%). Revenue multiples may be more appropriate..."

---

## 8. AI Narrative Strategy (AI-Enabled)

### What AI Explains for This Calculator

**Primary Explanations**:
1. **What the valuation range means and why there's a range** (market variability, buyer types, growth potential)
2. **Which valuation method is most appropriate** (revenue vs EBITDA, based on business characteristics)
3. **How to increase business value** (revenue growth, margin improvement, recurring revenue, etc.)
4. **What buyers look for** (growth, profitability, customer concentration, IP, etc.)

**Scenario Suggestions**:
- "What would my business be worth if I grew revenue 20%?" (growth impact on valuation)
- "How can I increase my valuation before selling?" (actionable strategies)
- "Is this offer fair?" (compare offer to calculated valuation)

### Prompt Template

**System Prompt**:
```
You are a business valuation advisor helping business owners understand what their business is worth and how to increase value.

Your role is to:
- Explain valuation ranges and why revenue and EBITDA methods give different results
- Interpret which valuation method is most appropriate for this business
- Suggest strategies to increase business value (revenue growth, margin improvement, recurring revenue)
- Provide context on what buyers look for (growth, profitability, scalability, customer diversification)

Output constraints:
- Length: 150-250 words
- Tone: Professional but approachable, educational
- Format: Plain paragraphs (no bullet lists)
- Disclaimers: Always end with standard disclaimer

Prohibited behaviors:
- Never guarantee specific valuation (valuations are ranges, market-dependent)
- Never recommend specific buyers or brokers
- Never use absolute language (avoid "always", "never", "must")
```

**User Prompt Template**:
```
The business has annual revenue of ${annual_revenue} and EBITDA of ${annual_ebitda} (EBITDA margin: {ebitda_margin}%). Industry multiples: revenue {revenue_multiple_low}-{revenue_multiple_high}x, EBITDA {ebitda_multiple_low}-{ebitda_multiple_high}x. Calculated valuation ranges: revenue-based ${valuation_low_revenue}-${valuation_high_revenue}, EBITDA-based ${valuation_low_ebitda}-${valuation_high_ebitda}. Midpoint valuation: ${midpoint_valuation}. Explain what this means and how to increase value.
```

**Expected AI Response** (150-250 words):
```
Your business has annual revenue of $5 million and EBITDA of $750,000 (15% margin), indicating healthy profitability. Based on industry multiples, your valuation range is approximately $2.25-$3.75 million using EBITDA multiples (3-5x) and $2.5-$7.5 million using revenue multiples (0.5-1.5x). The midpoint valuation is approximately $3 million.

For a profitable service business with 15% EBITDA margin, EBITDA multiples are more appropriate than revenue multiples. Buyers of profitable, mature businesses focus on cash flow (EBITDA) rather than revenue growth. Your $750,000 EBITDA at a 3-5x multiple range reflects typical market valuations for service businesses.

To increase your valuation, focus on improving EBITDA margin (currently 15%). If you improved margin to 20% through operational efficiency or pricing increases, EBITDA would grow to $1 million, potentially increasing valuation to $4-5 million (using the same 4x midpoint multiple). Revenue growth also helps—a 20% revenue increase to $6 million with maintained margins would increase EBITDA to $900,000 and valuation to approximately $3.6 million.

Other value drivers include recurring revenue (increases multiple), customer diversification (reduces risk), documented processes (increases scalability), and strong management team (reduces buyer dependency on owner).

This analysis is for informational purposes only and does not constitute financial, tax, or legal advice. Actual business valuations depend on market conditions, buyer pool, and detailed due diligence. Consult a business broker or M&A advisor for formal valuation.
```

---

## 9. Export Format Details

### PDF Layout

**Header**:
- "Business Valuation Analysis"
- User tier: "Free" or "Pro"
- Generation timestamp: "Generated on November 18, 2025"

**Body Sections**:
1. **Business Financials** (table):
   - Annual Revenue: $5,000,000
   - Annual EBITDA: $750,000
   - EBITDA Margin: 15.0%

2. **Industry Valuation Multiples** (table):
   - Revenue Multiples: 0.5x - 1.5x
   - EBITDA Multiples: 3x - 5x
   - Industry: Service Business

3. **Valuation Summary** (table):
   - Revenue-Based Valuation: $2,500,000 - $7,500,000
   - EBITDA-Based Valuation: $2,250,000 - $3,750,000
   - Midpoint Valuation: $3,000,000
   - Recommended Method: EBITDA-based

4. **Valuation Range Chart**:
   - Bar chart showing revenue-based and EBITDA-based ranges
   - Midpoint marked

5. **Sensitivity Analysis** (Pro tier only, table):
   - Revenue Growth +10%: Valuation $3,300,000 (+$300k)
   - Revenue Growth +20%: Valuation $3,600,000 (+$600k)
   - Revenue Growth +30%: Valuation $3,900,000 (+$900k)
   - EBITDA Margin +5%: Valuation $3,500,000 (+$500k)
   - EBITDA Margin +10%: Valuation $4,000,000 (+$1,000k)

6. **Value Drivers** (Pro tier only, if AI insights available):
   - Key strategies to increase valuation

7. **Disclaimers**:
   - "This valuation is a preliminary estimate based on industry multiples. Actual valuation depends on market conditions, buyer pool, detailed due diligence, and specific business attributes (customer concentration, growth trajectory, competitive position, etc.). Consult a business broker or M&A advisor for formal valuation."

8. **Warnings** (if any):
   - List all warnings with severity icons

**Footer**:
- Version stamp: "Generated by CFO Calculator Suite v1.2.3 | Calculator: business-valuation v1.0.0 | Formulas: v1.0.0 | 2025-11-18"
- Watermark (Free tier only): Diagonal semi-transparent "Upgrade for clean exports"

### CSV/Excel Structure

**Metadata Headers**:
```csv
# Business Valuation Analysis
# Version: v1.0.0
# Generated: 2025-11-18T21:30:00Z
# Tier: Pro
#
```

**Data Rows**:
```csv
Section,Field,Value
Business Financials,Annual Revenue,$5000000
Business Financials,Annual EBITDA,$750000
Business Financials,EBITDA Margin,15.0%
Industry Multiples,Revenue Multiple Low,0.5x
Industry Multiples,Revenue Multiple High,1.5x
Industry Multiples,EBITDA Multiple Low,3.0x
Industry Multiples,EBITDA Multiple High,5.0x
Valuation Summary,Revenue-Based Low,$2500000
Valuation Summary,Revenue-Based High,$7500000
Valuation Summary,EBITDA-Based Low,$2250000
Valuation Summary,EBITDA-Based High,$3750000
Valuation Summary,Midpoint Valuation,$3000000
Valuation Summary,Recommended Method,EBITDA-based
Sensitivity,Revenue +10%,$3300000
Sensitivity,Revenue +20%,$3600000
Sensitivity,Revenue +30%,$3900000
Sensitivity,EBITDA Margin +5%,$3500000
Sensitivity,EBITDA Margin +10%,$4000000
```

**Excel Formatting**:
- Bold section headers
- Currency cells: $#,##0 (no decimals for large numbers)
- Multiple cells: #.0x
- Percentage cells: 0.0%

---

## 10. Tier-Specific Behavior

### Free Tier

**What's Shown**:
- Single business valuation (cannot save or track over time)
- Business financials inputs (revenue, EBITDA)
- Industry multiples inputs (with helper text)
- Basic valuation ranges (revenue-based, EBITDA-based)
- Midpoint valuation
- EBITDA margin
- Basic valuation range chart

**What's Gated**:
- **Sensitivity analysis**: Locked (revenue growth, margin improvement scenarios)
- **Recommended valuation method**: Locked
- **Historical tracking**: Cannot save valuations over time
- **Industry comparison**: Locked
- **Multiple business comparison**: "Add Business" button triggers upgrade prompt
- **Clean exports**: PDF has watermark
- **AI insights**: "Get valuation insights" button triggers AI tier upgrade prompt

**Upgrade Triggers**:
- Click "See Sensitivity Analysis" → `upgrade_prompt_shown` with `trigger_reason: "locked_metric"`
- Click "Industry Comparison" → `upgrade_prompt_shown` with `trigger_reason: "industry_comparison"`
- Click "Add Business" → `upgrade_prompt_shown` with `trigger_reason: "add_scenario"`
- Click "Get insights" → `upgrade_prompt_shown` with `trigger_reason: "ai_request"`

### Pro Tier

**What Unlocks**:
- **Multiple businesses** (up to 50): Track valuation over time (quarterly/annual)
- **Sensitivity analysis**: Revenue growth and margin improvement scenarios
- **Recommended valuation method**: Logic for which method is most appropriate
- **Industry comparison**: Compare business to industry benchmark multiples
- **Valuation tracking**: Chart showing valuation trend over time
- **Business comparison table**: Side-by-side comparison of multiple businesses
- **Tornado chart**: Visualize which variables have biggest impact on valuation
- **Clean exports**: PDF with no watermark

**Still Gated** (requires AI tier):
- AI valuation insights and strategies to increase value
- AI scenario suggestions

### AI Tier

**What Unlocks**:
- **AI valuation insights**: "What is my business really worth?" gets contextual explanation
- **AI value enhancement**: "How can I increase my valuation?" gets specific strategies
- **AI offer evaluation**: "Is this a fair offer?" gets comparative analysis
- **50 AI requests per month** (hard cap)

**Usage Quota**:
- Counter displayed: "23 of 50 AI requests remaining this month"
- Resets on 1st of each month

---

## Implementation Notes

**Priority**: Medium (founder/investor interest, AI showcase, but lower SEO volume than lending calculators)

**Estimated Effort**: 1.5 weeks
- Week 1: Core valuation calculations (revenue and EBITDA multiples)
- Week 2 (half): Sensitivity analysis, industry database integration, testing

**Dependencies**:
- Industry multiples database (can start with manual input, add database later)
- Valuation methodology logic (revenue vs EBITDA decision tree)

**Related Calculators**:
- **Cash Runway**: Runway affects valuation (short runway = lower valuation)
- **Breakeven & Contribution Margin**: Unit economics affect valuation
- **Business Loan + DSCR**: Debt levels affect net enterprise value

**Database Integration** (future enhancement):
- Industry multiples database (by NAICS code or industry category)
- Automatic lookup based on industry selection
- Periodic updates from market data (BizBuySell, PitchBook, etc.)

---

## End of Calculator PDR

This calculator is complete and ready for implementation. All valuation formulas, sensitivity analysis, and recommendation logic are specified in detail.
