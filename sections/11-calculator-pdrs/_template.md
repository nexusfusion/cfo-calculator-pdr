# Calculator PDR Template

This template defines the structure that every individual calculator Product Design Review (PDR) must follow. Use this template when creating new calculators to ensure consistency, completeness, and implementation readiness.

---

## 1. Calculator Overview

**Calculator Name**: [Full display name, e.g., "Business Loan + DSCR Intelligence Calculator"]

**Calculator Slug**: [URL-safe identifier, e.g., "business-loan-dscr"]

**Primary Category**: [One of: Financing & Lending Intelligence, Cash Flow & Liquidity Intelligence, Profitability & Pricing Intelligence, Valuation & Deal Intelligence, Planning & Forecasting Intelligence]

**Secondary Category** (if applicable): [Optional second category]

**Business Role**: [One or more of the following]
- Traffic magnet (SEO-driven, high search volume)
- Pro upgrade driver (feature richness drives conversions)
- AI narrative showcase (demonstrates AI value)
- B2B demo calculator (showcases white-label potential)
- Affiliate revenue driver (high-intent users)

**Primary Success Metric**: [How success is measured for this calculator]
- Examples: "Exports per session", "Free to Pro conversion rate", "AI narrative usage rate", "Average scenarios per Pro user"

**Version**: [Current calculator version, e.g., "v1.0.0"]

**Formula Library Version**: [Current formula version, e.g., "v1.2.3"]

---

## 2. User Scenarios and Use Cases

### Who Uses This Calculator

**Primary Personas**:
- [Persona 1: e.g., "Small business owners seeking SBA loans"]
- [Persona 2: e.g., "CFOs evaluating financing options"]
- [Persona 3: e.g., "Lenders pre-qualifying loan applications"]

**Secondary Personas**:
- [Persona 4: e.g., "Business advisors assisting clients"]
- [Persona 5: e.g., "Investors analyzing deal structures"]

### When They Use It (Decision Contexts)

**Timing**:
- [Context 1: e.g., "Before applying for a loan (planning phase)"]
- [Context 2: e.g., "After receiving loan offer (comparison phase)"]
- [Context 3: e.g., "During annual budgeting (scenario testing)"]

**Frequency**:
- [e.g., "One-time use for specific decision" or "Recurring monthly use for ongoing monitoring"]

### What Questions It Answers

This calculator helps users answer:

1. [Question 1: e.g., "Can I afford this loan payment?"]
2. [Question 2: e.g., "Will my DSCR meet lender requirements?"]
3. [Question 3: e.g., "How much total interest will I pay?"]
4. [Question 4: e.g., "What if my revenue decreases by 10%?"]
5. [Question 5: e.g., "Should I choose a 10-year or 15-year term?"]

---

## 3. Inputs

### Input Field Definitions

| Field Name | Type | Units | Required | Validation | Default | Placeholder | Tooltip |
|------------|------|-------|----------|------------|---------|-------------|---------|
| [field_name_1] | [number/currency/percentage/dropdown] | [dollars/percent/months/years] | [Yes/No] | [min: X, max: Y, positive] | [value or null] | ["e.g., 250000"] | ["1-2 sentence explanation"] |
| [field_name_2] | ... | ... | ... | ... | ... | ... | ... |

**Example**:

| Field Name | Type | Units | Required | Validation | Default | Placeholder | Tooltip |
|------------|------|-------|----------|------------|---------|-------------|---------|
| loan_amount | currency | dollars | Yes | min: 1, max: 100000000 | null | "e.g., 250000" | "Total amount borrowed. Most SBA loans range from $50,000 to $5 million." |
| interest_rate | percentage | percent | Yes | min: 0, max: 30 | null | "e.g., 7.5" | "Annual interest rate for the loan. Typical SBA rates are 6-12%." |
| term_years | number | years | Yes | min: 1, max: 30 | 10 | "e.g., 10" | "Number of years to repay the loan. SBA loans typically have 10-25 year terms." |

### Input Groups (if applicable)

**Group 1: [e.g., "Loan Details"]**
- [field_name_1]
- [field_name_2]

**Group 2: [e.g., "Business Financials"]**
- [field_name_3]
- [field_name_4]

### Optional Inputs

**Advanced Options** (collapsed by default):
- [optional_field_1]: [Description and when to use]
- [optional_field_2]: [Description and when to use]

---

## 4. Outputs

### Key Metrics (Free Tier - Always Visible)

