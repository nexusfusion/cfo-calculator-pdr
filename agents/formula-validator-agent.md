# Formula Validator Agent

## Purpose
Test all formulas in the formula library against golden scenarios to ensure mathematical accuracy and catch regressions.

## Input
- Formula library path (e.g., `src/formulas/`)
- Golden scenarios directory (e.g., `tests/golden-scenarios/`)
- Tolerance settings (e.g., ±0.01 for percentages, ±$1 for currency)

## Process

### Step 1: Load Formula Library
- Read all formula functions from formula library
- List available formulas with signatures
- Verify each formula has:
  - Function name
  - Input parameters with types
  - Return type
  - Documentation

### Step 2: Load Golden Scenarios
- Read all JSON files from golden scenarios directory
- Parse each scenario file (format from Section 10.2)
- Group scenarios by calculator
- Count total scenarios to test

### Step 3: Execute Each Golden Scenario
For each scenario:
- Read scenario inputs
- Call referenced formulas with inputs
- Capture actual outputs
- Compare actual vs expected outputs
- Check if within tolerance

### Step 4: Validate Output Accuracy
For each output:
- Expected value from golden scenario
- Actual value from formula execution
- Tolerance from golden scenario (or default)
- Calculate difference: |actual - expected|
- Check: difference <= tolerance
- Record: PASS or FAIL

### Step 5: Validate Expected Warnings
- Execute warning conditions with scenario outputs
- Check if expected warnings triggered
- Check if unexpected warnings triggered
- Record: PASS (all match) or FAIL (mismatch)

### Step 6: Check for Edge Cases
Test each formula with edge case inputs:
- Zero values (where applicable)
- Negative values (where not allowed)
- Very large values (test overflow)
- Very small values (test underflow)
- Division by zero scenarios
- Verify graceful error handling

### Step 7: Performance Testing
For each formula:
- Execute 1000 times with typical inputs
- Measure average execution time
- Check: average time < 1ms per calculation
- Flag formulas that are too slow

### Step 8: Generate Test Report
Create detailed report:
- Total scenarios tested
- Pass/fail count per formula
- Pass/fail count per calculator
- Tolerance violations with details
- Edge case failures
- Performance issues
- Overall pass rate

## Output

### Summary Report:
```
Formula Validation Report
=========================
Generated: 2025-11-18 18:15:00
Formula Library Version: v1.0.0

Golden Scenarios Tested: 24
Calculators Covered: 8

Results:
✓ calculateMonthlyPayment: 8/8 scenarios passed
✓ calculateTotalInterest: 8/8 scenarios passed
✓ calculateDSCR: 8/8 scenarios passed
✗ calculateNPV: 2/3 scenarios passed
  - FAIL: equipment-lease-buy_golden_2.json
    Expected: $95,432.10 ± $10
    Actual: $95,455.32
    Difference: $23.22 (EXCEEDS TOLERANCE)
✓ calculateIRR: 3/3 scenarios passed

Overall: 22/24 scenarios passed (91.7%)

⚠️ 1 formula has failures - review required before deployment
```

### Detailed Failure Report:
```
FAILURE DETAILS
===============

Calculator: equipment-lease-buy
Scenario: equipment-lease-buy_golden_2.json
Formula: calculateNPV

Inputs:
  cash_flows: [-100000, 25000, 30000, 35000, 40000]
  discount_rate: 0.08

Expected Output:
  npv: 95432.10 ± 10.00

Actual Output:
  npv: 95455.32

Difference: 23.22 (tolerance: 10.00)
Status: FAIL - exceeds tolerance

Recommendation:
  - Review NPV formula implementation
  - Check discount rate compounding logic
  - Update golden scenario if formula is correct
```

## Success Criteria
- All golden scenarios pass (100% pass rate)
- No tolerance violations
- All edge cases handled gracefully
- All formulas execute within performance budget (<1ms average)
- No unexpected warnings triggered

## Failure Actions
- **Critical failures (>5% scenarios fail):** Block deployment
- **Minor failures (1-5% scenarios fail):** Warning, requires review
- **Tolerance violations:** Investigate formula or update golden scenario
- **Performance failures:** Optimize formula or flag for refactoring

## Usage Example
```bash
# Validate all formulas
claude-code "Use formula-validator-agent to test all formulas against golden scenarios"

# Validate specific formula
claude-code "Use formula-validator-agent to test calculateDSCR formula only"

# Validate specific calculator
claude-code "Use formula-validator-agent to test business-loan-dscr calculator scenarios"
```

## Dependencies
- Section 10.2: Golden scenario framework
- Section 12.3: Formula library API
- Section 1.6: Quality goals (95% accuracy target)

## Integration with CI/CD
- Run automatically on every commit
- Block merge if failures detected
- Generate report artifact for review
- Update dashboard with test results