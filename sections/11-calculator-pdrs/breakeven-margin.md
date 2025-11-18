# Breakeven & Contribution Margin Analyzer

## 1. Calculator Overview

**Calculator Name**: Breakeven & Contribution Margin Analyzer

**Calculator Slug**: `breakeven-margin`

**Primary Category**: Profitability & Pricing Intelligence

**Secondary Category**: Planning & Forecasting Intelligence

**Business Role**:
- **Cross-sell into non-lending use cases**: Expands beyond financing calculators into operational profitability
- **Pro upgrade driver**: Sensitivity analysis and margin optimization drive conversions
- **Traffic magnet**: High search volume for "breakeven calculator" and "contribution margin calculator"
- **B2B demo calculator**: White-label for business advisors, accountants, pricing consultants

**Primary Success Metric**: Export rate + scenario comparison usage (target: 30% export rate, 60% Pro users run multiple scenarios)

**Version**: v1.0.0

**Formula Library Version**: v1.1.0

---

## 2. User Scenarios and Use Cases

### Who Uses This Calculator

**Primary Personas**:
- **Small business owners** pricing products/services and determining profitability thresholds
- **Startup founders** setting pricing and understanding unit economics
- **Product managers** analyzing product line profitability and contribution margins

**Secondary Personas**:
- **Accountants and CFOs** conducting profitability analysis for clients or divisions
- **Business consultants** advising clients on pricing strategy
- **Investors** evaluating unit economics of portfolio companies (SaaS, e-commerce, etc.)

### When They Use It (Decision Contexts)

**Timing**:
- **Before launching new product**: Setting initial pricing based on cost structure
- **During pricing review**: Evaluating whether to raise or lower prices
- **After cost increases**: Recalculating breakeven when costs rise (materials, labor, etc.)
- **Budget planning**: Determining sales targets needed to cover fixed costs
- **Product portfolio review**: Comparing contribution margins across multiple products

**Frequency**:
- **One-time use** (Free tier): Single product pricing decision
- **Recurring quarterly** (Pro tier): Regular profitability reviews and pricing adjustments
- **Monthly** (Pro tier): SaaS companies and subscription businesses tracking unit economics

### What Questions It Answers

This calculator helps users answer:

1. **How many units do I need to sell to break even?** (Most common question)
2. **What is my contribution margin per unit?** (Profitability per sale)
3. **What is my contribution margin percentage?** (Margin as % of price)
4. **How much revenue do I need to cover all costs?** (Breakeven revenue)
5. **What happens if I raise prices by 10%?** (Sensitivity analysis, Pro feature)
6. **Which product is most profitable?** (Compare multiple products, Pro feature)

---

## 3. Inputs

### Input Field Definitions

| Field Name | Type | Units | Required | Validation | Default | Placeholder | Tooltip |
|------------|------|-------|----------|------------|---------|-------------|---------|
| selling_price_per_unit | currency | dollars | Yes | min: 0, max: 1000000 | null | "100" | "Selling price per unit (or per customer for services/SaaS). This is the revenue you receive per sale." |
| variable_cost_per_unit | currency | dollars | Yes | min: 0, max: 1000000 | null | "60" | "Variable cost per unit (costs that increase with each sale: materials, labor, shipping, payment processing). Exclude fixed costs." |
| monthly_fixed_costs | currency | dollars | Yes | min: 0, max: 100000000 | null | "20000" | "Total monthly fixed costs (costs that don't change with sales volume: rent, salaries, insurance, software). These must be covered to break even." |

### Input Groups

**Group 1: Unit Economics** (always visible)
- selling_price_per_unit
- variable_cost_per_unit

**Group 2: Fixed Costs** (always visible)
- monthly_fixed_costs

**Group 3: Volume & Target Analysis** (collapsed by default, Pro tier feature)
- **current_monthly_volume** (number): Current monthly sales volume (units). Required for margin of safety calculation. Default: null
- **target_monthly_volume** (number): Target monthly sales volume for profit calculation. Default: null

### Optional Inputs

**Advanced Options** (collapsed, for sophisticated analysis):
- **annual_fixed_costs** (currency): If business has significant annual costs (insurance, taxes, etc.) not spread monthly. Default: 0
- **discount_rate** (percentage): Discount or promotion rate if applicable. Default: 0
- **payment_processing_fee_percent** (percentage): Credit card or payment processing fees (reduces effective price). Default: 0

---

## 4. Outputs

### Key Metrics (Free Tier - Always Visible)

| Output Name | Description | Format | Units |
|-------------|-------------|--------|-------|
| contribution_margin_per_unit | Revenue minus variable cost per unit | "$#,##0.00" | dollars |
| contribution_margin_percent | Contribution margin as percentage of selling price | "#.0%" | percent |
| breakeven_units | Units needed to cover all fixed costs | "#,##0" | units |
| breakeven_revenue | Revenue needed to cover all fixed costs | "$#,##0.00" | dollars |

### Advanced Metrics (Pro Tier - Gated)

| Output Name | Description | Format | Units | Pro Only |
|-------------|-------------|--------|-------|----------|
| margin_of_safety | How far current sales exceed breakeven (units and %) | "#,##0 units (#%)" | mixed | Yes |
| profit_at_current_volume | Monthly profit at current sales volume | "$#,##0.00" | dollars | Yes |
| profit_at_target_volume | Monthly profit at target sales volume | "$#,##0.00" | dollars | Yes |
| effective_contribution_margin | Contribution margin after payment processing fees | "$#,##0.00" | dollars | Yes |
| operating_leverage | How sensitive profit is to volume changes | "#.00x" | ratio | Yes |
| price_sensitivity | Breakeven change if price increases/decreases 10% | "Table" | mixed | Yes |

### Charts and Visualizations

**Chart 1: Breakeven Analysis Chart** (Free tier, basic; Pro tier, enhanced)
- Type: Line chart with shaded regions
- X-axis: Volume (units sold)
- Y-axis: Dollars (revenue, total costs, profit)
- Lines: Revenue line (slope = selling price), Total cost line (fixed + variable costs), Breakeven point marked
- Purpose: Visualize breakeven point and profit/loss regions
- Tier: Free (basic), Pro (with current volume marker and target volume)

**Chart 2: Contribution Margin Waterfall** (Pro tier only)
- Type: Waterfall chart
- X-axis: Components (Selling price → Variable costs → Contribution margin → Fixed costs → Profit)
- Y-axis: Dollars
- Purpose: Show how selling price flows to profit after costs
- Tier: Pro

**Chart 3: Price Sensitivity Analysis** (Pro tier only)
- Type: Table or bar chart
- X-axis: Price change (-20%, -10%, 0%, +10%, +20%)
- Y-axis: Breakeven units, breakeven revenue, contribution margin %
- Purpose: Show how breakeven changes with different pricing
- Tier: Pro

### Output Formatting Rules

- **Currency**: $#,##0.00 (two decimals for dollars)
- **Percentages**: #.0% (one decimal for margin %)
- **Units**: #,##0 (no decimals, commas for thousands)
- **Margin of Safety**: Show both absolute (units) and percentage (e.g., "500 units (25%)")

---

## 5. Formulas and Calculation Logic

### Formula 1: Contribution Margin per Unit

**Purpose**: Calculate profit contribution per unit sold (before fixed costs)

**Equation**:
```
contribution_margin_per_unit = selling_price_per_unit - variable_cost_per_unit
```

**Variables**:
- selling_price_per_unit: Revenue per unit sold
- variable_cost_per_unit: Variable costs incurred per unit (materials, labor, shipping, etc.)
- contribution_margin_per_unit: Gross profit per unit available to cover fixed costs

**Source/Reference**: Standard contribution margin formula (managerial accounting)

### Formula 2: Contribution Margin Percentage

**Purpose**: Calculate contribution margin as percentage of selling price

**Equation**:
```
contribution_margin_percent = (contribution_margin_per_unit / selling_price_per_unit) × 100
```

**Interpretation**:
- 70%+ = Excellent margin (software, digital products, high-value services)
- 40-70% = Good margin (most services, some manufactured goods)
- 20-40% = Thin margin (retail, commodities)
- <20% = Very thin margin (low-margin, high-volume businesses)