| Output Name | Description | Format | Units |
|-------------|-------------|--------|-------|
| [output_1] | [What this metric means] | [e.g., "$#,##0.00"] | [dollars/percent/months] |
| [output_2] | ... | ... | ... |

**Example**:

| Output Name | Description | Format | Units |
|-------------|-------------|--------|-------|
| monthly_payment | Principal and interest payment per month | "$#,##0.00" | dollars |
| total_interest | Total interest paid over loan term | "$#,##0.00" | dollars |
| total_amount_paid | Total principal + interest | "$#,##0.00" | dollars |

### Advanced Metrics (Pro Tier - Gated)

| Output Name | Description | Format | Units | Pro Only |
|-------------|-------------|--------|-------|----------|
| [advanced_output_1] | [What this metric means] | [format] | [units] | Yes |
| [advanced_output_2] | ... | ... | ... | Yes |

**Example**:

| Output Name | Description | Format | Units | Pro Only |
|-------------|-------------|--------|-------|----------|
| dscr | Debt Service Coverage Ratio | "#.00" | ratio | Yes |
| covenant_headroom | How much DSCR exceeds minimum 1.25 | "+#.00 or -#.00" | ratio | Yes |

### Charts and Visualizations (if any)

**Chart 1: [Chart Name]**
- Type: [Line chart / Bar chart / Pie chart / etc.]
- X-axis: [Label and values]
- Y-axis: [Label and values]
- Purpose: [What insight this chart provides]
- Tier: [Free / Pro]

**Example**:
- **Chart 1: Amortization Schedule**
  - Type: Line chart
  - X-axis: Month (0 to term in months)
  - Y-axis: Dollars (principal, interest, balance)
  - Purpose: Visualize how much of each payment goes to principal vs interest over time
  - Tier: Pro

### Output Formatting Rules

- **Currency**: $#,##0.00 (always two decimals, commas for thousands)
- **Percentages**: #.00% (two decimals)
- **Ratios**: #.00 (two decimals)
- **Months/Years**: Integer (no decimals)

---

## 5. Formulas and Calculation Logic

### Formula Definitions

**Formula 1: [Formula Name]**

**Purpose**: [What this formula calculates]

**Equation**:
```
[Mathematical formula or pseudocode]
```

**Variables**:
- [variable_1]: [Description and units]
- [variable_2]: [Description and units]

**Source/Reference**: [e.g., "Standard amortization formula, see https://en.wikipedia.org/wiki/Amortization_calculator"]

**Example**:

**Formula 1: Monthly Payment (Amortization)**

**Purpose**: Calculate fixed monthly payment for fully amortizing loan

**Equation**:
```
monthly_payment = P × [r × (1 + r)^n] / [(1 + r)^n - 1]

Where:
P = principal (loan amount)
r = monthly interest rate (annual rate / 12)
n = number of monthly payments (term in years × 12)
```

**Variables**:
- P (principal): Loan amount in dollars
- r (monthly rate): Annual interest rate divided by 12, expressed as decimal
- n (number of payments): Loan term in years multiplied by 12

**Source/Reference**: Standard amortization formula, see https://en.wikipedia.org/wiki/Amortization_calculator

### Step-by-Step Calculation Flow

1. [Step 1: e.g., "Validate all inputs"]
2. [Step 2: e.g., "Convert annual rate to monthly rate: r = annual_rate / 12"]
3. [Step 3: e.g., "Calculate monthly payment using formula above"]
4. [Step 4: e.g., "Calculate total amount paid: monthly_payment × n"]
5. [Step 5: e.g., "Calculate total interest: total_amount_paid - principal"]
6. [Step 6: e.g., "If revenue and expenses provided, calculate DSCR"]
7. [Step 7: e.g., "Check warning conditions, add warnings if triggered"]
8. [Step 8: e.g., "Return results with version stamp"]

### Edge Cases and Handling

| Edge Case | Condition | Handling | Warning/Error |
|-----------|-----------|----------|---------------|
| [Case 1] | [When this occurs] | [How to handle] | [Warning message or error] |
| [Case 2] | ... | ... | ... |

**Example**:

| Edge Case | Condition | Handling | Warning/Error |
|-----------|-----------|----------|---------------|
| Zero interest rate | interest_rate = 0 | monthly_payment = principal / n (simple division) | Info: "Zero interest rate. Payment is principal only." |
| Negative DSCR | DSCR < 0 | Calculate as normal, show negative value | Critical: "Negative DSCR. Business has negative operating income." |
| Division by zero | annual_debt_service = 0 | DSCR = Infinity | Info: "DSCR cannot be calculated with zero debt service." |

