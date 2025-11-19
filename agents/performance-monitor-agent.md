# Performance Monitor Agent

## Purpose
Monitor calculator performance metrics in real-time, detect SLA violations, identify optimization opportunities, and ensure system meets performance targets from Section 1.6.

## Input
- Performance metrics (latency, throughput, resource usage)
- SLA targets from Section 1.6
- Calculator slug (specific calculator or all)
- Time range for analysis

## Process

### Step 1: Define Performance SLAs (from Section 1.6)

#### 1a. Response Time SLAs

**Calculation Engine:**
```
Target: p95 latency < 150ms
Warning: p95 > 120ms (80% of target)
Critical: p95 > 150ms (SLA violation)
```

**Export Generation:**
```
Target: p95 latency < 3 seconds
Warning: p95 > 2.5s (83% of target)
Critical: p95 > 3s (SLA violation)
```

**AI Requests:**
```
Target: p95 latency < 8 seconds (or timeout gracefully)
Warning: p95 > 6s (75% of target)
Critical: p95 > 8s or >10% timeout rate
```

**Page Load Time:**
```
Target: p95 < 2 seconds (full page load)
Warning: p95 > 1.6s (80% of target)
Critical: p95 > 2s (SLA violation)
```

#### 1b. Availability SLAs

**Uptime Target:**
```
Target: 99.0% monthly uptime
Warning: <99.5% (approaching target)
Critical: <99.0% (SLA violation)

Allowed downtime:
- Monthly: 7.2 hours (99.0%)
- Weekly: 1.68 hours
- Daily: 14.4 minutes
```

**Error Rate:**
```
Target: <1% error rate
Warning: >0.5% error rate
Critical: >1% error rate (SLA violation)
```

### Step 2: Collect Performance Metrics

#### 2a. Real-Time Metrics Collection

**Application Performance Monitoring (APM):**
Monitor with tools like New Relic, Datadog, or custom instrumentation:
```javascript
// Example metric capture
const startTime = performance.now();

// Execute calculation
const result = await calculateBusinessLoan(inputs);

const endTime = performance.now();
const duration = endTime - startTime;

// Log metric
trackMetric({
  metric_type: 'calculation_latency',
  calculator_slug: 'business-loan-dscr',
  duration_ms: duration,
  p50: calculatePercentile(durations, 50),
  p95: calculatePercentile(durations, 95),
  p99: calculatePercentile(durations, 99)
});
```

#### 2b. Metrics to Track

**Latency Metrics:**
- Calculation time (input → output)
- Export generation time
- AI request time
- Page load time (TTFB, FCP, LCP)
- API response time

**Throughput Metrics:**
- Requests per second (RPS)
- Calculations per minute
- Exports per hour
- Concurrent users

**Resource Metrics:**
- CPU utilization (%)
- Memory usage (MB)
- Database query time
- Cache hit rate (%)
- CDN cache hit rate (%)

**Web Vitals (Google Core Web Vitals):**
- Largest Contentful Paint (LCP): <2.5s
- First Input Delay (FID): <100ms
- Cumulative Layout Shift (CLS): <0.1

### Step 3: Monitor SLA Compliance

#### 3a. Continuous SLA Monitoring

**Every 5 Minutes:**
Check current p95 latency for:
- All calculators (aggregate)
- Each calculator individually
- Export service
- AI service

**Alert if:**
```
If p95 > Warning Threshold for 10 minutes → Warning alert
If p95 > Critical Threshold for 5 minutes → Critical alert
```

#### 3b. SLA Compliance Dashboard
```
SLA Compliance Report - Nov 18, 2025 18:45 UTC
=====================================================

Calculation Engine:
✅ p95 latency: 92ms (target <150ms)
✅ p99 latency: 128ms
✅ Error rate: 0.3% (target <1%)
Status: HEALTHY

Export Service:
⚠️ p95 latency: 2.8s (target <3s, warning at >2.5s)
✅ p99 latency: 3.4s
✅ Success rate: 96.2% (target >95%)
Status: WARNING - Approaching SLA limit

AI Service:
✅ p95 latency: 3.2s (target <8s)
⚠️ Timeout rate: 8% (target <10%)
✅ Success rate: 92%
Status: WARNING - Timeouts elevated

Page Load:
✅ p95 LCP: 1.6s (target <2.5s)
✅ p95 FID: 42ms (target <100ms)
✅ CLS: 0.05 (target <0.1)
Status: HEALTHY

Overall System Health: 🟡 FAIR
- 2 services in warning state
- 0 services in critical state
- No SLA violations detected
```

