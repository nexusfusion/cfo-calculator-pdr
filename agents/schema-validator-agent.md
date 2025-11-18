# Schema Validator Agent

## Purpose
Validate JSON calculator configurations against the schema defined in Section 12.2 before deployment.

## Input
- Path to JSON config file to validate
- Schema definition (from Section 12.2)

## Process

### Step 1: Load and Parse JSON
- Read JSON file
- Attempt to parse as valid JSON
- If parse fails: output syntax errors with line numbers

### Step 2: Validate Root Structure
- Check all required root keys present:
  - calculator_meta ✓
  - inputs ✓
  - calculations ✓
  - outputs ✓
  - warnings ✓
  - tier_config ✓
  - export_config ✓
- Check no unexpected keys present

### Step 3: Validate calculator_meta
- calculator_slug: string, matches pattern [a-z0-9-]+
- calculator_name: string, not empty
- calculator_version: string, matches semantic versioning (X.Y.Z)
- category: enum (financing-lending, cash-flow, profitability, valuation, planning)
- description: string, 10-500 characters
- business_role: enum (traffic_magnet, pro_driver, ai_showcase, b2b_demo)
- success_metric: string, not empty

### Step 4: Validate inputs Array
For each input object:
- field_id: string, unique within inputs, matches [a-z_]+
- field_name: string, not empty
- field_type: enum (number, currency, percentage, dropdown, slider)
- units: string
- required: boolean
- default_value: number or null
- placeholder: string
- tooltip: string, 10-200 characters
- validation object:
  - min: number or null
  - max: number or null
  - must_be_positive: boolean

Check for duplicate field_ids

### Step 5: Validate calculations Array
For each calculation:
- step_id: string, unique within calculations
- formula_function: string, must exist in formula library (check against Section 12.3)
- inputs: array of strings (field_ids or step_ids that exist)
- output_variable: string, unique across all calculations
- Verify calculation order (no forward references)

### Step 6: Validate outputs Object
- key_metrics: array of metric objects
- advanced_metrics: array of metric objects

For each metric:
- metric_id: string, unique
- metric_name: string, not empty
- variable: string, references a calculation output_variable
- format: enum (currency, percentage, number, ratio)
- decimals: number, 0-4
- tooltip: string, 10-200 characters

### Step 7: Validate warnings Array
For each warning:
- warning_id: string, unique
- condition: string, valid formula expression (references variables)
- severity: enum (info, warning, danger)
- message: string, 20-500 characters

### Step 8: Validate tier_config
- free_tier object:
  - max_scenarios: number, typically 1
  - visible_outputs: array of metric_ids (all must exist in outputs)
- pro_tier object:
  - max_scenarios: number, typically 50
  - visible_outputs: array of metric_ids
- ai_enabled: boolean

### Step 9: Validate export_config
- pdf_sections: array of strings
- csv_columns: array of strings
- include_charts: boolean

### Step 10: Validate ai_config (if present)
- ai_prompt_template: string, contains {variable} placeholders
- ai_response_length: number, 50-500

## Output

### If Valid:
```
✓ JSON syntax valid
✓ Root structure valid
✓ calculator_meta valid
✓ inputs valid (5 fields)
✓ calculations valid (3 steps)
✓ outputs valid (2 key, 3 advanced)
✓ warnings valid (2 conditions)
✓ tier_config valid
✓ export_config valid

✅ Configuration is VALID and ready for deployment
```

### If Invalid:
```
✗ JSON syntax valid
✓ Root structure valid
✗ calculator_meta: category "invalid-category" not in allowed values
✓ inputs valid (5 fields)
✗ calculations: formula_function "calculateXYZ" not found in formula library
  Available formulas: [list from Section 12.3]
✗ outputs: metric references undefined variable "total_xyz"
✓ warnings valid (2 conditions)
✓ tier_config valid
✓ export_config valid

❌ Configuration has 3 ERRORS - must be fixed before deployment
```

## Success Criteria
- All validation checks pass
- No errors reported
- All referenced formulas exist
- All variable references are valid
- Tier config is consistent

## Error Categories
- **Syntax errors**: JSON parsing fails
- **Schema errors**: Missing required fields, wrong types
- **Reference errors**: Variables or formulas don't exist
- **Logic errors**: Circular dependencies, forward references
- **Consistency errors**: Tier config doesn't match outputs

## Usage Example
```bash
# Validate a config
claude-code "Use schema-validator-agent to validate calculators/configs/business-loan-dscr.json"
```

## Dependencies
- Section 12.2: JSON schema specification
- Section 12.3: Formula library API (for formula validation)