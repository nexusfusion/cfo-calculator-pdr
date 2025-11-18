# SECTION 12: JSON ASSEMBLY LINE SYSTEM - BUILD INSTRUCTIONS

## Overview
You are building Section 12 of the CFO Business Intelligence Calculator Suite PDR. This section defines the JSON-based assembly line system that enables creating 450+ calculators from simple JSON configuration files.

## Context
- Reference Section 3 for: shared formula library, module boundaries
- Reference Section 11 for: calculator structure (inputs, outputs, formulas, warnings)
- Goal: Define a JSON file → working calculator in 5-10 minutes
- Target: Scale from 8 calculators to 450+ calculators

## Your Task
Create 5 markdown files:
1. `12.1-assembly-line-architecture.md`
2. `12.2-json-schema-specification.md`
3. `12.3-formula-library-api.md`
4. `12.4-deployment-pipeline.md`
5. `12.5-calculator-examples.md`

---

## 12.1 ASSEMBLY LINE ARCHITECTURE

**File:** `12.1-assembly-line-architecture.md`

**Content:**
Define the end-to-end architecture of the assembly line system.

**System components:**

1. **JSON Configuration Files**
   - Location: /calculators/configs/{calculator-slug}.json
   - One JSON file = one calculator
   - Version controlled in Git
   - Human-readable and editable

2. **Calculator Engine (Processor)**
   - Reads JSON config
   - Validates schema
   - Generates calculator UI components dynamically
   - Wires up formula library calls
   - Applies tier gating rules
   - Creates export templates

3. **Formula Library**
   - Shared, versioned math functions
   - Called by calculator configs via function names
   - Examples: calculateLoanPayment(), calculateDSCR(), calculateNPV()
   - Each formula has input/output contract

4. **UI Generator**
   - Takes JSON inputs section → generates React input components
   - Takes JSON outputs section → generates result cards
   - Takes JSON warnings → generates alert components
   - Applies design system from Section 4 automatically

5. **Validation Engine**
   - Validates JSON against schema before deployment
   - Validates calculator outputs against golden scenarios
   - Catches errors before going live

6. **Deployment System**
   - JSON file committed to Git
   - CI/CD pipeline validates and deploys
   - Calculator goes live automatically
   - WordPress shortcode auto-generated

**Data flow:**
```
1. Create JSON config → 
2. Validate schema → 
3. Generate UI components → 
4. Wire formula library → 
5. Apply tier gating → 
6. Generate exports → 
7. Deploy to production → 
8. Auto-generate WordPress shortcode
```

**Time to deploy: 5-10 minutes** (from JSON creation to live calculator)

**Format:** Section per component, include data flow diagram (text-based). 4-5 pages.

---

## 12.2 JSON SCHEMA SPECIFICATION

**File:** `12.2-json-schema-specification.md`

**Content:**
Define the complete JSON schema for calculator configurations.

**Root-level JSON structure:**
```json
{
  "calculator_meta": {...},
  "inputs": [...],
  "calculations": [...],
  "outputs": {...},
  "warnings": [...],
  "tier_config": {...},
  "export_config": {...},
  "ai_config": {...}
}
```

**Define each section in detail:**

1. **calculator_meta**
   - calculator_slug (string, unique identifier)
   - calculator_name (string, display name)
   - calculator_version (string, semantic version)
   - category (enum: financing-lending, cash-flow, profitability, valuation, planning)
   - description (string, 1-2 sentences)
   - business_role (enum: traffic_magnet, pro_driver, ai_showcase, b2b_demo)
   - success_metric (string)

2. **inputs** (array of input field objects)
   Each input:
   - field_id (string, unique)
   - field_name (string, display label)
   - field_type (enum: number, currency, percentage, dropdown, slider)
   - units (string: dollars, percent, months, years, etc.)
   - required (boolean)
   - default_value (number or null)
   - placeholder (string)
   - tooltip (string, 1-2 sentences)
   - validation (object):
     - min (number or null)
     - max (number or null)
     - must_be_positive (boolean)
     - custom_rule (string, formula expression)