**Source/Reference**: Standard gross margin percentage formula

### Formula 3: Breakeven Units

**Purpose**: Calculate units needed to cover all fixed costs

**Equation**:
```
breakeven_units = monthly_fixed_costs / contribution_margin_per_unit

# Round up to next whole unit (can't sell fractional units)
breakeven_units = ceil(breakeven_units)
```

**Variables**:
- monthly_fixed_costs: Total fixed costs per month
- contribution_margin_per_unit: Profit per unit to cover fixed costs
- breakeven_units: Minimum units to sell to avoid loss

**Source/Reference**: Standard breakeven analysis formula

**Interpretation**: Below breakeven = loss, at breakeven = zero profit, above breakeven = profit

### Formula 4: Breakeven Revenue

**Purpose**: Calculate revenue needed to cover all fixed costs

**Equation**:
```
breakeven_revenue = breakeven_units × selling_price_per_unit

# Alternative formula (using contribution margin %)
breakeven_revenue = monthly_fixed_costs / (contribution_margin_percent / 100)
```

**Source/Reference**: Standard breakeven revenue formula

### Formula 5: Margin of Safety

**Purpose**: Calculate cushion between current sales and breakeven (risk buffer)

**Equation**:
```
# Requires current_monthly_volume input
margin_of_safety_units = current_monthly_volume - breakeven_units
margin_of_safety_percent = (margin_of_safety_units / current_monthly_volume) × 100

# Interpretation
if margin_of_safety_percent > 40%:
    status = "Healthy cushion"
elif margin_of_safety_percent > 20%:
    status = "Moderate cushion"
elif margin_of_safety_percent > 0:
    status = "Thin cushion (risky)"
else:
    status = "Below breakeven (loss)"
```

**Source/Reference**: Margin of safety concept from financial planning & analysis

**Example**: Current volume = 1,000 units, breakeven = 600 units → Margin of safety = 400 units (40%)

### Formula 6: Profit at Volume

**Purpose**: Calculate monthly profit at given sales volume

**Equation**:
```
profit_at_volume = (contribution_margin_per_unit × volume) - monthly_fixed_costs

# At current volume
profit_at_current_volume = (contribution_margin_per_unit × current_monthly_volume) - monthly_fixed_costs

# At target volume
profit_at_target_volume = (contribution_margin_per_unit × target_monthly_volume) - monthly_fixed_costs
```

**Interpretation**:
- Positive = Profitable (above breakeven)
- Zero = Breakeven
- Negative = Loss (below breakeven)

### Formula 7: Price Sensitivity Analysis

**Purpose**: Show how breakeven changes with different pricing

**Equation**:
```
# Test price changes: -20%, -10%, 0%, +10%, +20%
for price_change in [-0.20, -0.10, 0, 0.10, 0.20]:
    adjusted_price = selling_price_per_unit × (1 + price_change)
    adjusted_contribution_margin = adjusted_price - variable_cost_per_unit
    adjusted_contribution_margin_percent = (adjusted_contribution_margin / adjusted_price) × 100
    adjusted_breakeven_units = monthly_fixed_costs / adjusted_contribution_margin
    adjusted_breakeven_revenue = adjusted_breakeven_units × adjusted_price

    # Store results for comparison table
    sensitivity_results[price_change] = {
        "price": adjusted_price,
        "contribution_margin": adjusted_contribution_margin,
        "contribution_margin_percent": adjusted_contribution_margin_percent,
        "breakeven_units": adjusted_breakeven_units,
        "breakeven_revenue": adjusted_breakeven_revenue
    }
```

**Insight**: Higher prices → higher margin % → lower breakeven units (but may reduce demand)

### Formula 8: Operating Leverage

**Purpose**: Measure how sensitive profit is to volume changes

**Equation**:
```
# Operating leverage = Contribution Margin / Operating Income
# Higher leverage = small volume changes create large profit changes

if profit_at_current_volume > 0:
    total_contribution_margin = contribution_margin_per_unit × current_monthly_volume
    operating_leverage = total_contribution_margin / profit_at_current_volume
else:
    operating_leverage = "N/A (not profitable)"
```

