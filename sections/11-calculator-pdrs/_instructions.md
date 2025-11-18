# SECTION 11: INDIVIDUAL CALCULATOR PDRS - BUILD INSTRUCTIONS

## Overview
You are building Section 11 of the CFO Business Intelligence Calculator Suite PDR. This section contains a template for calculator PDRs plus 8 individual calculator PDRs for the MVP set.

## Context
- Reference Section 2.2 for: the 8 MVP calculators and their basic descriptions
- Reference Section 2.1 for: calculator categories
- Each calculator PDR must be detailed and complete enough to implement
- Each calculator must define 2-3 golden test scenarios

## Your Task
Create 9 markdown files:
1. `_template.md` - The template structure for all calculator PDRs
2. `business-loan-dscr.md` - Calculator 1
3. `sba-7a-analyzer.md` - Calculator 2
4. `equipment-lease-buy.md` - Calculator 3
5. `cash-runway.md` - Calculator 4
6. `breakeven-margin.md` - Calculator 5
7. `invoice-factoring.md` - Calculator 6
8. `line-of-credit.md` - Calculator 7
9. `business-valuation.md` - Calculator 8

---

## TEMPLATE FILE

**File:** `_template.md`

**Content:**
Create a complete template that defines the structure every calculator PDR must follow.

**Template sections:**

1. **Calculator Overview**
   - Calculator name and slug
   - Primary category (and secondary if applicable)
   - Business role (traffic magnet, Pro driver, AI showcase, B2B demo)
   - Primary success metric (e.g., exports per session, AI usage rate, conversion rate)

2. **User Scenarios and Use Cases**
   - Who uses this calculator (personas)
   - When they use it (decision contexts)
   - What questions it answers (3-5 specific questions)

3. **Inputs**
   - Input field definitions with:
     - Field name
     - Type (number, currency, percentage, dropdown)
     - Units (dollars, percent, months, years)
     - Required vs optional
     - Validation rules (min, max, must be positive)
     - Default value (if any)
     - Placeholder text
     - Tooltip text (1-2 sentences explaining the field)

4. **Outputs**
   - Key metrics (Free tier, always visible)
   - Advanced metrics (Pro tier, gated)
   - Charts and visualizations (if any)
   - Output formatting rules (decimals, commas, units)

5. **Formulas and Calculation Logic**
   - Formula definitions with references to sources
   - Step-by-step calculation flow
   - Edge cases and how they're handled (e.g., divide by zero, negative results)
   - Formula version and source references

6. **Warnings and Alerts**
   - Warning conditions (when to show each warning)
   - Warning messages (exact text)
   - Warning severity (info, warning, danger)
   - Warning thresholds (configurable or hard-coded values)

7. **Golden Test Scenarios**
   - Scenario 1: Name, all inputs with values, expected outputs with tolerances, expected warnings
   - Scenario 2: Name, all inputs with values, expected outputs with tolerances, expected warnings
   - Scenario 3 (optional): Name, all inputs with values, expected outputs with tolerances, expected warnings

8. **AI Narrative Strategy (If AI-Enabled)**
   - What AI explains for this calculator
   - Prompt template specific to this calculator
   - Expected AI response format and length (150-250 words)

9. **Export Format Details**
   - PDF layout (specific to this calculator, what sections appear)
   - CSV/Excel structure (columns, rows, what data included)
   - What metadata is included in exports (version, timestamp, disclaimers)

10. **Tier-Specific Behavior**
    - Free tier: what's shown, what's gated
    - Pro tier: what unlocks
    - AI tier: what AI features are available

**Format:** The template itself should be 3-4 pages with clear section headers.

---

## INDIVIDUAL CALCULATOR PDRS

For each of the 8 calculators, follow the template structure above and provide complete, detailed content.

**Calculator 1: Business Loan + DSCR Intelligence Calculator**
**File:** `business-loan-dscr.md`

- Category: Financing & Lending Intelligence
- Role: Core workhorse; Pro upgrade driver
- Inputs: loan_amount, interest_rate, term_years, annual_revenue, annual_operating_expenses
- Key outputs: monthly_payment, total_interest, total_amount_paid
- Advanced outputs: DSCR, annual_debt_service, net_operating_income, covenant_headroom
- Golden scenarios:
  - Scenario 1: Typical SBA loan for restaurant (DSCR 1.42, healthy)
  - Scenario 2: Tight DSCR for retail business (DSCR 1.18, below threshold warning)
  - Scenario 3: Strong DSCR for service business (DSCR 2.10, well above threshold)

**Calculator 2: SBA 7(a) Loan Analyzer**
**File:** `sba-7a-analyzer.md`

- Category: Financing & Lending Intelligence
- Role: SEO/traffic magnet + affiliate driver
- Inputs: loan_amount, interest_rate, term_years, sba_guarantee_fee_percent, other_fees, annual_revenue, annual_operating_expenses
- Key outputs: monthly_payment, total_interest, total_sba_fees
- Advanced outputs: DSCR, effective_rate (including fees), comparison_to_conventional_loan
- Golden scenarios:
  - Scenario 1: Standard SBA 7(a) $250k (with guarantee fee 3.75%)
  - Scenario 2: Large SBA 7(a) $1M (with guarantee fee 3.5%)

**Calculator 3: Equipment Lease vs Buy Intelligence Calculator**
**File:** `equipment-lease-buy.md`

