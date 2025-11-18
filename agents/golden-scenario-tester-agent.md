# Golden Scenario Tester Agent

## Purpose
Run end-to-end tests of complete calculators using golden scenarios to ensure the entire system (inputs → calculations → outputs → warnings) works correctly.

## Input
- Calculator to test (e.g., `business-loan-dscr`)
- Golden scenarios for that calculator (e.g., `tests/golden-scenarios/business-loan-dscr_golden_*.json`)
- Tolerance settings (from scenario files or defaults)

## Process

### Step 1: Load Calculator Configuration
- Read calculator JSON config from `calculators/configs/{calculator-slug}.json`
- Validate config is valid (use Schema Validator Agent)
- Load calculator build artifacts from `calculators/built/{calculator-slug}/`
- Verify calculator is deployed and accessible

### Step 2: Load Golden Scenarios for Calculator
- Find all golden scenario files for this calculator
- Expected naming: `{calculator-slug}_golden_1.json`, `{calculator-slug}_golden_2.json`, etc.
- Parse each scenario file
- Verify each scenario has:
  - scenario_name
  - description
  - inputs (all required fields present)
  - expected_outputs (with values and tolerances)
  - expected_warnings (array, can be empty)

### Step 3: Execute Each Golden Scenario End-to-End

For each scenario:

#### 3a. Load Input Values
- Set all input fields to values from scenario
- Verify all required inputs provided
- Verify input validation passes

#### 3b. Trigger Calculation
- Execute calculator calculation logic
- Run all calculation steps in order
- Capture all output variables

#### 3c. Compare Outputs
For each expected output:
- Get actual value from calculator
- Get expected value from golden scenario
- Get tolerance from golden scenario
- Calculate: |actual - expected|
- Check: difference <= tolerance
- Record: PASS or FAIL with details

#### 3d. Validate Warnings
- Check which warnings triggered
- Compare to expected_warnings array
- Expected warning present but not triggered: FAIL
- Unexpected warning triggered: FAIL
- All match: PASS

#### 3e. Validate Tier Gating
- If calculator has advanced metrics:
  - Simulate Free tier: verify advanced metrics hidden
  - Simulate Pro tier: verify advanced metrics visible
- Verify upgrade prompts appear for gated features

#### 3f. Test Export Generation
- Generate PDF export with scenario results
- Verify PDF contains:
  - All inputs and outputs
  - Calculator version and timestamp
  - Watermark (if Free tier simulation)
- Check: PDF generation completes within 3 seconds (Section 1.6 SLA)

#### 3g. Test AI Integration (if ai_enabled)
- Trigger AI explanation request
- Verify AI prompt constructed correctly (Section 8 redaction rules)
- Check: AI response returned within 8 seconds (or timeout)
- Verify disclaimer present in AI output

### Step 4: Performance Testing
For each scenario:
- Measure calculation time (input → output)
- Check: p95 < 150ms (Section 1.6 SLA)
- Measure export generation time
- Check: p95 < 3 seconds (Section 1.6 SLA)
- Flag any scenarios exceeding SLAs

### Step 5: Generate Test Report

Create detailed report with:
- Scenario summary (total, passed, failed)
- Output accuracy results
- Warning validation results
- Tier gating test results
- Export test results
- AI test results (if applicable)
- Performance metrics
- Overall pass/fail status

## Output

### Summary Report: