# Export Builder Agent

## Purpose
Generate PDF, CSV, and Excel export templates and logic from calculator configuration and results.

## Input
- Calculator JSON config (export_config section)
- Calculator results (inputs + outputs from a calculation)
- User tier (Free, Pro, AI, B2B)
- Export format requested (PDF, CSV, Excel)

## Process

### Step 1: Read Export Configuration
From JSON export_config:
- pdf_sections: array of sections to include in PDF
- csv_columns: array of columns for CSV
- include_charts: boolean (whether to generate charts)
- Additional metadata:
  - Calculator version
  - Formula library version
  - Timestamp
  - Disclaimers

### Step 2: Determine Tier-Specific Behavior
Based on user tier (Section 6.1):
- **Free tier:**
  - PDF: Add watermark "Generated with Free Tier - Upgrade for clean exports"
  - PDF footer: "plusonecapital.com/upgrade"
  - CSV: Add header row "# Generated with Free Tier"
- **Pro tier:**
  - PDF: Clean, no watermark
  - PDF: Professional branding
  - CSV: Clean, professional header
- **AI tier:**
  - Same as Pro
  - If AI narrative generated, include in PDF
- **B2B:**
  - Custom branding from tenant config
  - No watermark
  - Custom disclaimers if configured

### Step 3: Generate PDF Export

#### 3a. Create PDF Structure
Based on pdf_sections array (or defaults):
- **Title Page:**
  - Calculator name
  - Scenario name (if applicable)
  - Generation date and time
  - Prepared by (if user logged in)
  - Version stamps

- **Summary Section:**
  - Key inputs (table format)
  - Key outputs (highlight box)
  - Warnings (if any, colored boxes)

- **Detailed Results:**
  - All outputs table (key + advanced if Pro)
  - Formatted with proper units
  - Visual separators

- **Charts/Visualizations (if include_charts=true):**
  - Generate chart images
  - Embed in PDF
  - Add captions

- **AI Narrative (if AI tier and narrative generated):**
  - Section titled "AI Analysis"
  - Narrative text with disclaimer
  - Disclaimer: "This analysis is for informational purposes only..."

- **Methodology Section:**
  - Formulas used (references)
  - Calculation steps
  - Assumptions

- **Footer on Every Page:**
  - Page numbers
  - Calculator version + formula version
  - Generated timestamp
  - Disclaimer text (Section 5.3.2)
  - Watermark (if Free tier)

#### 3b. Apply PDF Styling
From Section 4.3:
- Brand colors
- Typography (fonts, sizes, weights)
- Layout grid (margins, padding)
- Professional appearance (board-ready)

#### 3c. Generate PDF File
- Use PDF library (e.g., PDFKit, jsPDF, or server-side)
- Render all sections
- Optimize file size
- Target: Generation time < 3 seconds (Section 1.6 SLA)

### Step 4: Generate CSV Export

#### 4a. Define CSV Structure
From csv_columns or defaults:
- Column headers (from metric names)
- Data rows (one row per scenario, or structured differently)

#### 4b. Format CSV Data
- Currency: Remove $ symbol, use plain numbers (12345.67)
- Percentages: Use decimal format (0.075 for 7.5%)
- Dates: ISO format (YYYY-MM-DD)
- Include units in column headers (e.g., "Monthly Payment (USD)")