**Interpretation**:
- Operating leverage = 2.0x means 10% increase in sales → 20% increase in profit
- High leverage (>5x) = High risk/high reward (small volume swings create large profit swings)
- Low leverage (<2x) = More stable, less sensitive to volume

**Source/Reference**: Degree of operating leverage (DOL) formula from financial analysis

### Step-by-Step Calculation Flow

1. **Validate inputs**: Check required fields, ensure selling price > variable cost
2. **Calculate contribution margin per unit**: `selling_price - variable_cost`
3. **Calculate contribution margin percentage**: `(contribution_margin / selling_price) × 100`
4. **Calculate breakeven units**: `fixed_costs / contribution_margin`
5. **Calculate breakeven revenue**: `breakeven_units × selling_price`
6. **If current volume provided** (Pro tier):
   - Calculate margin of safety: `current_volume - breakeven_units`
   - Calculate profit at current volume: `(contribution_margin × current_volume) - fixed_costs`
7. **If target volume provided** (Pro tier):
   - Calculate profit at target volume
8. **Calculate price sensitivity** (Pro tier): Test ±10%, ±20% price changes
9. **Calculate operating leverage** (Pro tier): If profitable
10. **Check warning conditions**: Low margin, high breakeven, below breakeven, etc.
11. **Return results with version stamp**

### Edge Cases and Handling

| Edge Case | Condition | Handling | Warning/Error |
|-----------|-----------|----------|---------------|
| Price = Variable cost | selling_price = variable_cost | Contribution margin = 0, breakeven = Infinity | Error: "Contribution margin is zero. Selling price must exceed variable cost." |
| Price < Variable cost | selling_price < variable_cost | Negative contribution margin | Error: "Negative contribution margin. Each sale loses money. Increase price or reduce variable cost." |
| Zero fixed costs | monthly_fixed_costs = 0 | Breakeven = 0 units | Info: "No fixed costs. Breakeven is zero units. All sales are profitable." |
| Very low margin | contribution_margin_percent < 10% | Calculate as normal | Warning: "Very low contribution margin ({percent}%). Small price or cost changes have large impact on profitability." |
| Very high breakeven | breakeven_units > 10000 | Calculate as normal | Warning: "High breakeven volume ({units} units). Consider reducing fixed costs or improving margins." |

### Formula Version

**Current Version**: v1.1.0

**Version History**:
- v1.0.0: Initial implementation (contribution margin, breakeven)
- v1.1.0: Added price sensitivity analysis and operating leverage

---

## 6. Warnings and Alerts

### Warning Definitions

| Warning Code | Condition | Message | Severity | Action |
|--------------|-----------|---------|----------|--------|
| LOW_CONTRIBUTION_MARGIN | contribution_margin_percent < 20% | "Low contribution margin of {percent}%. Small cost increases or price decreases will significantly impact profitability. Consider pricing or cost reduction strategies." | warning | Increase prices, reduce variable costs, or focus on higher-margin products |
| VERY_LOW_MARGIN | contribution_margin_percent < 10% | "Very low contribution margin of {percent}%. Business model may not be sustainable. Each sale contributes little to covering fixed costs." | danger | Urgent: revise pricing strategy or reduce costs |
| HIGH_BREAKEVEN | breakeven_units > current_monthly_volume × 2 | "Breakeven volume ({breakeven_units} units) is {multiple}x your current volume. Significant sales growth needed to reach profitability." | warning | Reduce fixed costs, improve margins, or increase sales volume |
| BELOW_BREAKEVEN | current_monthly_volume < breakeven_units | "Current volume ({current_volume} units) is below breakeven ({breakeven_units} units). Business is operating at a loss of ${loss:,.0f} per month." | danger | Increase sales, reduce costs, or improve pricing immediately |
| THIN_MARGIN_OF_SAFETY | margin_of_safety_percent < 20% && margin_of_safety_percent > 0 | "Margin of safety is {percent}%. Small sales decline (20%) would put business below breakeven. Build buffer by growing sales or reducing costs." | warning | Increase sales or reduce costs to build cushion |
| HEALTHY_MARGIN | contribution_margin_percent >= 50% | "Excellent contribution margin of {percent}%. Strong unit economics provide cushion for growth investments and cost increases." | success | Maintain margin discipline as you scale |
| ZERO_MARGIN | contribution_margin_per_unit == 0 | "Zero contribution margin. Selling price equals variable cost. Business cannot cover fixed costs. Adjust pricing immediately." | danger | Increase prices or reduce variable costs urgently |