3. **calculations** (array of calculation steps)
   Each calculation:
   - step_id (string, unique)
   - formula_function (string, name of formula library function)
   - inputs (array of field_ids or previous step_ids)
   - output_variable (string, name to store result)
   - notes (string, explains what this calculates)

4. **outputs** (object with key_metrics and advanced_metrics)
   - key_metrics (array, Free tier visible)
   - advanced_metrics (array, Pro tier gated)
   Each metric:
   - metric_id (string)
   - metric_name (string, display label)
   - variable (string, references calculation output_variable)
   - format (enum: currency, percentage, number, ratio)
   - decimals (number, default 2)
   - tooltip (string)

5. **warnings** (array of warning conditions)
   Each warning:
   - warning_id (string)
   - condition (string, formula expression like "dscr < 1.25")
   - severity (enum: info, warning, danger)
   - message (string, plain language warning text)

6. **tier_config**
   - free_tier (object):
     - max_scenarios (number, typically 1)
     - visible_outputs (array of metric_ids)
   - pro_tier (object):
     - max_scenarios (number, typically 50)
     - visible_outputs (array, typically all)
   - ai_enabled (boolean)

7. **export_config**
   - pdf_sections (array defining PDF layout)
   - csv_columns (array defining CSV structure)
   - include_charts (boolean)

8. **ai_config** (optional)
   - ai_prompt_template (string, calculator-specific prompt)
   - ai_response_length (number, target word count)

**Include:** Full JSON schema with types and validation rules, plus 2-3 complete example JSON files.

**Format:** Section per JSON section, with schema definitions and examples. 6-8 pages.

---

## 12.3 FORMULA LIBRARY API

**File:** `12.3-formula-library-api.md`

**Content:**
Define the formula library functions that calculators can call.

**Formula library structure:**
- Each formula is a pure function (no side effects)
- Each formula has strict input/output contract
- Formulas are versioned (formula library v1.0.0, v1.1.0, etc.)
- Formulas are tested with golden scenarios

**Core formula categories:**

1. **Loan & Debt Formulas**
   - calculateMonthlyPayment(principal, annual_rate, term_months)
   - calculateTotalInterest(principal, monthly_payment, term_months)
   - calculateAmortizationSchedule(principal, annual_rate, term_months)
   - calculateDSCR(net_operating_income, annual_debt_service)
   - calculateEffectiveAPR(principal, total_cost, term_months)

2. **Cash Flow Formulas**
   - calculateBurnRate(monthly_revenue, monthly_expenses)
   - calculateRunway(cash_balance, monthly_burn)
   - calculateBreakeven(fixed_costs, contribution_margin_per_unit)

3. **Profitability Formulas**
   - calculateContributionMargin(selling_price, variable_cost)
   - calculateMarginOfSafety(current_revenue, breakeven_revenue)
   - calculateGrossMargin(revenue, cogs)

4. **Valuation Formulas**
   - calculateNPV(cash_flows, discount_rate)
   - calculateIRR(cash_flows)
   - calculateMultipleValuation(metric, multiple_low, multiple_high)

5. **Utility Formulas**
   - formatCurrency(value, decimals)
   - formatPercentage(value, decimals)
   - validatePositive(value)

**For each formula, define:**
- Function signature (name, parameters with types)
- Return value (type, structure)
- Edge case handling (divide by zero, negative values, etc.)
- Example usage
- Formula version (when introduced, any changes)

**Formula calling from JSON:**
```json
{
  "step_id": "calc_payment",
  "formula_function": "calculateMonthlyPayment",
  "inputs": ["loan_amount", "interest_rate", "term_years"],
  "output_variable": "monthly_payment"
}
```

**Format:** Section per formula category, with function signatures and examples. 5-6 pages.

---

## 12.4 DEPLOYMENT PIPELINE

**File:** `12.4-deployment-pipeline.md`

**Content:**
Define the automated deployment process from JSON to live calculator.

**Deployment steps:**