### Formula Version

**Current Version**: [e.g., "v1.2.3"]

**Version History**:
- [v1.0.0]: [Initial implementation]
- [v1.1.0]: [Added DSCR calculation]
- [v1.2.0]: [Improved rounding precision]
- [v1.2.3]: [Fixed edge case for zero interest rate]

---

## 6. Warnings and Alerts

### Warning Definitions

| Warning Code | Condition | Message | Severity | Action |
|--------------|-----------|---------|----------|--------|
| [WARNING_CODE_1] | [When to trigger] | ["Exact warning text"] | [info/warning/danger] | [What user should do] |
| [WARNING_CODE_2] | ... | ... | ... | ... |

**Example**:

| Warning Code | Condition | Message | Severity | Action |
|--------------|-----------|---------|----------|--------|
| DSCR_BELOW_MINIMUM | DSCR < 1.25 | "DSCR of [value] is below the typical lender minimum of 1.25. This loan may not be approved without additional collateral or guarantees." | warning | Consider increasing revenue, reducing expenses, or extending loan term |
| HIGH_DEBT_BURDEN | monthly_payment / monthly_income > 0.40 | "Monthly loan payment represents [percent]% of gross income. Lenders typically require this ratio to be below 40%." | warning | Reduce loan amount or extend term to lower monthly payment |
| NEGATIVE_OPERATING_INCOME | net_operating_income < 0 | "Business has negative operating income. DSCR calculation is not meaningful. Focus on achieving profitability before taking on debt." | danger | Review business model and path to profitability |

### Warning Thresholds

**Configurable Thresholds** (can be adjusted per tenant for B2B):
- [threshold_1]: [Default value, e.g., "DSCR minimum: 1.25"]
- [threshold_2]: [Default value]

**Hard-Coded Thresholds** (fixed for all users):
- [threshold_3]: [Value and rationale]

---

## 7. Golden Test Scenarios

### Scenario 1: [Scenario Name]

**Description**: [Brief description of what this scenario tests, e.g., "Typical SBA loan with healthy DSCR"]

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
  "monthly_payment": {
    "value": 2958.04,
    "tolerance": 1,
    "unit": "USD"
  },
  "total_interest": {
    "value": 104964.80,
    "tolerance": 100,
    "unit": "USD"
  },
  "total_amount_paid": {
    "value": 354964.80,
    "tolerance": 100,
    "unit": "USD"
  },
  "dscr": {
    "value": 1.42,
    "tolerance": 0.01,
    "unit": "ratio"
  }
}
```

**Expected Warnings**:
- None (healthy scenario)

### Scenario 2: [Scenario Name]

**Description**: [Brief description]

**Inputs**: [Full JSON with all input values]

**Expected Outputs**: [Full JSON with all output values, tolerances, and units]

**Expected Warnings**: [List of warning codes that should appear]

### Scenario 3: [Scenario Name] (Optional)

**Description**: [Brief description]

**Inputs**: [Full JSON]

**Expected Outputs**: [Full JSON]

**Expected Warnings**: [List of warning codes]

---

## 8. AI Narrative Strategy (If AI-Enabled)

### What AI Explains for This Calculator

**Primary Explanations**:
1. [Explanation type 1: e.g., "What the DSCR value means and whether it's acceptable for lenders"]
2. [Explanation type 2: e.g., "Why the total interest is so high and how to reduce it"]
3. [Explanation type 3: e.g., "Risks of this loan structure and mitigation strategies"]

**Scenario Suggestions**:
- [Suggestion type 1: e.g., "Alternative loan terms to test (longer term, lower amount, etc.)"]
- [Suggestion type 2: e.g., "Revenue/expense changes to improve DSCR"]

### Prompt Template

**System Prompt**:
```
You are a CFO advisor helping business owners understand [calculator purpose, e.g., "loan payments and debt service coverage"].

Your role is to:
- [Role 1: e.g., "Explain DSCR in plain language"]
- [Role 2: e.g., "Interpret whether this loan is likely to be approved"]
- [Role 3: e.g., "Suggest ways to improve DSCR if it's below threshold"]

Output constraints:
- Length: 150-250 words
- Tone: Professional but approachable
- Format: Plain paragraphs
- Disclaimers: Always end with standard disclaimer

