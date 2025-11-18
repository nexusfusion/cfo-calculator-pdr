# Deployment Pipeline Agent

## Purpose
Orchestrate the automated deployment of calculators from JSON configuration to live production, following the process defined in Section 12.4.

## Input
- Calculator JSON config file path (e.g., `calculators/configs/new-calculator.json`)
- Target environment (staging, production)
- Deployment mode (standard, canary, rollback)

## Process

### Step 1: Pre-Deployment Validation

#### 1a. Schema Validation
- Call Schema Validator Agent
- Verify JSON config is valid
- Check all referenced formulas exist
- Verify tier_config is valid
- **If fails:** Stop deployment, output errors

#### 1b. Golden Scenario Validation (if scenarios provided)
- Call Golden Scenario Tester Agent
- Run all golden scenarios for this calculator
- Check all outputs within tolerance
- **If fails:** Stop deployment, output failures

#### 1c. Security Scan
- Check JSON for suspicious patterns:
  - No external URLs in formulas
  - No eval() or dangerous functions
  - No hardcoded credentials
- **If fails:** Flag for security review, stop deployment

#### 1d. Version Check
- Read calculator_version from JSON
- Check if version already deployed
- If version exists: Error (must increment version)
- If new version: Proceed

### Step 2: Build Calculator

#### 2a. Generate UI Components
- Call UI Generator Agent
- Generate React/TypeScript components
- Output to `calculators/built/{calculator-slug}/`
- **If fails:** Stop deployment, output build errors

#### 2b. Generate Export Templates
- Call Export Builder Agent template generation
- Create PDF/CSV/Excel templates
- Output to `calculators/built/{calculator-slug}/exports/`
- **If fails:** Warning (exports may not work), proceed with caution

#### 2c. Wire Formula Library
- Link calculation steps to formula functions
- Generate calculation execution logic
- Add error handling for each formula call
- **If fails:** Stop deployment, output wiring errors

#### 2d. Apply Tier Gating
- Read tier_config from JSON
- Generate tier check logic
- Wire upgrade prompts
- Add analytics tracking
- **If fails:** Stop deployment, tier gating required

#### 2e. Compile TypeScript
- Run TypeScript compiler
- Check for type errors
- Generate JavaScript bundles
- **If fails:** Stop deployment, output TypeScript errors

#### 2f. Run Tests
- Unit tests for calculation logic
- Component tests for UI
- Integration tests for full calculator
- **If fails:** Stop deployment, output test failures

### Step 3: Staging Deployment

#### 3a. Deploy to Staging Environment
- Build Docker container (if applicable)
- Deploy to staging server
- Update routing to include new calculator
- Generate staging URL

#### 3b. Smoke Test in Staging
- Load calculator in staging
- Run one sample calculation
- Verify output correct
- Test export generation
- Test AI integration (if enabled)
- **If fails:** Stop deployment, output smoke test errors

#### 3c. Staging QA Review (Manual Step)
- Notify team: "Calculator X ready for QA in staging"
- Wait for manual approval or 24 hours timeout
- **If rejected:** Stop deployment, return to development
- **If approved:** Proceed to production

### Step 4: Production Deployment

#### 4a. Determine Deployment Strategy
Based on deployment mode:
- **Standard (default):** Deploy to all users immediately
- **Canary:** Deploy to 10% of users, monitor, then 50%, then 100%
- **Rollback:** Revert to previous version

#### 4b. Create Deployment Backup
- Snapshot current production state
- Store previous calculator version
- Enable one-click rollback if needed

#### 4c. Deploy to Production
- Update production build
- Deploy new calculator files
- Update routing/load balancer
- Clear CDN cache for calculator assets

#### 4d. Generate WordPress Shortcode
- Create shortcode: `[smartprofit_calculator slug="business-loan-dscr"]`
- Generate embed documentation
- Output shortcode to deployment notes
- Add to WordPress plugin registry

#### 4e. Update Calculator Registry
- Add new calculator to registry database
- Include metadata:
  - slug, name, version
  - category, business_role
  - created_at, deployed_at
  - formula_library_version
- Make available to Calculator Management Dashboard (Section 1.8.8)

### Step 5: Post-Deployment Verification

#### 5a. Health Check
- Hit calculator endpoint: `GET /api/calculators/{slug}/health`
- Expected response: `{"status": "healthy", "version": "X.Y.Z"}`
- **If fails:** Immediate rollback

#### 5b. Functional Smoke Test
- Run automated test:
  - Load calculator in production
  - Execute one calculation with known inputs
  - Verify output matches expected
  - Generate one export
  - Test AI call (if enabled)
- **If fails:** Immediate rollback

#### 5c. Performance Check
- Monitor first 100 calculations
- Check p95 latency < 150ms (Section 1.6 SLA)
- Check export time < 3s
- **If fails:** Alert team, may need rollback

#### 5d. Error Rate Monitoring (First Hour)
- Track error rate every 5 minutes
- Alert if error rate > 1%
- Alert if error rate > 5% (critical, automatic rollback)
- Track errors by type (calculation, export, AI)

### Step 6: Canary Deployment (if mode=canary)

#### 6a. Deploy to 10% Traffic
- Route 10% of users to new calculator version
- 90% still on previous version (if exists) or baseline
- Monitor for 2 hours