### Step 4: Identify Performance Bottlenecks

#### 4a. Latency Breakdown Analysis

**For slow calculations, analyze:**
```
Total Calculation Time: 287ms (exceeds 150ms target)

Breakdown:
1. Input validation: 8ms (3%)
2. Formula library calls:
   - calculateMonthlyPayment(): 45ms (16%)
   - calculateDSCR(): 182ms (63%) ← BOTTLENECK
   - Other formulas: 12ms (4%)
3. Output formatting: 18ms (6%)
4. Warning evaluation: 22ms (8%)

Root Cause: DSCR calculation inefficient
- Performing redundant calculations
- Not using cached values

Recommendation: Optimize DSCR formula
- Cache intermediate results
- Simplify calculation logic
- Expected improvement: 182ms → 60ms
```

#### 4b. Resource Utilization Analysis
```
Resource Utilization - Last Hour
================================

CPU Usage:
- Average: 42%
- Peak: 78% (during export generation spike)
- Headroom: 22% (healthy)

Memory Usage:
- Average: 1.2 GB / 4 GB (30%)
- Peak: 2.1 GB (52%)
- Memory leaks detected: None

Database:
- Average query time: 12ms
- Slow queries (>100ms): 3 detected
  - Scenario retrieval: 145ms ← INVESTIGATE
  - User lookup: 8ms (fast)
  - Export metadata: 122ms ← INVESTIGATE

Cache Performance:
- Hit rate: 87% (target >80%)
- Miss rate: 13%
- Eviction rate: 2%

Recommendations:
1. Add index to scenarios table (improve retrieval)
2. Increase cache TTL for export metadata
3. Monitor CPU during peak hours (may need scaling)
```

### Step 5: Track Performance by Calculator

#### 5a. Per-Calculator Performance
```
Calculator Performance Ranking (p95 latency)
============================================

Fast (< 100ms):
✅ Cash Runway Calculator: 68ms
✅ Breakeven Analysis: 74ms
✅ Simple Valuation: 82ms

Acceptable (100-150ms):
🟢 Business Loan + DSCR: 115ms
🟢 SBA 7(a) Analyzer: 128ms
🟢 Line of Credit: 142ms

Warning (>150ms):
⚠️ Equipment Lease vs Buy: 187ms
⚠️ Invoice Factoring: 203ms

Recommendations:
- Optimize Equipment Lease calculator (NPV calculations slow)
- Optimize Invoice Factoring (effective APR calculation inefficient)
```

#### 5b. Performance Trends Over Time
```
Business Loan + DSCR Calculator - 7-Day Trend
==============================================

Nov 12: p95 = 98ms ✅
Nov 13: p95 = 102ms ✅
Nov 14: p95 = 156ms ⚠️ (deployed v2.1.0 - regression?)
Nov 15: p95 = 148ms ⚠️
Nov 16: p95 = 122ms 🟢 (optimization deployed)
Nov 17: p95 = 115ms 🟢
Nov 18: p95 = 115ms 🟢

Insight: v2.1.0 deployment caused 50% latency increase
- Root cause identified: New covenant headroom metric added inefficient calculation
- Fix deployed Nov 16: Optimized calculation logic
- Performance now stable at acceptable level
```

### Step 6: Monitor External Dependencies

#### 6a. Third-Party Service Performance

**AI Provider (Anthropic API):**
```
Anthropic API Performance - Last 24 Hours
==========================================

Latency:
- p50: 1.8s
- p95: 4.2s (target <8s) ✅
- p99: 9.1s (occasional slow requests)

Success Rate: 94.2%
Timeout Rate: 5.8% (acceptable)

Status Codes:
- 200 OK: 94.2%
- 429 Rate Limit: 2.1% (expected during spikes)
- 500 Server Error: 1.8%
- 503 Service Unavailable: 1.1%
- Timeouts: 0.8%

Recommendation: Current performance acceptable
- Monitor 429 rate limits (consider request queuing)
- Implement retry logic for 5xx errors
```