### Warning Thresholds

**Configurable Thresholds** (can be adjusted per tenant for B2B):
- **Low contribution margin**: 20% (varies by industry: SaaS 70%+, retail 20-30%, manufacturing 30-50%)
- **Very low margin**: 10% (universally concerning)
- **Thin margin of safety**: 20% (businesses prefer 30-50% cushion)

**Hard-Coded Thresholds** (mathematical, not configurable):
- **Zero margin**: 0% (mathematical impossibility to break even)
- **Negative margin**: <0% (losing money on each sale)

---

## 7. Golden Test Scenarios

### Scenario 1: Healthy SaaS Business - High Margin

**Description**: SaaS company with excellent 70% contribution margin. Very healthy unit economics.

**Inputs**:
```json
{
  "selling_price_per_unit": 100,
  "variable_cost_per_unit": 30,
  "monthly_fixed_costs": 50000,
  "current_monthly_volume": 1200
}
```

**Expected Outputs**:
```json
{
  "contribution_margin_per_unit": {
    "value": 70,
    "tolerance": 0,
    "unit": "USD",
    "note": "$100 price - $30 variable cost = $70"
  },
  "contribution_margin_percent": {
    "value": 70.0,
    "tolerance": 0.1,
    "unit": "percent",
    "note": "$70 / $100 = 70%"
  },
  "breakeven_units": {
    "value": 715,
    "tolerance": 1,
    "unit": "units",
    "note": "$50,000 / $70 = 714.3, rounded up to 715"
  },
  "breakeven_revenue": {
    "value": 71500,
    "tolerance": 100,
    "unit": "USD",
    "note": "715 units × $100 = $71,500"
  },
  "margin_of_safety": {
    "units": 485,
    "percent": 40.4,
    "tolerance": 1,
    "note": "1,200 current - 715 breakeven = 485 units (40.4%)"
  },
  "profit_at_current_volume": {
    "value": 34000,
    "tolerance": 10,
    "unit": "USD",
    "note": "(1,200 × $70) - $50,000 = $34,000"
  },
  "operating_leverage": {
    "value": 2.47,
    "tolerance": 0.1,
    "unit": "ratio",
    "note": "Total contribution $84k / Profit $34k = 2.47x"
  }
}
```

**Expected Warnings**:
- HEALTHY_MARGIN: "Excellent contribution margin of 70.0%. Strong unit economics..."

### Scenario 2: Thin Margin Retail Business - Low Margin, High Volume

**Description**: Retail business with thin 15% margin. High breakeven volume required.

**Inputs**:
```json
{
  "selling_price_per_unit": 50,
  "variable_cost_per_unit": 42.50,
  "monthly_fixed_costs": 30000,
  "current_monthly_volume": 5000
}
```

**Expected Outputs**:
```json
{
  "contribution_margin_per_unit": {
    "value": 7.50,
    "tolerance": 0,
    "unit": "USD"
  },
  "contribution_margin_percent": {
    "value": 15.0,
    "tolerance": 0.1,
    "unit": "percent"
  },
  "breakeven_units": {
    "value": 4000,
    "tolerance": 1,
    "unit": "units",
    "note": "$30,000 / $7.50 = 4,000 units"
  },
  "breakeven_revenue": {
    "value": 200000,
    "tolerance": 100,
    "unit": "USD",
    "note": "4,000 units × $50 = $200,000"
  },
  "margin_of_safety": {
    "units": 1000,
    "percent": 20.0,
    "tolerance": 1,
    "note": "5,000 current - 4,000 breakeven = 1,000 units (20%)"
  },
  "profit_at_current_volume": {
    "value": 7500,
    "tolerance": 10,
    "unit": "USD",
    "note": "(5,000 × $7.50) - $30,000 = $7,500"
  },
  "operating_leverage": {
    "value": 5.0,
    "tolerance": 0.1,
    "unit": "ratio",
    "note": "High leverage due to thin margins (contribution $37.5k / profit $7.5k)"
  }
}
```

