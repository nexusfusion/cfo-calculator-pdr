# SECTION 4: SHARED UX SYSTEM & DESIGN STANDARDS - BUILD INSTRUCTIONS

## Overview
You are building Section 4 of the CFO Business Intelligence Calculator Suite PDR. This section defines the consistent UX patterns, visual design, and content standards that apply to ALL calculators in the suite.

## Context
- Reference Section 1 for: target users (1.2), tier behavior (1.5)
- Reference Section 2 for: calculator categories, tier behavior patterns
- All calculators must feel like one cohesive product
- Must support Free/Pro/AI tier gating with clear visual indicators

## Your Task
Create 4 markdown files:
1. `4.1-common-layout.md`
2. `4.2-interaction-patterns.md`
3. `4.3-visual-language.md`
4. `4.4-content-standards.md`

---

## 4.1 COMMON LAYOUT PATTERN

**File:** `4.1-common-layout.md`

**Content:**
Define the standard calculator layout that all calculators follow.

**Main layout sections:**
1. **Left Panel: Inputs**
   - Input field types (text, number, percentage, currency, dropdown)
   - Validation UI (real-time errors, warnings)
   - Tooltips and help text positioning
   - Required vs optional field indicators
   
2. **Right Panel: Key Results**
   - Key metrics card (always visible)
   - Advanced metrics card (Pro-gated with lock icon and upgrade prompt)
   - Charts and visualizations
   - Warning/alert boxes

3. **Top Bar: Scenario Tabs**
   - Free tier: single scenario (no tabs)
   - Pro tier: multiple scenario tabs with naming
   - Add/delete scenario buttons

4. **Bottom Bar: Actions**
   - Export button (tier-gated)
   - AI explanation button (tier-gated)
   - Share/save buttons (tier-gated)

5. **Warnings and Alerts Panel**
   - Severity levels: info (blue), warning (yellow), danger (red)
   - Placement (below results or inline)
   - Dismissibility

**Include:** ASCII layout diagram showing these 5 areas

**Format:** Section per layout area. 3-4 pages.

---

## 4.2 INTERACTION PATTERNS

**File:** `4.2-interaction-patterns.md`

**Content:**
Define how users interact with calculators.

**Patterns to define:**
1. **Input Behavior**
   - Real-time validation (when to show errors)
   - Auto-calculation triggers (on blur, on change, manual button)
   - Input masking and formatting (currency with $, percentages with %)

2. **Scenario Management**
   - Creating new scenarios (Free: blocked with upgrade prompt, Pro: allowed)
   - Switching between scenarios (tab interface)
   - Comparing scenarios side-by-side (Pro only)
   - Deleting/archiving scenarios

3. **Tier Gating UI Patterns**
   - Locked feature indicators (lock icons, blur overlays, "Pro" badges)
   - Upgrade prompt modals (when triggered, what they say)
   - Feature teaser UI (showing Pro features to Free users with CTAs)

4. **Empty States and Sample Scenarios**
   - First-time user experience (sample pre-filled scenario)
   - "Try it" vs "Start from scratch" options
   - Calculator-specific sample scenarios

5. **Loading and Progress States**
   - Calculation in progress (spinner, disable inputs)
   - Export generation (progress bar, estimated time)
   - AI narrative generation (typing indicator, timeout handling)

**Format:** Section per pattern type. 4-5 pages.

---

## 4.3 VISUAL LANGUAGE AND CONSISTENCY

**File:** `4.3-visual-language.md`

**Content:**
Define visual design standards.

**Design system elements:**
1. **Color System**
   - Primary brand colors (e.g., blue for actions, green for success)
   - Semantic colors (red=danger, yellow=warning, blue=info, green=success)
   - Tier-specific colors (Free=gray, Pro=blue, AI=purple/gradient)

2. **Typography**
   - Font families (headings, body text, code/numbers)
   - Font sizes and hierarchy (H1, H2, H3, body, small)
   - Number formatting rules:
     - Currency: $X,XXX.XX (always 2 decimals)
     - Percentages: X.X% (1 decimal unless specified)
     - Large numbers: use commas, no abbreviations unless > $1M

3. **Spacing and Layout Grid**
   - Grid system (8px or 4px base unit)
   - Padding/margin standards
   - Component spacing rules

4. **Icons and Visual Elements**
   - Icon library (lock, info, warning, check, etc.)
   - Status indicators (badges, dots, checkmarks)
   - Chart styles (colors, axes labels, legends)

5. **Accessibility Standards**
   - WCAG 2.1 AA contrast minimums (4.5:1 for text)
   - Keyboard navigation (tab order, focus indicators)
   - Screen reader support (ARIA labels, roles, live regions)

**Format:** Section per design element. 3-4 pages.

---

## 4.4 CONTENT STANDARDS

**File:** `4.4-content-standards.md`

**Content:**
Define writing and content standards.

**Standards to define:**
1. **Naming Conventions**
   - Input field labels (use plain language, avoid jargon)
   - Output metric names (consistent across calculators)
   - Scenario naming (user-friendly defaults like "Scenario 1" vs "scenario_001")

2. **Tooltip Style and Guidelines**
   - Length limits (1-2 sentences, ~50 words max)
   - Tone (helpful, not condescending)
   - When to use tooltips vs inline help vs help links
   - Examples of good vs bad tooltips

3. **Warning and Alert Messaging**
   - Warning message templates:
     - Info: "Your [metric] is [value]. This is [context]."
     - Warning: "Your [metric] is [value], which is [threshold]. Consider [action]."
     - Danger: "Your [metric] is [value]. This may [risk]. You should [action]."
   - Tone (direct, actionable, not alarmist)
   - When to show warnings vs when to block

4. **Error Messages**
   - Validation error format: "[Field] must be [requirement]"
   - System error format: "Unable to [action]. Please [resolution]."
   - Never show technical errors to users

**Include:** Table of example messages (good vs bad)

**Format:** Section per content type. 2-3 pages.

---

## Overall Guidelines

**Tone:** Design-focused but implementable. Writing for designers and frontend developers.

**Formatting:**
- Markdown headers: ##, ###, ####
- Tables for comparisons and examples
- Color hex codes where relevant (#0066CC)
- Bullet lists for guidelines

**Consistency:**
- Reference Section 1.5 for tier behavior
- Reference Section 2.4 for tier patterns by category
- Use consistent terminology (Free tier, Pro tier, AI add-on)

**Length:** 12-15 pages total across 4 files

**Output Files:**
- 4.1-common-layout.md
- 4.2-interaction-patterns.md
- 4.3-visual-language.md
- 4.4-content-standards.md

Each file starts with # header matching title.