**CDN Performance (CloudFlare):**
```
CDN Cache Performance
=====================

Hit Rate: 92% (excellent)
Miss Rate: 8%
Average Response Time: 45ms

Geographic Distribution:
- US-East: 38ms (best)
- US-West: 52ms
- Europe: 78ms
- Asia: 145ms (slowest, investigate)

Recommendation:
- Excellent overall performance
- Consider adding Asia CDN edge locations for better latency
```

### Step 7: Detect Performance Anomalies

#### 7a. Anomaly Detection

**Statistical Anomaly Detection:**
```
🚨 Performance Anomaly Detected

Calculator: business-loan-dscr
Metric: Calculation latency
Current p95: 324ms (vs normal 115ms)
Deviation: +181% (2.8 standard deviations)
Duration: 23 minutes
Impact: 847 slow calculations

Possible Causes:
1. Database slowdown? (check DB metrics)
2. Formula library issue? (check error logs)
3. Increased traffic? (check concurrent users)
4. Infrastructure issue? (check CPU/memory)

Action Taken:
- Alerted on-call engineer
- Monitoring closely
- Prepared rollback if needed

Status: INVESTIGATING
```

#### 7b. Pattern Recognition
```
📊 Performance Pattern Identified

Pattern: Daily latency spike at 9-10am EST
Observation: p95 latency increases 40% during this window
Affected: All calculators
Cause: Traffic spike (business hours starting on East Coast)

Current Handling: No issues, within SLA
Recommendation: Pre-emptive scaling
- Add auto-scaling trigger at 8:30am EST
- Scale up 2 additional instances
- Expected improvement: Reduce spike from 40% to 15%
```

### Step 8: Generate Performance Reports

#### 8a. Hourly Performance Summary (Auto-Generated)
```
⏱️ Hourly Performance Summary - 18:00-19:00 UTC
================================================

Overall: ✅ HEALTHY

Calculation Engine:
- p95 latency: 98ms (target <150ms) ✅
- Throughput: 1,247 calculations
- Error rate: 0.2%

Export Service:
- p95 latency: 2.1s (target <3s) ✅
- Exports generated: 312
- Success rate: 97.4%

AI Service:
- p95 latency: 3.8s (target <8s) ✅
- Requests: 89
- Success rate: 93.3%

Page Load:
- p95 LCP: 1.4s ✅
- p95 FID: 38ms ✅
- Traffic: 2,134 page views

Top Performers:
🏆 Cash Runway: 62ms (fastest)
🏆 Breakeven: 71ms

Needs Attention:
⚠️ Equipment Lease: 192ms (slow, investigate)

Next Report: 19:00-20:00 UTC
```

#### 8b. Daily Performance Report
```
📈 Daily Performance Report - Nov 18, 2025
===========================================

SLA Compliance: ✅ 100%
- All metrics within target
- Zero SLA violations
- System uptime: 100%

Key Metrics:
- Total calculations: 18,204
- Average p95 latency: 105ms (target <150ms)
- Average export time: 2.3s (target <3s)
- Error rate: 0.4% (target <1%)

Performance Highlights:
✨ Calculation latency improved 12% vs yesterday
✨ Export success rate 97.8% (up from 96.1%)
⚠️ AI timeout rate increased to 7.2% (monitor)

Slowest Hour: 9-10am EST (expected traffic spike)
Fastest Hour: 2-3am EST (low traffic)

Optimization Opportunities:
1. Equipment Lease calculator: 198ms avg (optimize NPV)
2. Invoice Factoring calculator: 215ms avg (optimize APR)
3. Database slow queries: 3 queries >100ms (add indexes)

Actions Taken Today:
✅ Deployed optimization to Business Loan calculator
✅ Increased cache TTL for formula library
✅ Added database index for scenario retrieval

Tomorrow's Goals:
- Maintain p95 < 150ms for all calculators
- Reduce AI timeout rate below 5%
- Deploy optimization for Equipment Lease calculator
```

