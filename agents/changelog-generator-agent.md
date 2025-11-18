# Changelog Generator Agent

## Purpose
Automatically generate structured changelog entries from Git commits, version updates, and deployment history, maintaining a comprehensive record of all calculator and system changes.

## Input
- Git repository history
- Calculator version changes (from JSON configs)
- Deployment logs
- Date range (optional, defaults to latest changes)
- Format preference (markdown, JSON, HTML)

## Process

### Step 1: Parse Git Commit History

#### 1a. Fetch Commits Since Last Release
```bash
git log --since="2025-10-01" --oneline --no-merges
```

#### 1b. Categorize Commits by Type
Parse commit messages using conventional commit format:
- `feat:` → New Features
- `fix:` → Bug Fixes
- `perf:` → Performance Improvements
- `docs:` → Documentation Updates
- `style:` → UI/UX Changes
- `refactor:` → Code Refactoring
- `test:` → Test Updates
- `chore:` → Maintenance Tasks

Example commit:
```
feat(business-loan-dscr): add covenant headroom metric
fix(sba-7a): correct guarantee fee calculation for loans >$500k
perf(formula-library): optimize DSCR calculation by 30%
docs(api): update REST API documentation for v2 endpoints
```

#### 1c. Extract Calculator-Specific Changes
Group commits by calculator slug:
- business-loan-dscr: 3 commits
- sba-7a-analyzer: 2 commits
- formula-library: 1 commit
- ui-components: 2 commits

### Step 2: Detect Version Changes

#### 2a. Read Calculator JSON Configs
Compare current vs previous versions:
```
calculators/configs/business-loan-dscr.json
- Previous version: 2.0.0
- Current version: 2.1.0
- Version bump: MINOR (new features added)
```

#### 2b. Identify Breaking Changes
Scan commits for:
- `BREAKING CHANGE:` in commit body
- Major version bump (2.0.0 → 3.0.0)
- API endpoint changes
- Input/output schema changes

### Step 3: Categorize Changes by Impact

#### 3a. User-Facing Changes
Changes that affect end users:
- New calculators
- New features in existing calculators
- UI/UX improvements
- Bug fixes that resolve user-reported issues
- Performance improvements (faster load times)

#### 3b. Developer-Facing Changes
Changes that affect developers/integrators:
- API changes
- Formula library updates
- WordPress plugin updates
- Breaking changes requiring code updates

#### 3c. Internal Changes
Changes not visible to users:
- Code refactoring
- Test improvements
- Build process updates
- Infrastructure changes

### Step 4: Generate Changelog Entries by Calculator

#### 4a. Per-Calculator Changelog Format
```markdown
## Business Loan + DSCR Calculator v2.1.0 (2025-11-18)

### ✨ New Features
- Added covenant headroom metric showing cushion above lender minimum DSCR
- Added scenario comparison table (Pro tier only)
- Enabled AI explanations for DSCR interpretation (AI tier)

### 🐛 Bug Fixes
- Fixed rounding error in DSCR calculation for edge cases
- Corrected validation allowing zero interest rates
- Fixed PDF export formatting on mobile devices

### ⚡ Performance Improvements
- Reduced calculation time from 120ms to 85ms (29% faster)
- Optimized PDF generation, now completes in <2 seconds

### 📚 Documentation
- Updated FAQs with DSCR benchmark guidance
- Added video tutorial for using multiple scenarios
- Improved tooltip text for all input fields

### 🔧 Technical Changes
- Upgraded to formula library v1.1.0
- Migrated to TypeScript 5.0
- Added 5 new golden test scenarios

### ⚠️ Breaking Changes
None

---
```