#### 4c. Add CSV Metadata
Header rows (commented with #):
```csv
# Calculator: Business Loan + DSCR Intelligence Calculator
# Version: v2.1.0
# Formula Library: v1.0.0
# Generated: 2025-11-18T18:30:00Z
# Tier: Pro
# Disclaimer: This is decision support only, not financial advice
#
Input,Value,Unit
Loan Amount,250000,USD
Interest Rate,7.5,%
...
```

### Step 5: Generate Excel Export

#### 5a. Create Excel Workbook Structure
Multiple sheets:
- **Summary Sheet:**
  - Calculator info (name, version, date)
  - Key results highlighted
  - Warnings section
  
- **Inputs Sheet:**
  - All input values
  - Descriptions from tooltips
  - Units clearly labeled

- **Outputs Sheet:**
  - All outputs (key + advanced)
  - Formatted with Excel number formats
  - Color coding for warnings

- **Charts Sheet (if include_charts=true):**
  - Embedded charts
  - Chart data tables

- **Methodology Sheet:**
  - Formulas used
  - Calculation breakdown
  - References

#### 5b. Apply Excel Formatting
- Currency format: $#,##0.00
- Percentage format: 0.0%
- Number format: #,##0.00
- Bold headers
- Freeze top row
- Auto-column width
- Conditional formatting for warnings (red if DSCR < 1.25)

#### 5c. Add Excel Metadata
Workbook properties:
- Title: Calculator name
- Author: "SmartProfit Calculator Suite"
- Subject: Calculator description
- Keywords: Category tags
- Comments: Version info, disclaimers

### Step 6: Include Version Stamps
In all export formats:
- Calculator slug and version (e.g., "business-loan-dscr v2.1.0")
- Formula library version (e.g., "formulas v1.0.0")
- Export generation timestamp (ISO 8601)
- User tier (Free, Pro, AI, B2B)
- Prepared by (user email or "Anonymous" if not logged in)
- Reviewed by (if applicable, from scenario metadata)

### Step 7: Add Disclaimers
From Section 5.3.2, include in all exports:
- "Decision support tool only" disclaimer
- "Not a substitute for professional advice" disclaimer
- AI disclaimer (if AI narrative included)
- Custom disclaimers (if B2B tenant configured)

Position in exports:
- PDF: Footer on every page + dedicated section
- CSV: Header comment rows
- Excel: Separate "Disclaimers" sheet

### Step 8: Handle Multi-Scenario Exports
If exporting multiple scenarios (Pro tier):
- PDF: Section per scenario, comparison table at end
- CSV: One row per scenario
- Excel: One sheet per scenario + summary comparison sheet

### Step 9: Generate Download Link
- Save export file to temp storage
- Generate download URL (expires in 24 hours for anonymous, 90 days for Pro)
- Return URL to frontend
- Track export event (Section 7.2: export_generated, export_downloaded)

### Step 10: Cleanup and Retention
Based on Section 5.4:
- Anonymous users: Delete export after 24 hours
- Pro users: Keep for 90 days
- Store metadata only after file deletion (for analytics)

## Output

### Success Response:
```json
{
  "status": "success",
  "export_id": "exp_abc123xyz",
  "format": "pdf",
  "download_url": "https://exports.smartprofit.com/exp_abc123xyz.pdf",
  "expires_at": "2025-11-19T18:30:00Z",
  "file_size_bytes": 245760,
  "generation_time_ms": 2340,
  "metadata": {
    "calculator_slug": "business-loan-dscr",
    "calculator_version": "v2.1.0",
    "formula_version": "v1.0.0",
    "scenario_count": 1,
    "tier": "pro"
  }
}
```

### Error Response:
```json
{
  "status": "error",
  "error_code": "EXPORT_GENERATION_FAILED",
  "error_message": "PDF generation timed out after 6 seconds",
  "details": {
    "calculator_slug": "business-loan-dscr",
    "format": "pdf",
    "scenario_count": 3
  }
}
```

## Success Criteria
- Export generates successfully within SLA (p95 < 3 seconds)
- All data included and formatted correctly
- Tier-specific behavior applied (watermarks, disclaimers)
- Version stamps present
- Download link works and expires correctly
- File size reasonable (PDF < 5MB typical)

## Quality Checks
- Visual review of PDF (looks professional, board-ready)
- CSV opens correctly in Excel/Google Sheets
- Excel file opens without errors, formulas work
- Watermark visible on Free tier exports
- Disclaimers present and readable
- All numbers formatted with correct decimals and units

## Performance Optimization
- Cache common templates
- Generate charts asynchronously if slow
- Use streaming for large exports
- Queue large exports (>3 scenarios) for async processing

## Usage Example
```bash
# Generate PDF export
claude-code "Use export-builder-agent to generate PDF export for business-loan-dscr results"

# Generate all formats
claude-code "Use export-builder-agent to generate PDF, CSV, and Excel exports for business-loan-dscr results"

# Generate multi-scenario comparison
claude-code "Use export-builder-agent to generate comparison PDF for 3 business-loan-dscr scenarios"
```

## Dependencies
- Section 1.6: Performance SLAs (3 second target)
- Section 4.3: Design system (PDF styling)
- Section 5.3.2: Disclaimer text
- Section 5.4: Retention policies
- Section 6.1: Tier-specific behavior
- Section 7.2: Analytics events
- Section 12.2: JSON schema (export_config)