#### 8c. Monthly Performance Review
```
📊 Monthly Performance Review - November 2025
==============================================

Executive Summary:
🎯 Overall SLA Compliance: 99.8% (target: 99.0%)
✅ All performance targets met or exceeded
🚀 Average latency improved 18% vs October

Detailed Metrics:

Availability:
- Uptime: 99.92% (target 99.0%) ✅
- Total downtime: 5.8 hours allowed vs 0.6 hours actual
- Incidents: 1 minor (resolved in 45 minutes)

Latency Performance:
- Calculation p95: 108ms average (target <150ms) ✅
- Export p95: 2.4s average (target <3s) ✅
- AI p95: 4.1s average (target <8s) ✅
- Page load p95: 1.6s average (target <2s) ✅

Throughput:
- Total calculations: 547,000 (up 22% vs Oct)
- Peak RPS: 47 (handled smoothly)
- Concurrent users peak: 312

Optimizations Deployed:
✅ Nov 4: Optimized formula library (15% latency reduction)
✅ Nov 12: Added database caching (improved export speed 20%)
✅ Nov 16: Fixed DSCR calculation regression
✅ Nov 23: Implemented CDN for static assets

Performance Improvements:
- Calculation latency: 128ms → 108ms (16% faster)
- Export generation: 2.9s → 2.4s (17% faster)
- Page load time: 1.9s → 1.6s (16% faster)

Known Issues:
⚠️ Equipment Lease calculator slow (avg 198ms, target <150ms)
- Root cause: NPV calculation inefficient
- Fix planned: December optimization sprint

🔮 AI timeout rate elevated (6.8% average)
- Root cause: External (Anthropic API intermittent latency)
- Mitigation: Implemented retry logic, monitoring

Trends:
📈 Traffic growing 8% month-over-month
📈 Mobile traffic increasing (now 28% of total)
📱 Mobile performance 12% slower than desktop (investigate)

December Goals:
1. Optimize Equipment Lease calculator (<150ms)
2. Improve mobile performance (reduce gap to 5%)
3. Reduce AI timeout rate below 5%
4. Maintain 99.0%+ uptime
5. Prepare for 2x traffic scaling (holiday season)
```

### Step 9: Auto-Optimize Performance

#### 9a. Automated Optimizations

**Cache Warming:**
```
Detected: Formula library cache cold after deployment
Action: Pre-warm cache with common calculations
Result: First-request latency reduced from 450ms to 120ms
```

**Auto-Scaling:**
```
Detected: CPU utilization >75% for 5 minutes
Action: Scale from 2 to 4 instances
Result: CPU utilization reduced to 42%
Wait for traffic to decrease, then scale down
```

**Query Optimization:**
```
Detected: Slow query (scenarios table, 145ms)
Action: Suggested adding index on user_id + calculator_slug
Result: Query time reduced to 8ms (94% improvement)
```

#### 9b. Proactive Recommendations
```
💡 Performance Optimization Recommendations

Based on analysis of last 30 days:

High Priority:
1. Add database index: scenarios(user_id, calculator_slug)
   - Impact: 94% faster scenario retrieval
   - Effort: 5 minutes
   - Risk: Low

2. Optimize Equipment Lease NPV calculation
   - Impact: 40% faster calculation (198ms → 120ms)
   - Effort: 4 hours development
   - Risk: Medium (formula change, needs thorough testing)

3. Implement result caching for repeated calculations
   - Impact: 80% faster for cached results
   - Effort: 8 hours development
   - Risk: Low

Medium Priority:
4. Upgrade to HTTP/2 for API endpoints
   - Impact: 15-20% faster page loads
   - Effort: 2 hours infrastructure
   - Risk: Low

5. Lazy-load AI explanation feature
   - Impact: 10% faster initial page load
   - Effort: 3 hours development
   - Risk: Low

Low Priority:
6. Compress JSON API responses
   - Impact: 5% faster API calls
   - Effort: 1 hour
   - Risk: Very low
```

### Step 10: Alert on Performance Issues

#### 10a. Alerting Rules

**Critical Alerts (Page Immediately):**
```
- p95 latency > 150ms for 5 consecutive minutes
- Error rate > 5% for any 5-minute window
- Uptime < 99.0% (SLA violation)
- All calculations failing (100% error rate)
```

