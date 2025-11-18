# Calculator Doc Generator Agent

## Purpose
Automatically generate comprehensive documentation for calculators from their JSON configuration files, ensuring consistent, accurate, and complete documentation across all 450+ calculators.

## Input
- Calculator JSON config file (e.g., `calculators/configs/business-loan-dscr.json`)
- Documentation template (from Section 11 template)
- Target output directory (e.g., `docs/calculators/`)

## Process

### Step 1: Read Calculator Configuration
- Parse JSON config file
- Extract all metadata:
  - calculator_meta (name, description, version, category, business_role)
  - inputs (all fields with validation rules, tooltips)
  - calculations (all formulas used)
  - outputs (key and advanced metrics)
  - warnings (conditions and messages)
  - tier_config (Free vs Pro behavior)
  - ai_config (if AI-enabled)

### Step 2: Generate Calculator Overview Section

#### 2a. Title and Metadata
```markdown
# Business Loan + DSCR Calculator

**Version:** 2.1.0  
**Category:** Financing & Lending Intelligence  
**Status:** Production  
**Last Updated:** 2025-11-18

## Overview

Calculate monthly loan payments and Debt Service Coverage Ratio (DSCR) for business loans. This calculator helps business owners and lenders evaluate loan affordability and assess whether a business generates sufficient cash flow to cover debt obligations.

**Primary Use Cases:**
- SBA loan evaluation
- Conventional business loan analysis
- Lender underwriting assessment
- Business loan comparison

**Business Role:** Pro Upgrade Driver  
**Success Metric:** Pro conversion rate from DSCR calculation
```

### Step 3: Generate User Scenarios Section

Extract from calculator's intended use:
```markdown
## Who Uses This Calculator

**Business Owners:**
- Evaluating loan affordability before applying
- Understanding how much debt their business can support
- Preparing for lender conversations

**Lenders & Underwriters:**
- Quick DSCR calculation during initial screening
- Assessing loan risk and covenant compliance
- Determining appropriate loan terms

**Financial Advisors:**
- Helping clients evaluate financing options
- Modeling different loan scenarios
- Preparing loan applications
```

### Step 4: Generate Inputs Documentation

For each input field, create detailed documentation:
```markdown
## Calculator Inputs

### Required Inputs

#### Loan Amount
- **Field Type:** Currency (USD)
- **Validation:** Must be positive, $1,000 minimum
- **Tooltip:** Total amount you want to borrow
- **Example:** $250,000
- **Notes:** Include any upfront fees or costs in the total loan amount if they're being financed

#### Interest Rate
- **Field Type:** Percentage
- **Validation:** Must be positive, 0.01% to 50%
- **Tooltip:** Annual interest rate for the loan
- **Example:** 7.5%
- **Notes:** Use the annual percentage rate (APR) provided by the lender

#### Loan Term
- **Field Type:** Number (years)
- **Validation:** Must be positive, 1 to 30 years
- **Tooltip:** How many years to repay the loan
- **Example:** 10 years
- **Notes:** Common terms are 5, 10, 15, 20, or 25 years depending on loan type

#### Annual Revenue
- **Field Type:** Currency (USD)
- **Validation:** Must be positive
- **Tooltip:** Your business's total annual revenue
- **Example:** $1,500,000
- **Notes:** Use trailing 12-month revenue or projected annual revenue

#### Annual Operating Expenses
- **Field Type:** Currency (USD)
- **Validation:** Must be positive, must be less than annual revenue
- **Tooltip:** Your business's total annual operating costs (excluding debt service)
- **Example:** $1,200,000
- **Notes:** Include all operating expenses except debt payments (payroll, rent, supplies, etc.)
```

### Step 5: Generate Outputs Documentation