#### 4b. System-Wide Changelog Format
```markdown
# SmartProfit Calculator Suite - Changelog

## Version 1.2.0 (2025-11-18)

### 🎉 New Calculators
- **Invoice Factoring Calculator** - Calculate effective APR and compare to line of credit
- **Line of Credit Utilization Calculator** - Analyze cost at different utilization levels

### ✨ Platform Features
- **Multi-scenario comparison view** - Compare up to 5 scenarios side-by-side (Pro tier)
- **Enhanced export templates** - New board-ready PDF layouts
- **AI narrative improvements** - More detailed scenario-specific recommendations

### 🐛 Bug Fixes
- Fixed export watermark not appearing on Free tier PDFs
- Corrected tier gating logic for B2B white-label customers
- Fixed mobile responsive issues on iPad mini
- Resolved intermittent AI timeout errors

### ⚡ Performance Improvements
- Reduced average page load time from 1.8s to 1.2s (33% faster)
- Optimized formula library execution (15-30% faster calculations)
- Implemented CDN caching for static assets
- Reduced bundle size by 200KB through code splitting

### 📊 Analytics & Monitoring
- Added conversion funnel tracking for Free → Pro upgrades
- Implemented error tracking with Sentry integration
- Added performance monitoring dashboard (Section 1.8.3)

### 🔐 Security & Compliance
- Updated data retention policies per GDPR requirements
- Added tenant-level AI opt-out controls (B2B)
- Implemented stricter input validation to prevent XSS

### 📚 Documentation
- Published API v2 documentation at docs.smartprofit.com/api
- Added WordPress embed guide with video tutorials
- Updated calculator user guides with new screenshots

### 🛠️ Developer Experience
- Released Calculator Builder Agent v1.0
- Published JSON schema specification (Section 12.2)
- Added deployment pipeline automation (10 min JSON → production)

### ⚠️ Breaking Changes
- **API v1 deprecated** - Will be sunset on 2026-06-01. Migrate to API v2.
  - Migration guide: docs.smartprofit.com/api/v1-to-v2-migration
- **Old WordPress shortcode format deprecated** - Use new format: `[smartprofit_calculator slug="..."]`
  - Old format still works but will be removed in v2.0.0

### 🔄 Updated Calculators
- **Business Loan + DSCR Calculator**: v2.0.0 → v2.1.0 (see above)
- **SBA 7(a) Loan Analyzer**: v1.5.2 → v1.6.0 (updated fee structure)
- **Cash Runway Calculator**: v1.2.1 → v1.2.2 (bug fixes only)

### 📦 Dependencies
- Upgraded React 18.2 → 18.3
- Upgraded TypeScript 4.9 → 5.0
- Upgraded formula-library 1.0.0 → 1.1.0

### 👥 Contributors
- 8 commits by engineering team
- 2 bug reports from community users (thank you!)
- 1 feature request implemented from feedback

---
```

### Step 5: Generate Version Summary
```markdown
## Version 1.2.0 at a Glance

**Released:** November 18, 2025  
**Type:** Minor Release (new features, backward compatible)

**Highlights:**
- 2 new calculators (Invoice Factoring, Line of Credit)
- Multi-scenario comparison (Pro tier feature)
- 33% faster page loads
- Enhanced AI narratives
- 15 bug fixes

**Impact:**
- **Users:** New features, better performance, bug fixes
- **Developers:** API v1 deprecated (6 month sunset), WordPress shortcode updated
- **System:** More reliable, faster, better monitoring

**Upgrade Priority:** Recommended for all users  
**Estimated Downtime:** None (rolling deployment)

**Next Release:** Version 1.3.0 planned for December 15, 2025
```

### Step 6: Generate Release Notes (User-Friendly Version)

Transform technical changelog into user-friendly announcement:
```markdown
# What's New in SmartProfit Calculators - November 2025

## 🚀 Two New Calculators

We've added two powerful new financial calculators to help you make better business decisions:

**Invoice Factoring Calculator**  
Wondering if invoice factoring is worth the cost? This calculator compares factoring fees to alternative financing and shows you the true effective APR.

**Line of Credit Calculator**  
Optimize your line of credit usage. See how utilization rates affect your costs and find the sweet spot for your working capital needs.

## ⚡ Faster, Smoother, Better

- **33% faster loading** - Calculators now load in just over 1 second
- **Smoother calculations** - Results appear even faster with our optimized formula engine
- **Better mobile experience** - Improved layouts for tablets and phones

## 🎯 New Pro Features

**Multi-Scenario Comparison** (Pro tier)  
Compare up to 5 different scenarios side-by-side. Perfect for evaluating multiple loan offers or running what-if analyses.

**Enhanced PDF Exports**  
New professional templates designed for board presentations and lender submissions.

## 🤖 Smarter AI Insights

Our AI explanations are now more detailed and scenario-specific. Ask "What does this mean?" and get personalized insights based on your exact numbers.

## 🐛 Bug Fixes

We fixed 15 bugs including:
- Export watermarks now appear correctly on Free tier
- Mobile display issues on iPad resolved
- AI explanations no longer time out

## 📖 Learn More

- [Watch video tutorials](https://smartprofit.com/tutorials)
- [Read full changelog](https://smartprofit.com/changelog)
- [Contact support](mailto:support@smartprofit.com)

## Coming Next Month

December release preview:
- Equipment financing calculator
- Breakeven analysis improvements
- More export format options

---

*Have feedback or feature requests? We'd love to hear from you at [feedback@smartprofit.com](mailto:feedback@smartprofit.com)*
```