**Warning Alerts (Slack Notification):**
```
- p95 latency > 120ms for 10 consecutive minutes
- Error rate > 0.5% for 15 minutes
- Export success rate < 95%
- AI timeout rate > 10%
```

**Info Alerts (Daily Digest):**
```
- Performance optimization opportunities identified
- Slow queries detected
- Cache hit rate below 80%
```

#### 10b. Alert Message Example
```
🚨 CRITICAL PERFORMANCE ALERT

Metric: Calculation Latency (p95)
Current: 187ms (target <150ms)
Duration: 8 minutes
Calculator: business-loan-dscr

Impact: SLA VIOLATION
Affected Users: ~200 (estimated)

Trend: ⬆️ Increasing (was 165ms 5 min ago)

Dashboard: https://dashboard.smartprofit.com/performance
Logs: https://logs.smartprofit.com/search?calculator=business-loan-dscr

Recommended Actions:
1. Check error logs for calculation failures
2. Verify database performance (slow queries?)
3. Check if recent deployment caused regression
4. Consider rollback if issue persists

Assigned: @oncall-engineer
Incident: INC-1249 created automatically
```

## Output

### Real-Time Performance Dashboard:
```json
{
  "timestamp": "2025-11-18T19:00:00Z",
  "sla_compliance": {
    "calculation_latency": {
      "p95_ms": 105,
      "target_ms": 150,
      "status": "healthy",
      "compliance_percent": 70
    },
    "export_latency": {
      "p95_ms": 2400,
      "target_ms": 3000,
      "status": "healthy",
      "compliance_percent": 80
    },
    "uptime": {
      "current_percent": 99.92,
      "target_percent": 99.0,
      "status": "healthy"
    }
  },
  "current_metrics": {
    "requests_per_second": 8.2,
    "concurrent_users": 47,
    "cpu_percent": 42,
    "memory_mb": 1247
  }
}
```

### Success Message:
```
✅ Performance Monitoring Report Generated

Time Period: Nov 18, 2025 (Daily)
SLA Compliance: 100% ✅

Performance Summary:
- Calculation p95: 105ms (target <150ms) ✅
- Export p95: 2.4s (target <3s) ✅
- AI p95: 4.1s (target <8s) ✅
- Error rate: 0.4% (target <1%) ✅
- Uptime: 100%

Improvements vs Yesterday:
- Calculation latency: 12% faster
- Export success rate: +1.7%
- Page load time: 8% faster

Issues Detected: 2
- Equipment Lease calculator slow (198ms, needs optimization)
- AI timeout rate elevated (7.2%, monitoring)

Optimizations Deployed: 1
- Business Loan calculator optimization (115ms → 92ms)

Next Actions:
- Optimize Equipment Lease NPV calculation
- Add database index for scenario retrieval
- Monitor AI service performance

Full dashboard: https://dashboard.smartprofit.com/performance
```

## Success Criteria
- All SLA metrics tracked in real-time
- Alerts fire when thresholds exceeded
- Performance trends identified
- Optimization recommendations actionable
- Reports generated automatically
- Zero false positives on critical alerts

## Quality Checks
- Metrics accurate (spot-check against logs)
- Alerts appropriate (not too noisy, not too quiet)
- Dashboards load quickly (<2s)
- Recommendations practical and prioritized
- Historical data retained (90 days minimum)

## Usage Example
```bash
# Generate performance report
claude-code "Use performance-monitor-agent to generate daily performance report for Nov 18 2025"

# Analyze specific calculator performance
claude-code "Use performance-monitor-agent to analyze business-loan-dscr calculator performance"

# Identify optimization opportunities
claude-code "Use performance-monitor-agent to identify performance optimization opportunities"

# Check SLA compliance
claude-code "Use performance-monitor-agent to check SLA compliance for last 7 days"
```

## Dependencies
- Section 1.6: SLA targets and quality goals
- Section 7.3: Performance monitoring implementation
- Section 1.8.6: Security & Compliance Dashboard (performance display)
- Error Monitor Agent: for correlating errors with performance
- APM tool (New Relic, Datadog, or similar)