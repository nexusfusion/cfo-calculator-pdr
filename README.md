# CFO Business Intelligence Calculator Suite - Product Design & Requirements Document (PDR)

## Project Overview
Complete PDR for a suite of CFO-grade business calculators covering lending, cash flow, profitability, valuation, and planning intelligence.

## Document Structure
This PDR is organized into 11 major sections, each broken into subsections stored as individual markdown files.

### Sections
1. **Vision & Goals** - Product vision, users, CFO-grade definition, monetization, quality goals, internal operations
2. **Product Scope** - Calculator categories, MVP set, roadmap, tier behavior
3. **Platform Architecture & Tech Stack** - Architecture layers, technology choices, module boundaries, performance
4. **Shared UX System & Design Standards** - Layout patterns, interaction patterns, visual language, content standards
5. **Data, Security, and Compliance** - Data models, security baselines, compliance posture, retention policies
6. **Tiering, Pricing, and Packaging** - Tier definitions, feature flagging, upgrade flows
7. **Analytics & Observability** - Event taxonomy, standard events, dashboards, error monitoring
8. **AI Usage & Prompting Framework** - AI roles, prompt guidelines, redaction, cost management
9. **Implementation Plan & Milestones** - Milestone breakdown, workstream mapping, risks
10. **Testing, QA, and Release Management** - Test strategy, golden scenarios, release process
11. **Individual Calculator PDRs** - Detailed specs for each of the 8 MVP calculators

## How to Use This Repository

### Building Sections with Claude Code
Each section has an `_instructions.md` file that tells Claude Code what to build.

Example:
```bash
# In VS Code terminal
cd sections/01-vision-and-goals
# Review _instructions.md, then use Claude Code to build the subsection files
```

### Agents
The `agents/` directory contains reusable agent instructions for:
- Building sections from instructions
- Checking consistency across sections
- Compiling the final PDR document

### Reference Files
- `reference/existing-pdr-sections-1-2.md` - Your already-completed sections
- `reference/terminology-glossary.md` - Consistent terminology
- `reference/cross-reference-index.md` - How sections link together

### Output
Final compiled PDR will be in `output/complete-pdr.md`

## Current Status
- [x] Section 1: Vision & Goals (8 subsections, ~28 pages) ✅
- [x] Section 2: Product Scope (5 subsections, ~20 pages) ✅
- [x] Section 3: Platform Architecture & Tech Stack (4 subsections, ~17 pages) ✅
- [x] Section 4: Shared UX System & Design Standards (4 subsections, ~14 pages) ✅
- [x] Section 5: Data, Security, and Compliance (4 subsections, ~17 pages) ✅
- [x] Section 6: Tiering, Pricing, and Packaging (3 subsections, ~12 pages) ✅
- [x] Section 7: Analytics & Observability (4 subsections, ~17 pages) ✅
- [x] Section 8: AI Usage & Prompting Framework (4 subsections, ~15 pages) ✅
- [x] Section 9: Implementation Plan & Milestones (3 subsections, ~13 pages) ✅
- [x] Section 10: Testing, QA, and Release Management (4 subsections, ~16 pages) ✅
- [x] Section 11: Individual Calculator PDRs (9 files: template + 8 calculators, ~50 pages) ✅
- [x] Section 12: JSON Assembly Line System (5 subsections, ~28 pages) ✅

**🎉 COMPLETE: 12 of 12 sections (100%)**
**Total Pages: ~247 pages**
**Last Updated: 2025-11-18 6:08 PM**
**Status: COMPLETE - Ready for 450+ calculator implementation via JSON Assembly Line**

## Key Features
- 8 MVP calculator specifications ready to build
- JSON Assembly Line system for scaling to 450+ calculators
- 10 internal dashboards for business operations
- Complete architecture, security, compliance, and testing frameworks
PDR Version: 1.0-draft
Last Updated: 2025-11-17