1. **Create/Edit JSON Config**
   - Developer creates /calculators/configs/new-calculator.json
   - Follows schema from 12.2
   - Commits to Git (feature branch)

2. **Automated Validation (CI/CD)**
   - JSON schema validation (is it valid JSON, does it match schema?)
   - Formula library check (do all referenced formulas exist?)
   - Tier config validation (are tier rules valid?)
   - Golden scenario check (if provided, do calculations match expected outputs?)

3. **UI Generation (Build Time)**
   - React components generated from JSON
   - Input fields created with validation rules
   - Result cards created for outputs
   - Warning components wired to conditions

4. **Formula Wiring (Build Time)**
   - Calculation steps executed in order
   - Formula library functions called with correct inputs
   - Output variables stored for use in next steps

5. **Export Template Generation (Build Time)**
   - PDF template generated from export_config
   - CSV structure defined
   - Metadata included (version, timestamp)

6. **Deployment (Merge to Main)**
   - Pass all validation → merge to main branch
   - Auto-deploy to staging for final QA
   - Deploy to production (blue-green deployment)
   - WordPress shortcode auto-generated and documented

7. **Post-Deployment Verification**
   - Health check: calculator loads successfully
   - Smoke test: run one calculation, verify output
   - Monitor error rates for first 24 hours

**Time breakdown:**
- Create JSON: 5 minutes
- Commit and validation: 2 minutes
- Build and deploy: 3 minutes
- **Total: ~10 minutes from concept to live**

**Rollback process:**
- Revert Git commit
- Previous version deploys automatically
- Takes < 5 minutes

**Format:** Section per deployment step, with timeline and automation details. 4-5 pages.

---

## 12.5 CALCULATOR EXAMPLES

**File:** `12.5-calculator-examples.md`

**Content:**
Provide 3 complete JSON examples showing different calculator types.

**Example 1: Simple Calculator (Loan Payment)**
- Full JSON config for a basic loan payment calculator
- 3 inputs (loan_amount, interest_rate, term_years)
- 1 calculation step (calculateMonthlyPayment)
- 2 outputs (monthly_payment, total_interest)
- 1 warning (high interest rate)
- Minimal tier config (free shows all)

**Example 2: Moderate Complexity (DSCR)**
- Full JSON config for Business Loan + DSCR
- 5 inputs (loan, rate, term, revenue, expenses)
- 3 calculation steps (payment, annual debt service, DSCR)
- 5 outputs (2 key, 3 advanced)
- 2 warnings (DSCR too low, DSCR excellent)
- Pro tier gates advanced metrics

**Example 3: Complex Calculator (Lease vs Buy)**
- Full JSON config for Equipment Lease vs Buy
- 7 inputs (cost, lease payment, term, loan rate, tax rate, residual value, purchase option)
- 6 calculation steps (lease cost, loan cost, NPV calculations, after-tax comparison)
- 8 outputs (3 key, 5 advanced including NPV and IRR)
- 3 warnings (lease expensive, buy expensive, close comparison)
- AI config included (prompt template for recommendation)

**For each example:**
- Complete JSON file (copy-paste ready)
- Explanation of each section
- Screenshot or wireframe of resulting calculator UI
- Notes on why this structure was chosen

**Format:** Section per example, with full JSON and explanations. 6-8 pages.

---

## Overall Guidelines

**Tone:** Implementation-focused and tutorial-style. Writing for developers who will use this system.

**Formatting:**
- Markdown headers: ##, ###, ####
- JSON code blocks with syntax highlighting
- Tables for formula signatures
- Bullet lists for steps and requirements
- ASCII diagrams for architecture

**Consistency:**
- Reference Section 3 for formula library concept
- Reference Section 11 for calculator structure
- Use consistent JSON naming (snake_case for field names)
- Use consistent formula naming (camelCase for function names)

**Length:** 25-32 pages total across 5 files

**Output Files:**
- 12.1-assembly-line-architecture.md
- 12.2-json-schema-specification.md
- 12.3-formula-library-api.md
- 12.4-deployment-pipeline.md
- 12.5-calculator-examples.md

Each file starts with # header matching title.