Prohibited behaviors:
- Never guarantee loan approval
- Never recommend taking or not taking a loan (only explain metrics)
- Use hedging language: "typically", "generally", "most lenders"
```

**User Prompt Template**:
```
The business has annual revenue of [annual_revenue], operating expenses of [annual_operating_expenses], and a loan with monthly payment of [monthly_payment]. The calculated DSCR is [dscr]. Explain what this means and whether it's acceptable for lenders.
```

**Expected AI Response** (150-250 words):
```
[Example response showing appropriate tone, length, and content]
```

---

## 9. Export Format Details

### PDF Layout

**Header**:
- Calculator name
- User tier (Free/Pro)
- Generation timestamp

**Body Sections**:
1. **Inputs Summary**: Table with all user inputs
2. **Key Results**: [List key metrics shown in PDF]
3. **Advanced Results** (Pro tier only): [List advanced metrics]
4. **Charts** (if applicable): [List charts included]
5. **Warnings**: [Any warnings that were displayed]

**Footer**:
- Disclaimers (per Section 5.3)
- Version stamp: "Generated by CFO Calculator Suite v[suite_version] | Calculator: [calculator_slug] v[calc_version] | Formulas: v[formula_version] | [timestamp]"
- Watermark (Free tier only): Diagonal "Upgrade for clean exports"

### CSV/Excel Structure

**Metadata Headers** (rows 1-5):
```csv
# [Calculator Name]
# Version: v[version]
# Generated: [timestamp]
# Tier: [free/pro]
#
```

**Data Rows**:
```csv
Section,Field,Value
Inputs,[input_1_name],[input_1_value]
Inputs,[input_2_name],[input_2_value]
...
Outputs,[output_1_name],[output_1_value]
Outputs,[output_2_name],[output_2_value]
...
Warnings,[warning_1_code],[warning_1_message]
```

**Excel Formatting** (if applicable):
- Bold headers
- Currency formatting: $#,##0.00
- Percentage formatting: 0.00%
- Alternating row colors for readability

---

## 10. Tier-Specific Behavior

### Free Tier

**What's Shown**:
- [Feature 1: e.g., "Single scenario (cannot save)"]
- [Feature 2: e.g., "Key metrics (monthly payment, total interest, total amount paid)"]
- [Feature 3: e.g., "Basic warnings (DSCR below minimum)"]

**What's Gated**:
- [Gated 1: e.g., "Multiple scenarios (Add Scenario button triggers upgrade prompt)"]
- [Gated 2: e.g., "Advanced metrics (DSCR, covenant headroom - shown as locked with Pro badge)"]
- [Gated 3: e.g., "Clean exports (PDF has watermark)"]
- [Gated 4: e.g., "AI narratives (Explain button triggers upgrade prompt)"]

**Upgrade Triggers**:
- Click "Add Scenario" → upgrade_prompt_shown with trigger_reason: "add_scenario"
- Click locked advanced metric → upgrade_prompt_shown with trigger_reason: "locked_metric"
- Click "Explain this result" → upgrade_prompt_shown with trigger_reason: "ai_request"

### Pro Tier

**What Unlocks**:
- [Feature 1: e.g., "Multiple scenarios (up to 50)"]
- [Feature 2: e.g., "All advanced metrics (DSCR, covenant headroom, sensitivity analysis)"]
- [Feature 3: e.g., "Clean exports (no watermark)"]
- [Feature 4: e.g., "Scenario comparison (side-by-side view)"]

**Still Gated** (requires AI tier):
- [Gated: e.g., "AI narratives and scenario suggestions"]

### AI Tier

**What Unlocks**:
- [Feature 1: e.g., "AI explanations for all results"]
- [Feature 2: e.g., "AI scenario suggestions"]
- [Feature 3: e.g., "50 AI requests per month"]

**Usage Quota**:
- 50 requests/month (hard cap)
- Counter displayed: "[X] of 50 AI requests remaining this month"

---

## Implementation Notes

**Priority**: [High / Medium / Low]

**Estimated Effort**: [e.g., "2 weeks (1 week formula + UI, 1 week testing)"]

**Dependencies**:
- [Dependency 1: e.g., "Shared amortization formula library"]
- [Dependency 2: e.g., "DSCR calculation component"]

**Related Calculators**:
- [Related 1: e.g., "SBA 7(a) Analyzer (similar inputs, different fee structure)"]
- [Related 2: e.g., "Line of Credit Analyzer (alternative financing option)"]

---

## End of Template

Use this template for all new calculator PDRs. Ensure every section is completed with specific, detailed information before implementation begins.