- Category: Financing & Lending Intelligence
- Role: High-intent traffic + affiliate driver
- Inputs: equipment_cost, lease_monthly_payment, lease_term_months, loan_interest_rate (if financing), loan_term_years, tax_rate, residual_value (if lease)
- Key outputs: total_lease_cost, total_loan_cost, total_cash_cost
- Advanced outputs: NPV_lease, NPV_buy, IRR, after_tax_cost_comparison, recommended_option
- Golden scenarios:
  - Scenario 1: Lease vs finance $100k equipment (lease wins slightly)
  - Scenario 2: Lease vs cash purchase $50k (cash wins if good tax position)

**Calculator 4: Cash Runway & Burn Rate Calculator**
**File:** `cash-runway.md`

- Category: Cash Flow & Liquidity Intelligence
- Role: Founders/CFO awareness + AI showcase
- Inputs: current_cash_balance, monthly_revenue, monthly_expenses (or separate: monthly_fixed_costs, monthly_variable_costs)
- Key outputs: monthly_burn_rate, runway_months
- Advanced outputs: runway_base_case, runway_downside (revenue -20%), runway_upside (revenue +20%), breakeven_month, sensitivity_chart
- Golden scenarios:
  - Scenario 1: Startup with 15 months runway (burn $10k/month)
  - Scenario 2: Profitable business (negative burn, infinite runway)
  - Scenario 3: High-burn startup with 6 months runway (urgent funding needed)

**Calculator 5: Breakeven & Contribution Margin Analyzer**
**File:** `breakeven-margin.md`

- Category: Profitability & Pricing Intelligence
- Role: Cross-sell into non-lending use cases
- Inputs: selling_price_per_unit, variable_cost_per_unit, monthly_fixed_costs
- Key outputs: contribution_margin_per_unit, contribution_margin_percent, breakeven_units, breakeven_revenue
- Advanced outputs: margin_of_safety (at current volume), profit_at_volume (for given volume), sensitivity_to_price_changes
- Golden scenarios:
  - Scenario 1: Product with 40% margin (healthy)
  - Scenario 2: Product with 15% margin (thin, risky)
  - Scenario 3: High-volume low-margin product (5% margin, large scale)

**Calculator 6: Invoice Factoring / AR Financing Intelligence Calculator**
**File:** `invoice-factoring.md`

- Category: Financing & Lending Intelligence
- Role: Niche traffic + affiliate driver
- Inputs: invoice_amount, factoring_advance_rate (e.g., 85%), factoring_fee_percent, days_to_payment, alternative_apr (for comparison to line of credit)
- Key outputs: advance_amount, factoring_cost, effective_apr, net_proceeds
- Advanced outputs: cost_vs_line_of_credit, impact_on_cash_timing, annualized_cost
- Golden scenarios:
  - Scenario 1: Standard factoring (85% advance, 3% fee, 60 days)
  - Scenario 2: Expensive factoring (80% advance, 5% fee, 90 days)

**Calculator 7: Line of Credit Utilization & Cost Analyzer**
**File:** `line-of-credit.md`

- Category: Cash Flow & Liquidity Intelligence
- Role: Upsell/add-on for working capital users
- Inputs: credit_line_limit, average_utilization_percent, interest_rate, unused_line_fee_percent, months_utilized
- Key outputs: interest_cost, unused_line_fee_cost, total_cost, effective_cost_percent
- Advanced outputs: cost_at_different_utilization_levels, repayment_schedule_example, DSCR_impact (if revenue/expenses provided)
- Golden scenarios:
  - Scenario 1: Low utilization (30%, relatively expensive per dollar used)
  - Scenario 2: High utilization (80%, more efficient)

**Calculator 8: Simple Business Valuation (EBITDA/Revenue Multiple)**
**File:** `business-valuation.md`

- Category: Valuation & Deal Intelligence
- Role: Founders/investor interest + AI narrative driver
- Inputs: annual_revenue, annual_ebitda, industry_revenue_multiple_low, industry_revenue_multiple_high, industry_ebitda_multiple_low, industry_ebitda_multiple_high
- Key outputs: valuation_range_revenue_based, valuation_range_ebitda_based, midpoint_valuation
- Advanced outputs: sensitivity_to_revenue_growth, sensitivity_to_ebitda_margin, valuation_comparison_chart
- Golden scenarios:
  - Scenario 1: SaaS business (high revenue multiples 4-6x, EBITDA 10-15x)
  - Scenario 2: Service business (low multiples 0.5-1.5x revenue, 3-5x EBITDA)

---

## Instructions for Each Calculator PDR

For each calculator file, provide:
- Complete, detailed content for all 10 template sections
- Specific, realistic input/output values
- Exact tooltip text (helpful, plain language, 1-2 sentences)
- Exact warning messages (direct, actionable)
- Golden scenarios with full details (not just "enter typical values")
- AI prompt examples that are calculator-specific

**Each calculator PDR should be 5-7 pages.**

---

## Overall Guidelines

**Tone:** Detailed and implementation-ready. Writing for engineers building these calculators.

**Formatting:**
- Markdown headers: ##, ###, ####
- Tables for inputs and outputs
- Bullet lists for features and requirements
- Code blocks for formulas (pseudocode acceptable)
- JSON blocks for golden scenarios

**Consistency:**
- Use consistent field naming (loan_amount not loanAmount, interest_rate not rate)
- Use consistent units (always specify: dollars, percent, months, years)
- Reference Section 1.5 for tier behavior
- Reference Section 2.4 for category-specific tier patterns

**Length:** 
- Template: 3-4 pages
- Each calculator: 5-7 pages
- Total Section 11: 43-60 pages

**Output Files:**
- _template.md
- business-loan-dscr.md
- sba-7a-analyzer.md
- equipment-lease-buy.md
- cash-runway.md
- breakeven-margin.md
- invoice-factoring.md
- line-of-credit.md
- business-valuation.md

Each file starts with # header matching title.