**Expected Warnings**:
- LOW_CONTRIBUTION_MARGIN: "Low contribution margin of 15.0%. Small cost increases or price decreases will significantly impact profitability..."
- THIN_MARGIN_OF_SAFETY: "Margin of safety is 20.0%. Small sales decline (20%) would put business below breakeven..."

### Scenario 3: Loss-Making Business - Below Breakeven

**Description**: Business operating below breakeven. Current volume insufficient to cover fixed costs.

**Inputs**:
```json
{
  "selling_price_per_unit": 80,
  "variable_cost_per_unit": 50,
  "monthly_fixed_costs": 40000,
  "current_monthly_volume": 800
}
```

**Expected Outputs**:
```json
{
  "contribution_margin_per_unit": {
    "value": 30,
    "tolerance": 0,
    "unit": "USD"
  },
  "contribution_margin_percent": {
    "value": 37.5,
    "tolerance": 0.1,
    "unit": "percent"
  },
  "breakeven_units": {
    "value": 1334,
    "tolerance": 1,
    "unit": "units",
    "note": "$40,000 / $30 = 1,333.3, rounded up to 1,334"
  },
  "breakeven_revenue": {
    "value": 106720,
    "tolerance": 100,
    "unit": "USD"
  },
  "margin_of_safety": {
    "units": -534,
    "percent": -66.8,
    "tolerance": 1,
    "note": "800 current - 1,334 breakeven = -534 units (negative = below breakeven)"
  },
  "profit_at_current_volume": {
    "value": -16000,
    "tolerance": 10,
    "unit": "USD",
    "note": "(800 × $30) - $40,000 = -$16,000 (loss)"
  },
  "operating_leverage": {
    "value": "N/A",
    "note": "Not profitable, operating leverage not meaningful"
  }
}
```

**Expected Warnings**:
- BELOW_BREAKEVEN: "Current volume (800 units) is below breakeven (1,334 units). Business is operating at a loss of $16,000 per month."

---

## 8. AI Narrative Strategy (AI-Enabled)

### What AI Explains for This Calculator

**Primary Explanations**:
1. **What the contribution margin means and whether it's healthy** (industry context: SaaS vs retail vs manufacturing)
2. **How to interpret breakeven** (how many sales needed, how close current volume is)
3. **Margin of safety interpretation** (risk buffer, what happens if sales decline)
4. **Pricing strategy insights** (should you raise prices? Trade-off between margin and volume)

**Scenario Suggestions**:
- "What if I raise prices by 10%?" (higher margin, lower breakeven, but may reduce volume)
- "What if I reduce variable costs by 15%?" (improve margin without changing price)
- "What volume do I need to reach $50k monthly profit?" (reverse calculation)

### Prompt Template

**System Prompt**:
```
You are a CFO advisor helping business owners understand contribution margins, breakeven analysis, and unit economics.

Your role is to:
- Explain contribution margin in plain language and interpret whether it's healthy for this business type
- Interpret breakeven analysis and margin of safety (risk level)
- Suggest pricing or cost strategies to improve profitability
- Provide industry context (SaaS vs retail vs manufacturing have different margin profiles)

Output constraints:
- Length: 150-250 words
- Tone: Professional but approachable, practical and actionable
- Format: Plain paragraphs (no bullet lists)
- Disclaimers: Always end with standard disclaimer

Prohibited behaviors:
- Never guarantee specific profit outcomes
- Never recommend specific pricing without understanding market dynamics
- Never use absolute language (avoid "always", "never", "must")
```

**User Prompt Template**:
```
The business sells a product/service at ${selling_price_per_unit} with variable cost of ${variable_cost_per_unit}. Monthly fixed costs are ${monthly_fixed_costs}. Contribution margin is ${contribution_margin_per_unit} ({contribution_margin_percent}%). Breakeven is {breakeven_units} units ({breakeven_revenue}). Current volume is {current_monthly_volume} units with margin of safety of {margin_of_safety_percent}%. Explain what this means and suggest improvements.
```