### Step 7: Generate Changelog Formats

#### 7a. Markdown Format (CHANGELOG.md)
Standard markdown file in repo root

#### 7b. JSON Format (for API consumption)
```json
{
  "version": "1.2.0",
  "release_date": "2025-11-18",
  "release_type": "minor",
  "breaking_changes": false,
  "changes": {
    "new_features": [
      {
        "title": "Invoice Factoring Calculator",
        "description": "Calculate effective APR and compare to line of credit",
        "tier": "free",
        "impact": "user-facing"
      }
    ],
    "bug_fixes": [
      {
        "title": "Fixed export watermark issue",
        "description": "Watermarks now appear correctly on Free tier PDFs",
        "severity": "medium",
        "calculator": "all"
      }
    ],
    "performance_improvements": [
      {
        "title": "Reduced page load time",
        "description": "Average load time reduced from 1.8s to 1.2s",
        "improvement_percent": 33
      }
    ]
  },
  "deprecated": [
    {
      "item": "API v1",
      "sunset_date": "2026-06-01",
      "migration_guide": "https://docs.smartprofit.com/api/v1-to-v2-migration"
    }
  ],
  "contributors": 8,
  "commits": 47
}
```

#### 7c. HTML Format (for website display)
```html
<div class="changelog-entry">
  <h2>Version 1.2.0 <span class="badge badge-success">Latest</span></h2>
  <p class="release-date">Released November 18, 2025</p>
  
  <div class="section new-features">
    <h3>✨ New Features</h3>
    <ul>
      <li><strong>Invoice Factoring Calculator</strong> - Calculate effective APR and compare to line of credit</li>
      <li><strong>Multi-scenario comparison</strong> - Compare up to 5 scenarios side-by-side (Pro tier)</li>
    </ul>
  </div>
  
  <div class="section bug-fixes">
    <h3>🐛 Bug Fixes</h3>
    <ul>
      <li>Fixed export watermark not appearing on Free tier PDFs</li>
      <li>Corrected tier gating logic for B2B customers</li>
    </ul>
  </div>
  
  <a href="/changelog/1.2.0" class="btn btn-link">View Full Changelog →</a>
</div>
```

### Step 8: Generate Deprecation Warnings

For breaking changes or deprecations:
```markdown
## ⚠️ Deprecation Notice

### API v1 Sunset

**Effective Date:** June 1, 2026 (6 months from release)

**What's Changing:**
API v1 endpoints will be shut down and will return 410 Gone status.

**Who's Affected:**
- Developers using direct API integration
- Third-party apps using old API
- Custom WordPress plugins using v1 endpoints

**What You Need to Do:**
1. Review your API usage: Check which endpoints you're calling
2. Read migration guide: https://docs.smartprofit.com/api/v1-to-v2-migration
3. Update to API v2: Most endpoints have direct equivalents
4. Test thoroughly: Use staging environment first
5. Deploy updated code: Before June 1, 2026

**Support:**
Contact api-support@smartprofit.com for migration assistance.
```

### Step 9: Tag and Link Changelog Entries
```markdown
## Tags

#new-calculator #performance #bug-fix #breaking-change #api #wordpress

## Related

- [API v2 Documentation](https://docs.smartprofit.com/api/v2)
- [Migration Guide: API v1 → v2](https://docs.smartprofit.com/api/v1-to-v2-migration)
- [Video: What's New in November 2025](https://youtube.com/watch?v=...)
- [Blog Post: Introducing Multi-Scenario Comparison](https://blog.smartprofit.com/multi-scenario)

## Resources

- Download release: https://github.com/smartprofit/calculators/releases/tag/v1.2.0
- Docker image: `smartprofit/calculators:1.2.0`
- NPM package: `@smartprofit/calculator-sdk@1.2.0`
```