For each output metric:
```markdown
## Calculator Outputs

### Key Metrics (Free Tier)

#### Monthly Payment
- **Format:** Currency (USD)
- **Formula:** Standard amortization formula: P * [r(1+r)^n] / [(1+r)^n - 1]
- **Example:** $2,958.00
- **Interpretation:** This is your required monthly loan payment including principal and interest

#### Total Interest
- **Format:** Currency (USD)
- **Formula:** (Monthly Payment * Number of Payments) - Loan Amount
- **Example:** $104,960
- **Interpretation:** Total interest paid over the life of the loan

### Advanced Metrics (Pro Tier)

#### Debt Service Coverage Ratio (DSCR)
- **Format:** Ratio (X.XX)
- **Formula:** Net Operating Income / Annual Debt Service
- **Example:** 1.42
- **Interpretation:** 
  - DSCR > 1.25: Generally acceptable for most lenders
  - DSCR 1.15-1.25: Marginal, may require additional collateral
  - DSCR < 1.15: High risk, likely to be declined
- **Why It Matters:** Lenders use DSCR to assess whether your business generates enough cash flow to comfortably cover loan payments

#### Net Operating Income (NOI)
- **Format:** Currency (USD)
- **Formula:** Annual Revenue - Annual Operating Expenses
- **Example:** $300,000
- **Interpretation:** Your business's cash flow available to service debt

#### Annual Debt Service
- **Format:** Currency (USD)
- **Formula:** Monthly Payment * 12
- **Example:** $35,496
- **Interpretation:** Total annual loan payments (principal + interest)

#### Covenant Headroom
- **Format:** Percentage
- **Formula:** (DSCR - Minimum Required DSCR) / Minimum Required DSCR * 100
- **Example:** 13.6%
- **Interpretation:** How much cushion you have above the typical lender minimum (1.25 DSCR)
```

### Step 6: Generate Formulas & Calculations Section