**Expected AI Response** (150-250 words):
```
With a selling price of $100 and variable cost of $30, your contribution margin is $70 per unit (70%). This is excellent—each sale contributes $70 toward covering your $50,000 in monthly fixed costs. This is typical of software and high-value services where variable costs are low.

Your breakeven point is 715 units ($71,500 in revenue). At your current volume of 1,200 units, you're comfortably above breakeven with a margin of safety of 40%. This means sales could drop 40% (to 720 units) before you'd hit breakeven. That's a healthy cushion.

Current monthly profit is $34,000. The strong 70% margin gives you flexibility—you could invest in marketing to grow volume, or you have room to offer discounts without jeopardizing profitability.

Your operating leverage is 2.47x, meaning a 10% increase in sales volume would increase profit by approximately 25%. This high leverage is advantageous in growth mode but means profit is sensitive to volume swings.

To further improve profitability, focus on volume growth rather than price increases (your margin is already strong). Even a 20% volume increase to 1,440 units would add $16,800 in monthly profit.

This analysis is for informational purposes only and does not constitute financial, legal, or professional advice. Consult qualified professionals for decisions affecting your business.
```

---

## 9. Export Format Details

### PDF Layout

**Header**:
- "Breakeven & Contribution Margin Analysis"
- User tier: "Free" or "Pro"
- Generation timestamp: "Generated on November 18, 2025"

**Body Sections**:
1. **Unit Economics** (table):
   - Selling Price per Unit: $100.00
   - Variable Cost per Unit: $30.00
   - Contribution Margin per Unit: $70.00
   - Contribution Margin %: 70.0%

2. **Fixed Costs** (table):
   - Monthly Fixed Costs: $50,000

3. **Breakeven Analysis** (table):
   - Breakeven Units: 715 units
   - Breakeven Revenue: $71,500
   - Current Volume: 1,200 units
   - Status: Above Breakeven

4. **Breakeven Chart**:
   - Line chart showing revenue and cost lines, breakeven point marked
   - Free tier: Basic chart
   - Pro tier: Enhanced with current volume marker and shaded profit/loss regions

5. **Profitability Analysis** (Pro tier only, table):
   - Profit at Current Volume: $34,000
   - Margin of Safety: 485 units (40.4%)
   - Operating Leverage: 2.47x

6. **Price Sensitivity Analysis** (Pro tier only, table):
   - Price -20%: Breakeven X units, Margin Y%
   - Price -10%: Breakeven X units, Margin Y%
   - Current Price: Breakeven 715 units, Margin 70%
   - Price +10%: Breakeven X units, Margin Y%
   - Price +20%: Breakeven X units, Margin Y%

7. **Contribution Margin Waterfall** (Pro tier only):
   - Waterfall chart showing: Price → Variable costs → Contribution margin → Fixed costs → Profit

8. **Warnings** (if any):
   - List all warnings with severity icons

**Footer**:
- Disclaimers: "This analysis is for informational purposes only and does not constitute financial or pricing advice. Actual results may vary based on market conditions. Consult qualified professionals for decisions affecting your business."
- Version stamp: "Generated by CFO Calculator Suite v1.2.3 | Calculator: breakeven-margin v1.0.0 | Formulas: v1.1.0 | 2025-11-18"
- Watermark (Free tier only): Diagonal semi-transparent "Upgrade for clean exports"

### CSV/Excel Structure

**Metadata Headers**:
```csv
# Breakeven & Contribution Margin Analysis
# Version: v1.0.0
# Generated: 2025-11-18T21:30:00Z
# Tier: Pro
#
```

