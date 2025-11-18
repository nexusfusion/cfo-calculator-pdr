# Calculator Builder Agent

## Purpose
Transform a JSON calculator configuration into a working, deployable calculator with UI components, formula logic, and tier gating.

## Input
- Path to JSON config file (e.g., `calculators/configs/business-loan-dscr.json`)
- Target output directory (e.g., `calculators/built/business-loan-dscr/`)

## Process

### Step 1: Validate JSON Config
- Read the JSON file
- Validate against schema from Section 12.2
- Check all referenced formulas exist in formula library
- Verify tier_config is valid
- If validation fails: output errors and stop

### Step 2: Generate Input Components
- For each input in `inputs` array:
  - Create React input component based on field_type
  - Apply validation rules (min, max, must_be_positive)
  - Add tooltip with help text
  - Wire up onChange handlers
  - Generate field according to Section 4 UX standards

### Step 3: Generate Calculation Logic
- For each step in `calculations` array:
  - Map formula_function to formula library function
  - Create function call with correct inputs
  - Store result in output_variable
  - Handle edge cases (null, undefined, division by zero)
  - Execute calculations in order (respect dependencies)

### Step 4: Generate Output Components
- For key_metrics:
  - Create result cards (always visible)
  - Apply formatting (currency, percentage, decimals)
  - Add tooltips
- For advanced_metrics:
  - Create result cards with Pro gate
  - Show lock icon and upgrade prompt for Free tier
  - Display values for Pro tier

### Step 5: Generate Warning Logic
- For each warning in `warnings` array:
  - Evaluate condition after calculations complete
  - If condition true: display warning with severity styling
  - Use warning message text from JSON
  - Apply Section 4 warning styles (info/warning/danger colors)

### Step 6: Apply Tier Gating
- Read tier_config
- Implement scenario limits (1 for Free, 50 for Pro)
- Gate advanced metrics visibility
- Add upgrade prompts where appropriate
- Follow Section 6 tier gating patterns

### Step 7: Generate Export Templates
- Create PDF template based on export_config
  - Include all inputs and outputs
  - Add calculator version, timestamp
  - Add watermark for Free tier (Section 6.1)
- Create CSV structure
- Create Excel structure (if specified)

### Step 8: Generate AI Integration (if ai_enabled)
- Use ai_prompt_template from JSON
- Wire up "Explain this result" button
- Follow Section 8 AI framework (redaction, disclaimers)
- Implement usage caps (50 requests/month for AI tier)

### Step 9: Output Built Calculator
- Write all generated files to output directory:
  - `Calculator.tsx` (main React component)
  - `calculations.ts` (calculation logic)
  - `types.ts` (TypeScript interfaces)
  - `config.json` (runtime config for tier gating)
  - `export-templates/` (PDF, CSV, Excel templates)
- Generate deployment manifest
- Create WordPress shortcode definition

## Output
- Built calculator in `calculators/built/{calculator-slug}/`
- Deployment manifest with:
  - Calculator slug
  - Version
  - Files generated
  - Dependencies (formula library functions used)
  - Deployment instructions

## Success Criteria
- All files generated without errors
- Calculator passes schema validation
- Generated code follows TypeScript standards
- UI matches Section 4 design system
- Tier gating matches Section 6 rules

## Error Handling
- If formula not found: List missing formulas, provide formula library reference
- If validation fails: Output specific validation errors with line numbers
- If tier_config invalid: Explain what's wrong, provide example
- If AI config present but ai_enabled false: Warning (config ignored)

## Usage Example
```bash
# Run the agent
claude-code "Use calculator-builder-agent to build calculator from calculators/configs/business-loan-dscr.json"

# Expected output
✓ JSON validated
✓ Input components generated (5 fields)
✓ Calculation logic generated (3 steps)
✓ Output components generated (2 key, 3 advanced)
✓ Warnings generated (2 conditions)
✓ Tier gating applied
✓ Export templates created (PDF, CSV)
✓ Calculator built successfully

Output: calculators/built/business-loan-dscr/
```

## Dependencies
- Section 12.2: JSON schema specification
- Section 3.3: Formula library API
- Section 4: UX design standards
- Section 6: Tier gating rules
- Section 8: AI framework (if ai_enabled)