### Step 10: Notify Stakeholders

Generate notification content for different channels:

#### 10a. Email to Users
```
Subject: New Calculators + Faster Performance 🚀

Hi [Name],

We just released SmartProfit Calculators v1.2.0 with some exciting updates:

✨ 2 New Calculators
- Invoice Factoring Calculator
- Line of Credit Calculator

⚡ 33% Faster
Pages load in just over 1 second now.

🎯 New Pro Feature
Compare up to 5 scenarios side-by-side.

[See What's New] → smartprofit.com/whats-new

Questions? Reply to this email.

The SmartProfit Team
```

#### 10b. Slack Notification (Internal Team)
```
🎉 Version 1.2.0 Deployed to Production

✅ 2 new calculators live
✅ 15 bug fixes deployed
✅ Performance improved 33%
⚠️ API v1 deprecated (6 month sunset)

📊 Deployment Stats:
- Build time: 8m 23s
- Tests passed: 287/287
- Zero downtime deployment

🔗 Full changelog: https://smartprofit.com/changelog/1.2.0
📈 Monitor: https://dashboard.smartprofit.com
```

#### 10c. Twitter/Social Media
```
🚀 SmartProfit v1.2.0 is live!

✨ 2 new business calculators
⚡ 33% faster performance  
🎯 Multi-scenario comparison (Pro)

Try them free: smartprofit.com

#fintech #businesstools #smallbusiness
```

## Output

### Generated Files:

**Main Changelog:**
- `CHANGELOG.md` (cumulative, all versions)
- `docs/changelog/1.2.0.md` (version-specific detailed)
- `docs/releases/1.2.0-release-notes.md` (user-friendly)

**Structured Data:**
- `changelog/1.2.0.json` (machine-readable)
- `changelog/1.2.0.html` (website display)

**Notifications:**
- `notifications/1.2.0-email.txt`
- `notifications/1.2.0-slack.txt`
- `notifications/1.2.0-social.txt`

### Success Message:
```
✅ Changelog Generated

Version: 1.2.0
Release Date: 2025-11-18
Type: Minor Release

Summary:
- 2 new calculators
- 8 new features
- 15 bug fixes
- 5 performance improvements
- 1 breaking change (API v1 deprecated)

Files Generated:
✓ CHANGELOG.md updated
✓ docs/changelog/1.2.0.md created
✓ docs/releases/1.2.0-release-notes.md created
✓ changelog/1.2.0.json created
✓ changelog/1.2.0.html created
✓ Email notification draft ready
✓ Slack notification ready
✓ Social media post ready

Commits Analyzed: 47
Contributors: 8
Calculators Updated: 3

Next Steps:
1. Review generated changelog for accuracy
2. Send email notification to users
3. Post announcement on social media
4. Update website /changelog page
5. Tag release in Git: git tag v1.2.0
```

## Success Criteria
- All commits categorized correctly
- Breaking changes clearly marked
- Version numbers accurate
- User-facing vs technical changes separated
- Deprecation warnings included
- Links and references valid
- Notification content ready for distribution

## Quality Checks
- Changelog follows Keep a Changelog format
- Semantic versioning applied correctly
- All calculator version changes documented
- Breaking changes have migration guides
- No duplicate entries
- Proper markdown formatting
- Links all valid (no 404s)

## Usage Example
```bash
# Generate changelog for latest release
claude-code "Use changelog-generator-agent to generate changelog for version 1.2.0"

# Generate changelog since specific date
claude-code "Use changelog-generator-agent to generate changelog for all changes since 2025-10-01"

# Regenerate full changelog (all versions)
claude-code "Use changelog-generator-agent to regenerate complete CHANGELOG.md from Git history"

# Generate release notes only (user-friendly)
claude-code "Use changelog-generator-agent to generate release notes for version 1.2.0"
```

## Dependencies
- Git repository with commit history
- Calculator JSON configs (for version tracking)
- Deployment logs (Section 12.4)
- Section 10.3: Release process and versioning standards
- Conventional Commits format for commit messages