**Data Rows**:
```csv
Section,Field,Value
Unit Economics,Selling Price,$100.00
Unit Economics,Variable Cost,$30.00
Unit Economics,Contribution Margin,$70.00
Unit Economics,Contribution Margin %,70.0%
Fixed Costs,Monthly Fixed Costs,$50000
Breakeven Analysis,Breakeven Units,715
Breakeven Analysis,Breakeven Revenue,$71500
Breakeven Analysis,Current Volume,1200
Breakeven Analysis,Status,Above Breakeven
Profitability,Profit at Current Volume,$34000
Profitability,Margin of Safety,485 units (40.4%)
Profitability,Operating Leverage,2.47x
Price Sensitivity,-20% Price,Breakeven 893 units
Price Sensitivity,-10% Price,Breakeven 784 units
Price Sensitivity,Current Price,Breakeven 715 units
Price Sensitivity,+10% Price,Breakeven 658 units
Price Sensitivity,+20% Price,Breakeven 610 units
```

**Excel Formatting**:
- Bold section headers
- Currency cells: $#,##0.00
- Percentage cells: 0.0%
- Conditional formatting:
  - Margin of safety > 30%: Green
  - Margin of safety 10-30%: Yellow
  - Margin of safety < 10%: Red

---

## 10. Tier-Specific Behavior

### Free Tier

**What's Shown**:
- Single product analysis (cannot compare multiple products)
- Unit economics inputs (price, variable cost, fixed cost)
- Basic metrics: Contribution margin, breakeven units, breakeven revenue
- Basic breakeven chart (revenue and cost lines)

**What's Gated**:
- **Margin of safety**: Locked (requires current volume input)
- **Profit at volume**: Locked
- **Price sensitivity analysis**: Preview with blur + "Pro" badge
- **Operating leverage**: Locked
- **Multiple product comparison**: "Add Product" button triggers upgrade prompt
- **Clean exports**: PDF has watermark
- **AI insights**: "Get pricing recommendations" button triggers AI tier upgrade prompt

**Upgrade Triggers**:
- Click "Calculate Margin of Safety" → `upgrade_prompt_shown` with `trigger_reason: "locked_metric"`
- Click "Price Sensitivity Analysis" → `upgrade_prompt_shown` with `trigger_reason: "sensitivity_analysis"`
- Click "Add Product" → `upgrade_prompt_shown` with `trigger_reason: "add_scenario"`
- Click "Get pricing recommendations" → `upgrade_prompt_shown` with `trigger_reason: "ai_request"`

### Pro Tier

**What Unlocks**:
- **Multiple products** (up to 50): Compare contribution margins across product lines
- **Margin of safety**: Calculate cushion above breakeven
- **Profit calculations**: Profit at current volume, profit at target volume
- **Price sensitivity analysis**: See how breakeven changes with ±10%, ±20% price changes
- **Operating leverage**: Understand profit sensitivity to volume changes
- **Product comparison table**: Side-by-side comparison of multiple products
- **Contribution margin waterfall chart**: Visual breakdown from price to profit
- **Clean exports**: PDF with no watermark

**Still Gated** (requires AI tier):
- AI pricing recommendations
- AI scenario suggestions

### AI Tier

**What Unlocks**:
- **AI margin insights**: "Is my margin healthy?" gets industry-specific answer
- **AI pricing recommendations**: "Should I raise prices?" gets trade-off analysis
- **AI volume targets**: "What volume do I need for $50k profit?" gets detailed answer
- **50 AI requests per month** (hard cap)

**Usage Quota**:
- Counter displayed: "23 of 50 AI requests remaining this month"
- Resets on 1st of each month

---

## Implementation Notes

**Priority**: Medium-High (expands beyond lending, cross-sell opportunity)

**Estimated Effort**: 1.5 weeks
- Week 1: Core contribution margin and breakeven calculations
- Week 2 (half): Price sensitivity analysis, operating leverage, testing

**Dependencies**:
- None (standalone calculator, no shared formulas needed)

**Related Calculators**:
- **Cash Runway**: Similar breakeven concept (when does cash hit zero?)
- **Business Valuation**: Unit economics affect valuation
- **Invoice Factoring**: Margin affects ability to factor invoices

**White-Label Potential**:
- High potential for accountants, business advisors, pricing consultants
- Customizable industry benchmarks (SaaS 70%+, retail 20-30%, manufacturing 30-50%)

---

## End of Calculator PDR

This calculator is complete and ready for implementation. All contribution margin, breakeven, and sensitivity formulas are specified in detail.
