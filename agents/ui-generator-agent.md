# UI Generator Agent

## Purpose
Generate React/TypeScript UI components from JSON calculator configuration, following Section 4 UX design standards.

## Input
- Calculator JSON config (validated by Schema Validator Agent)
- UI component library path (shared components from Section 3.3)
- Design system configuration (from Section 4)

## Process

### Step 1: Analyze Calculator Config
- Read calculator_meta (name, description)
- Read inputs array (count, types, validation rules)
- Read outputs (key vs advanced metrics split)
- Read warnings array
- Read tier_config (Free vs Pro behavior)
- Determine layout complexity (simple vs complex)

### Step 2: Generate Calculator Shell Component
Create main Calculator component:
- Component name: `{CalculatorSlugPascalCase}Calculator.tsx`
- Import shared UI components (from Section 4.1):
  - InputPanel
  - ResultsPanel
  - ScenarioTabs
  - WarningAlert
  - ExportButton
  - AIButton
- Apply layout from Section 4.1:
  - Left panel: inputs
  - Right panel: outputs
  - Top bar: scenario tabs (if Pro tier)
  - Bottom bar: actions (export, AI)

### Step 3: Generate Input Components
For each input in JSON config:

#### 3a. Determine Input Component Type
Based on field_type:
- `number` → `<NumberInput />`
- `currency` → `<CurrencyInput />` (with $ formatting)
- `percentage` → `<PercentageInput />` (with % formatting)
- `dropdown` → `<DropdownSelect />`
- `slider` → `<SliderInput />`

#### 3b. Apply Validation Rules
From validation object:
- min/max: Set HTML min/max attributes
- must_be_positive: Add client-side check
- custom_rule: Generate validation function

#### 3c. Add Input Metadata
- Label: field_name from JSON
- Placeholder: placeholder text
- Tooltip: Create `<Tooltip>` component with tooltip text
- Required indicator: Red asterisk if required=true
- Units display: Show units after input (e.g., "years", "%", "$")

#### 3d. Wire onChange Handlers
- Update state on change
- Trigger validation on blur
- Auto-calculate if all required inputs valid (Section 4.2.1)

### Step 4: Generate Calculation Wiring
- Create calculation function that:
  - Reads all input values from state
  - Calls formula library functions in order (from calculations array)
  - Stores intermediate results
  - Handles errors gracefully (try/catch)
  - Updates output state

### Step 5: Generate Output Components

#### 5a. Key Metrics (Always Visible)
For each metric in key_metrics:
- Create `<ResultCard>` component
- Display metric_name as card title
- Format value based on format field:
  - `currency`: `$X,XXX.XX`
  - `percentage`: `X.X%`
  - `number`: `X,XXX`
  - `ratio`: `X.XX`
- Apply decimals setting
- Add tooltip with explanation

#### 5b. Advanced Metrics (Pro-Gated)
For each metric in advanced_metrics:
- Create `<ResultCard>` with Pro gate
- Free tier:
  - Show card with blur overlay
  - Display lock icon
  - Show "Unlock with Pro" badge
  - Click → Upgrade modal (Section 6.3)
- Pro tier:
  - Show card normally
  - No lock icon

### Step 6: Generate Warning Components
For each warning in warnings array:
- Create warning evaluation logic (checks condition)
- If condition true, render `<WarningAlert>`:
  - severity → CSS class (info/warning/danger colors from Section 4.3.1)
  - message → Alert text
  - Icon based on severity (info icon, warning triangle, danger circle)
- Position below results (Section 4.1.5)

### Step 7: Generate Tier Gating Logic
Based on tier_config:
- Scenario tabs:
  - Free: Hide tabs, single scenario only
  - Pro: Show tabs, "Add Scenario" button enabled
- Advanced metrics visibility:
  - Free: Apply blur + lock + upgrade prompt
  - Pro: Show all metrics
- Generate upgrade trigger functions:
  - onClick handlers that show upgrade modal
  - Track trigger reason (for analytics, Section 7.2)

### Step 8: Generate Action Buttons

#### 8a. Export Button
- Always visible
- Free tier: Shows upgrade prompt on click
- Pro tier: Opens export options (PDF, CSV, Excel)
- Follows Section 6.3 upgrade flow

#### 8b. AI Explanation Button
- Only visible if ai_enabled=true in JSON
- Free/Pro tier: Shows AI tier upgrade prompt
- AI tier: Triggers AI request (follows Section 8)
- Shows loading state during AI request
- Displays result in modal