#### 6b. Monitor Canary Metrics
- Error rate (compare to baseline)
- Latency (compare to baseline)
- Conversion rate (Free → Pro)
- User feedback (support tickets)
- **If metrics degrade > 20%:** Rollback canary

#### 6c. Expand to 50% Traffic
- If canary successful, route 50% traffic
- Monitor for 2 hours
- **If metrics degrade:** Rollback

#### 6d. Deploy to 100% Traffic
- If 50% successful, route all traffic
- Monitor for 24 hours
- Mark deployment as complete

### Step 7: Rollback (if mode=rollback or auto-triggered)

#### 7a. Detect Rollback Trigger
Automatic rollback if:
- Error rate > 5% in 5 minutes
- Smoke test fails
- Manual rollback requested

#### 7b. Execute Rollback
- Restore previous calculator version from backup
- Update routing to previous version
- Clear CDN cache
- Takes < 5 minutes (Section 12.4)

#### 7c. Notify Team
- Alert: "Calculator X rolled back due to [reason]"
- Include error logs
- Include metrics that triggered rollback

#### 7d. Post-Rollback Analysis
- Review what went wrong
- Update golden scenarios if needed
- Fix issues before re-deploying

### Step 8: Documentation & Notification

#### 8a. Update Changelog
- Add entry to CHANGELOG.md:
  - Calculator name and version
  - Date deployed
  - What's new (features, fixes)
  - Breaking changes (if any)

#### 8b. Generate Deployment Report
```
Deployment Report
=================
Calculator: business-loan-dscr
Version: v2.1.0
Deployed: 2025-11-18 18:45:00 UTC
Environment: Production
Mode: Standard (100% traffic)

Pre-Deployment:
✓ Schema validation passed
✓ Golden scenarios: 3/3 passed
✓ Security scan: No issues
✓ Version check: New version

Build:
✓ UI generated successfully
✓ Exports generated
✓ TypeScript compiled
✓ Tests passed: 45/45

Deployment:
✓ Staging deployed and approved
✓ Production deployed
✓ Health check passed
✓ Smoke test passed
✓ WordPress shortcode: [smartprofit_calculator slug="business-loan-dscr"]

Post-Deployment (1 hour):
✓ Error rate: 0.3% (threshold: 1%)
✓ p95 latency: 92ms (target: <150ms)
✓ Export time: 2.1s (target: <3s)

Status: ✅ DEPLOYMENT SUCCESSFUL
```

#### 8c. Notify Stakeholders
- Email/Slack: "Calculator X v2.1.0 deployed to production"
- Include shortcode for WordPress embedding
- Include link to Calculator Management Dashboard
- Include deployment report

### Step 9: Enable Monitoring

#### 9a. Set Up Dashboard Alerts
- Configure alerts in Section 1.8 dashboards:
  - API Management Dashboard: Monitor health
  - Calculator Management Dashboard: Track usage
  - Business Intelligence Dashboard: Track conversions

#### 9b. Enable Analytics
- Ensure calculator_viewed events tracking
- Ensure calculator_calculated events tracking
- Ensure export/AI events tracking
- Verify events appearing in Section 7 dashboards

## Output

### Success:
```
🎉 Deployment Successful

Calculator: business-loan-dscr v2.1.0
Environment: Production
Deployed: 2025-11-18 18:45:00 UTC
Time: 8m 32s

WordPress Shortcode:
[smartprofit_calculator slug="business-loan-dscr"]

Links:
- Calculator: https://smartprofit.com/calculators/business-loan-dscr
- Dashboard: https://admin.smartprofit.com/calculators/business-loan-dscr
- Deployment Report: /deployments/business-loan-dscr-v2.1.0.md

Next Steps:
1. Embed calculator on WordPress using shortcode above
2. Monitor usage in Calculator Management Dashboard
3. Track conversion rates in Business Intelligence Dashboard
```

### Failure:
```
❌ Deployment Failed

Calculator: business-loan-dscr v2.1.0
Failed at: Post-Deployment Health Check
Reason: Health check endpoint returned 500 error

Error Details:
GET /api/calculators/business-loan-dscr/health
Response: 500 Internal Server Error
Body: {"error": "Formula library function not found: calculateXYZ"}

Action Taken: Automatic rollback initiated

Status: Rolled back to previous version (v2.0.5)

Next Steps:
1. Fix formula reference error in JSON config
2. Run Schema Validator Agent to catch error early
3. Re-deploy after fix
```

## Success Criteria
- All validation passes
- Build succeeds
- Staging deployment works
- Production deployment works
- Health checks pass
- Error rate < 1% after 1 hour
- Performance SLAs met
- WordPress shortcode generated

## Timeline
- **Standard deployment:** ~10 minutes (JSON to live)
- **Canary deployment:** ~6 hours (including monitoring phases)
- **Rollback:** < 5 minutes

## Usage Example
```bash
# Deploy new calculator to production
claude-code "Use deployment-pipeline-agent to deploy business-loan-dscr calculator to production"

# Deploy with canary mode
claude-code "Use deployment-pipeline-agent to deploy business-loan-dscr calculator to production with canary mode"

# Rollback to previous version
claude-code "Use deployment-pipeline-agent to rollback business-loan-dscr calculator"
```

## Dependencies
- Section 1.6: Performance SLAs
- Section 12.4: Deployment pipeline specification
- Schema Validator Agent
- Golden Scenario Tester Agent
- UI Generator Agent
- Export Builder Agent