Document calculation logic:
```markdown
## Formulas & Calculation Logic

### Calculation Steps

**Step 1: Calculate Monthly Payment**
```
Formula: PMT(rate, nper, pv)
Where:
- rate = annual_interest_rate / 12
- nper = term_years * 12
- pv = -loan_amount (negative because it's money borrowed)

Implementation: Uses calculateMonthlyPayment() from formula library v1.0.0
Reference: Standard amortization formula (financial mathematics)
```

**Step 2: Calculate Total Interest**
```
Formula: (monthly_payment * term_years * 12) - loan_amount

Implementation: Simple arithmetic
```

**Step 3: Calculate Net Operating Income**
```
Formula: annual_revenue - annual_operating_expenses

Implementation: Simple subtraction
```

**Step 4: Calculate Annual Debt Service**
```
Formula: monthly_payment * 12

Implementation: Simple multiplication
```

**Step 5: Calculate DSCR**
```
Formula: net_operating_income / annual_debt_service

Implementation: Uses calculateDSCR() from formula library v1.0.0
Reference: Standard lending metric
```

### Edge Cases & Error Handling

**Zero or Negative Inputs:**
- Validation prevents negative values from being entered
- Zero values handled gracefully with error messages

**Division by Zero:**
- If annual_debt_service is zero (shouldn't happen with positive loan amount)
- Returns error: "Cannot calculate DSCR with zero debt service"

**Extremely Long Terms:**
- Terms over 30 years may be unrealistic for business loans
- Warning shown: "Loan term unusually long, verify with lender"
```

### Step 7: Generate Warnings Documentation

For each warning condition:
```markdown
## Warnings & Alerts

### Info Warnings

**"DSCR is above typical lender minimum"**
- **Condition:** DSCR >= 1.50
- **Severity:** Info (blue)
- **Meaning:** Your debt service coverage ratio is strong. Most lenders will view this favorably.
- **Action:** This is a positive indicator. No action needed.

### Warning Alerts

**"DSCR is below typical lender minimum"**
- **Condition:** DSCR < 1.25
- **Severity:** Warning (yellow)
- **Meaning:** Your DSCR is below the typical minimum lenders require (1.25).
- **Action:** Consider:
  - Reducing loan amount
  - Extending loan term to lower monthly payments
  - Increasing revenue or reducing expenses before applying

### Danger Alerts

**"DSCR indicates insufficient cash flow"**
- **Condition:** DSCR < 1.10
- **Severity:** Danger (red)
- **Meaning:** Your business may not generate enough cash flow to reliably cover loan payments.
- **Action:** 
  - Loan application likely to be declined
  - Review business financials carefully
  - Consider alternative financing options
  - Consult with a financial advisor
```

### Step 8: Generate Golden Test Scenarios Section

Document the golden scenarios:
```markdown
## Golden Test Scenarios

These scenarios are used for automated testing to ensure calculator accuracy.

### Scenario 1: Typical SBA Loan - Restaurant

**Description:** Standard SBA 7(a) loan for a restaurant purchasing equipment

**Inputs:**
- Loan Amount: $250,000
- Interest Rate: 7.5%
- Term: 10 years
- Annual Revenue: $1,500,000
- Annual Operating Expenses: $1,200,000

**Expected Outputs:**
- Monthly Payment: $2,958.00 (±$1.00)
- Total Interest: $104,960 (±$100)
- DSCR: 1.42 (±0.01)
- Net Operating Income: $300,000 (±$100)
- Annual Debt Service: $35,496 (±$50)

**Expected Warnings:**
- Info: "DSCR is above typical lender minimum"

**Test Status:** ✅ Passing (last verified: 2025-11-18)

### Scenario 2: Low DSCR - Retail Business

**Description:** Retail business with tight cash flow, DSCR near minimum threshold

**Inputs:**
- Loan Amount: $250,000
- Interest Rate: 8.0%
- Term: 10 years
- Annual Revenue: $1,200,000
- Annual Operating Expenses: $1,158,000

**Expected Outputs:**
- Monthly Payment: $3,033.19 (±$1.00)
- DSCR: 1.18 (±0.01)

**Expected Warnings:**
- Warning: "DSCR is below typical lender minimum"

**Test Status:** ✅ Passing (last verified: 2025-11-18)

### Scenario 3: Strong DSCR - Service Business

**Description:** Service business with strong cash flow and low debt burden

**Inputs:**
- Loan Amount: $150,000
- Interest Rate: 6.5%
- Term: 7 years
- Annual Revenue: $2,000,000
- Annual Operating Expenses: $1,500,000

**Expected Outputs:**
- DSCR: 2.10 (±0.01)

**Expected Warnings:**
- Info: "DSCR is above typical lender minimum"

**Test Status:** ✅ Passing (last verified: 2025-11-18)
```

### Step 9: Generate Tier Behavior Section

Document Free vs Pro features:
```markdown
## Tier-Specific Behavior

### Free Tier
**Included:**
- Single scenario
- Monthly payment calculation
- Total interest calculation
- Basic loan information

**Gated (Pro Upgrade Required):**
- DSCR calculation and analysis
- Net Operating Income
- Annual Debt Service
- Covenant Headroom
- Multiple scenario comparison
- Clean exports (PDF watermarked in Free)

**Upgrade Triggers:**
- Clicking on locked DSCR metric
- Attempting to create second scenario
- Clicking export button (shows upgrade prompt)

### Pro Tier ($19.99/month)
**Unlocked:**
- All advanced metrics (DSCR, NOI, debt service)
- Up to 50 scenarios
- Scenario comparison
- Clean PDF exports
- CSV and Excel exports
- Priority support

### AI Tier ($19.99/month additional)
**Unlocked:**
- AI-powered explanations of DSCR results
- Scenario recommendations
- Risk analysis narrative
- 50 AI requests per month
```

### Step 10: Generate AI Integration Section (if applicable)

If ai_enabled=true:
```markdown
## AI Features

### AI Explanation: "What does my DSCR mean?"

**Trigger:** Click "Explain this result" button next to DSCR output

**What AI Explains:**
- What your specific DSCR value means
- How lenders will interpret this ratio
- What risks or strengths it indicates
- Suggestions for improving DSCR if needed

**Example AI Response:**
> "Your DSCR of 1.42 indicates that your business generates $1.42 in operating income for every $1 in debt service. This is a healthy ratio that most lenders will view favorably, as it provides a 42% cushion above the typical 1.25 minimum threshold. This suggests your business has adequate cash flow to comfortably handle the loan payments with room for unexpected expenses or revenue fluctuations."

**Important:** AI responses are for informational purposes only and do not constitute financial advice.

**Redaction & Privacy:**
- Only numerical results sent to AI (no business names, no free-text notes)
- No personally identifiable information included
- See Section 8 for full AI privacy policy
```

### Step 11: Generate Export Documentation

Document export formats:
```markdown
## Exports

### PDF Export
**Includes:**
- All inputs and outputs
- Charts (if generated)
- Warnings and alerts
- Calculator version and timestamp
- Methodology section
- Disclaimers

**Free Tier:** Watermarked with "Generated with Free Tier - Upgrade for clean exports"
**Pro Tier:** Clean, professional PDF suitable for lenders or board presentations

**Generation Time:** Typically 2-3 seconds

### CSV Export (Pro Only)
**Includes:**
- All inputs and outputs in tabular format
- Metadata header rows
- Easy to import into Excel or Google Sheets

### Excel Export (Pro Only)
**Includes:**
- Multiple sheets: Summary, Inputs, Outputs, Methodology
- Formatted with proper number formats
- Charts embedded
- Professional styling
```

### Step 12: Generate FAQs Section

Auto-generate common questions:
```markdown
## Frequently Asked Questions

**Q: What is a good DSCR for a business loan?**
A: Most lenders require a minimum DSCR of 1.25, meaning your business should generate $1.25 in operating income for every $1 in debt payments. A DSCR of 1.50 or higher is considered strong. Below 1.25 may result in loan denial or require additional collateral.

**Q: Should I include my salary in operating expenses?**
A: Yes, if you're an employee of the business. Include all payroll costs in operating expenses. However, owner draws or distributions should NOT be included—these come from net operating income.

**Q: Can I use this calculator for personal loans?**
A: No, this calculator is designed for business loans with DSCR analysis. For personal loans, use a standard loan calculator without the DSCR component.

**Q: What if my business is seasonal?**
A: Use your trailing 12-month revenue and expenses, or project annual figures based on seasonal patterns. Lenders typically evaluate annual cash flow, not monthly fluctuations.

**Q: How accurate is this calculator?**
A: The payment calculations use standard amortization formulas and are accurate to the cent. However, your actual loan terms may vary based on lender-specific fees, prepayment penalties, or other factors. Always verify final terms with your lender.

**Q: Why can't I see the DSCR metric?**
A: DSCR calculation is a Pro tier feature. Upgrade to Pro to unlock DSCR analysis and advanced metrics. Click the locked metric for upgrade options.
```

### Step 13: Generate Related Calculators Section
```markdown
## Related Calculators

**For more complete loan analysis, also use:**
- **SBA 7(a) Loan Analyzer:** Includes SBA guarantee fees and effective APR
- **Equipment Lease vs Buy:** Compare financing options for equipment purchases
- **Cash Runway Calculator:** Understand how loan payments affect your runway

**For business planning:**
- **Breakeven Analysis:** Determine required revenue to cover all costs including loan payments
- **Business Valuation:** Estimate business value considering debt load
```

### Step 14: Generate Version History Section
```markdown
## Version History

### v2.1.0 (2025-11-18)
- Added covenant headroom metric
- Improved DSCR warning thresholds
- Enhanced AI explanations
- Updated golden test scenarios

### v2.0.0 (2025-10-15)
- Major redesign with new UI
- Added multiple scenario support (Pro tier)
- Implemented tier gating
- Added AI integration

### v1.0.0 (2025-08-01)
- Initial release
- Basic loan payment calculation
- DSCR calculation (Pro tier)
```

### Step 15: Generate Support & Contact Section
```markdown
## Support & Resources

**Documentation:** https://docs.smartprofit.com/calculators/business-loan-dscr  
**WordPress Shortcode:** `[smartprofit_calculator slug="business-loan-dscr"]`  
**API Endpoint:** `https://api.smartprofit.com/v1/calculate/business-loan-dscr`  
**Support:** support@smartprofit.com  
**Feature Requests:** https://feedback.smartprofit.com

**Disclaimers:**
This calculator is for informational and educational purposes only. It does not constitute financial, legal, or professional advice. Actual loan terms and DSCR requirements vary by lender. Consult with qualified professionals before making financial decisions.
```

## Output

### Generated Documentation File:
`docs/calculators/business-loan-dscr.md` (10-15 pages)

### Success Message:
```
✅ Calculator Documentation Generated

Calculator: business-loan-dscr
Version: v2.1.0
Output: docs/calculators/business-loan-dscr.md

Documentation Sections:
✓ Overview and metadata
✓ User scenarios (3 personas)
✓ Input documentation (5 fields)
✓ Output documentation (5 metrics)
✓ Formulas and calculations (5 steps)
✓ Warnings documentation (3 conditions)
✓ Golden scenarios (3 scenarios)
✓ Tier behavior (Free/Pro/AI)
✓ AI integration details
✓ Export formats
✓ FAQs (5 questions)
✓ Related calculators
✓ Version history
✓ Support information

Total Length: ~12 pages
Last Updated: 2025-11-18
```

## Success Criteria
- All JSON config sections documented
- All inputs have detailed descriptions
- All outputs have interpretation guides
- Formulas clearly explained with references
- Golden scenarios included
- Tier behavior clearly stated
- FAQs answer common questions
- No missing sections

## Quality Checks
- Technical accuracy (formulas match implementation)
- Clarity (understandable by non-technical users)
- Completeness (no missing information)
- Consistency (matches other calculator docs)
- Proper markdown formatting

## Usage Example
```bash
# Generate docs for one calculator
claude-code "Use calculator-doc-generator-agent to generate documentation for business-loan-dscr calculator"

# Regenerate docs for all calculators
claude-code "Use calculator-doc-generator-agent to regenerate documentation for all calculators"

# Update docs after calculator version change
claude-code "Use calculator-doc-generator-agent to update documentation for business-loan-dscr calculator version 2.2.0"
```

## Dependencies
- Section 11: Calculator PDR template (structure reference)
- Section 12.2: JSON schema (for reading config)
- Calculator JSON config files
- Golden scenario files (Section 10.2)