### Step 9: Apply Design System Standards
From Section 4.3:
- Colors:
  - Primary actions: brand blue (#0066CC or configured)
  - Success: green
  - Warning: yellow
  - Danger: red
- Typography:
  - Headings: defined font family and sizes
  - Body text: consistent size and line height
  - Numbers: monospace or tabular figures
- Spacing:
  - Consistent padding/margin (8px grid)
  - Card spacing
  - Input field spacing
- Accessibility:
  - ARIA labels on all inputs
  - ARIA roles on interactive elements
  - Focus indicators (keyboard navigation)
  - Contrast ratios meet WCAG 2.1 AA

### Step 10: Generate TypeScript Interfaces
Create types file (`types.ts`):
- Input interface (all input fields)
- Output interface (all output variables)
- Calculation result interface
- Component props interfaces
- Enum types for dropdowns

### Step 11: Add Analytics Tracking
Instrument with events from Section 7.2:
- calculator_viewed (on component mount)
- calculator_calculated (on calculation trigger)
- upgrade_prompt_shown (when gate hit)
- export_requested (on export button click)
- ai_narrative_requested (on AI button click)

## Output

Generated files in `calculators/built/{calculator-slug}/components/`:
```
├── Calculator.tsx (main component)
├── types.ts (TypeScript interfaces)
├── styles.module.css (component-specific styles)
└── README.md (component usage instructions)
```

### Example Generated Component Structure:
```typescript
// Calculator.tsx
import React, { useState, useEffect } from 'react';
import { InputPanel, ResultCard, WarningAlert, ExportButton, AIButton } from '@/components/shared';
import { calculateMonthlyPayment, calculateDSCR } from '@/formulas';
import { trackEvent } from '@/analytics';
import { BusinessLoanInputs, BusinessLoanOutputs } from './types';

export const BusinessLoanDSCRCalculator: React.FC = () => {
  // State management
  const [inputs, setInputs] = useState<BusinessLoanInputs>({...});
  const [outputs, setOutputs] = useState<BusinessLoanOutputs | null>(null);
  const [warnings, setWarnings] = useState<Warning[]>([]);
  
  // Track view
  useEffect(() => {
    trackEvent('calculator_viewed', {
      calculator_slug: 'business-loan-dscr',
      tier: userTier
    });
  }, []);
  
  // Calculate function
  const handleCalculate = () => {
    try {
      const monthlyPayment = calculateMonthlyPayment(
        inputs.loan_amount,
        inputs.interest_rate,
        inputs.term_years * 12
      );
      
      const dscr = calculateDSCR(
        inputs.annual_revenue - inputs.annual_expenses,
        monthlyPayment * 12
      );
      
      setOutputs({ monthlyPayment, dscr, ... });
      
      // Evaluate warnings
      if (dscr < 1.25) {
        setWarnings([{ severity: 'warning', message: '...' }]);
      }
      
      trackEvent('calculator_calculated', {...});
    } catch (error) {
      handleError(error);
    }
  };
  
  // Render
  return (
    <div className="calculator-container">
      <div className="input-panel">
        {/* Generated input components */}
      </div>
      <div className="results-panel">
        {/* Generated result cards */}
      </div>
      {warnings.map(w => <WarningAlert {...w} />)}
      <div className="actions">
        <ExportButton tier={userTier} />
        <AIButton tier={userTier} aiEnabled={true} />
      </div>
    </div>
  );
};
```

## Success Criteria
- All input components generated with correct types
- All validation rules applied
- All outputs formatted correctly
- Tier gating works (Free vs Pro behavior)
- Analytics events tracked
- Accessibility standards met (WCAG 2.1 AA)
- TypeScript compiles without errors
- Follows Section 4 UX standards exactly

## Quality Checks
- Run ESLint: No errors
- Run TypeScript compiler: No errors
- Check accessibility: WCAG 2.1 AA compliant
- Visual review: Matches design system
- Responsive test: Works on mobile/tablet/desktop

## Usage Example
```bash
# Generate UI for a calculator
claude-code "Use ui-generator-agent to generate UI components for business-loan-dscr calculator"
```

## Dependencies
- Section 4: UX design standards (complete reference)
- Section 6: Tier gating rules
- Section 7.2: Analytics events
- Section 12.2: JSON schema (for reading config)
- Schema Validator Agent: ensures valid input