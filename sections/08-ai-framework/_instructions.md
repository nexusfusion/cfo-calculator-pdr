# SECTION 8: AI USAGE & PROMPTING FRAMEWORK - BUILD INSTRUCTIONS

## Overview
You are building Section 8 of the CFO Business Intelligence Calculator Suite PDR. This section defines how AI is used across the suite, prompt design standards, redaction/logging rules, and cost management.

## Context
- Reference Section 1.4 for: AI-enhanced but not AI-dependent, AI guardrails
- Reference Section 1.5 for: AI add-on tier (50 requests/month cap)
- Reference Section 5.2 for: privacy (no free-text by default, pseudonymous logging)
- AI role: narratives and suggestions only, never modifies formulas

## Your Task
Create 4 markdown files:
1. `8.1-ai-roles.md`
2. `8.2-prompt-design.md`
3. `8.3-redaction-logging.md`
4. `8.4-cost-management.md`

---

## 8.1 AI ROLES ACROSS THE SUITE

**File:** `8.1-ai-roles.md`

**Content:**
Define what AI does and does NOT do in the suite.

**AI roles:**
1. **Explanations and Narratives**
   - Per-scenario explanations (e.g., "What does my DSCR of 1.42 mean?")
   - Metric interpretation (e.g., "Why is this warning showing?")
   - Risk commentary (e.g., "What are the risks of this loan structure?")
   - Plain-language summaries of results
   - Always includes "Advisory only, not credit advice" disclaimer

2. **Scenario Suggestions**
   - "What else should I test?" recommendations
   - Parameter optimization hints (e.g., "Try increasing revenue by 10% to see impact on DSCR")
   - Alternative scenario ideas (e.g., "Consider a longer term to reduce monthly payment")
   - Never automatically creates scenarios, only suggests

3. **Cross-Calculator Insights (Future Phase)**
   - Out of scope for v1
   - Phase 3+: "This loan affects your cash runway" type insights
   - Multi-calculator scenario sets analyzed together

**What AI does NOT do:**
- Never modifies formulas or calculation logic
- Never makes credit decisions or underwriting recommendations
- Never guarantees outcomes (always uses hedging language like "may", "could", "typically")
- Never accesses or stores user PII
- Never learns from user data (stateless, no fine-tuning on user scenarios)

**AI disclaimer text (required in all AI outputs):**
"This analysis is for informational purposes only and does not constitute financial, legal, or professional advice. Consult qualified professionals for decisions affecting your business."

**Format:** Section per role, bullet list for what AI does NOT do, disclaimer box. 3-4 pages.

---

## 8.2 PROMPT DESIGN GUIDELINES

**File:** `8.2-prompt-design.md`

**Content:**
Define how to construct prompts for AI requests.

**Prompt design rules:**
1. **System Prompt Template**
   - Role definition: "You are a CFO advisor helping business owners understand financial calculations."
   - Output constraints:
     - Length: 150-250 words for explanations, 50-100 words for suggestions
     - Tone: Professional but approachable, avoid jargon
     - Format: Plain paragraphs, use bullet lists only when listing alternatives
   - Disclaimers to include: "This is decision support, not advice. Consult professionals."
   - Prohibited behaviors:
     - Never modify user inputs or suggest changing formulas
     - Never guarantee specific outcomes
     - Never use absolute language (always, never, must)
     - Use hedging language (may, could, typically, generally)

2. **Input Data Structure for Prompts**
   - Use structured aggregates (e.g., "Loan amount: $250,000, Interest rate: 7.5%, DSCR: 1.42")
   - Use ranges instead of exact values where appropriate (e.g., "DSCR between 1.25-1.50" not "DSCR: 1.42")
   - Never include free-text user notes by default
   - Never include scenario names or labels by default
   - Include only: numeric inputs, calculated outputs, warning flags (yes/no)

3. **Redaction Rules (Default)**
   - By default, NO free-text fields sent to AI
   - By default, NO scenario names/labels sent to AI
   - By default, NO counterparty names sent to AI
   - Only aggregated numeric data and generic descriptors
   - Opt-in flag required to send any free-text (with user consent and separate PDR review)

4. **Prompt Examples Per Calculator Type**
   - **Lending calculators (DSCR explanation):**
     - System: "You are a CFO advisor explaining debt service coverage ratio."
     - User: "The business has annual revenue of $1,500,000, operating expenses of $1,200,000, and a loan with monthly payment of $2,958. The calculated DSCR is 1.42. Explain what this means and whether it's acceptable for lenders."
     - Expected response: "A DSCR of 1.42 means the business generates $1.42 in operating income for every $1 in debt payments. Most lenders require a minimum DSCR of 1.25, so this ratio is comfortably above that threshold..."

   - **Cash flow calculators (runway risk):**
     - System: "You are a CFO advisor analyzing cash runway and burn rate."
     - User: "The business has $150,000 in cash, monthly revenue of $25,000, and monthly expenses of $35,000. The calculated runway is 15 months. Explain the risk."
     - Expected response: "With a monthly burn rate of $10,000 (expenses exceeding revenue), the business has approximately 15 months of runway. This assumes no changes to revenue or expenses..."

   - **Profitability calculators (margin risk):**
     - System: "You are a CFO advisor analyzing margins and breakeven."
     - User: "The product has a selling price of $100, variable cost of $60, and fixed costs of $50,000 per month. The contribution margin is 40% and breakeven volume is 1,250 units. Explain the risk of discounting."
     - Expected response: "With a contribution margin of 40%, each 10% price discount reduces margin to 30%, requiring 67% more volume to maintain the same profit..."

**For each prompt type:**
- System prompt text
- User prompt structure
- Example expected response

**Format:** Section per prompt rule, examples in quote blocks. 4-5 pages.

---

## 8.3 REDACTION, LOGGING, AND OPT-OUT

**File:** `8.3-redaction-logging.md`

**Content:**
Define what gets logged, what's redacted, and tenant-level AI controls.

**Sections:**

1. **AI Request Logging**
   - What gets logged:
     - session_id or user_id (pseudonymous)
     - calculator_slug
     - ai_request_type (explain_result, suggest_scenario)
     - timestamp
     - response_time_ms
     - success (true/false)
     - Summary: "DSCR explanation requested" (not full prompt text)
   - What does NOT get logged:
     - Full prompt text
     - Full AI response text
     - Any user PII (names, emails)
     - Specific numeric inputs from scenarios
   - Log retention: 12 months

2. **AI Suggestion Logging**
   - When AI suggests input changes, log:
     - session_id or user_id
     - scenario_id
     - suggestion_type (e.g., "increase_revenue", "extend_term")
     - suggestion_summary: "Suggested increasing revenue by 10%"
     - timestamp
   - User must explicitly accept suggestions (AI never auto-applies changes)

3. **Tenant-Level AI Controls (B2B)**
   - Control options:
     - **AI disabled entirely:** No AI calls made, no data sent to AI providers
     - **Strict redaction mode:** Only high-level aggregates sent (ranges, not exact values)
     - **Standard mode (default):** Numeric inputs and outputs sent, no free-text
     - **Enhanced mode (opt-in only):** Allow free-text fields if explicitly configured
   - Configuration per tenant:
     - Stored in tenant_configs table
     - ai_enabled (true/false)
     - ai_redaction_level (disabled, strict, standard, enhanced)
   - If AI is disabled, "Explain this result" buttons are hidden in UI

4. **On-Premises AI (B2B Option)**
   - For customers who want AI but won't send data externally:
     - Customer hosts their own LLM (e.g., self-hosted Claude, Llama)
     - Our system calls their endpoint instead of Anthropic API
     - Requires separate implementation and support agreement
     - Out of scope for v1

**Format:** Section per logging/control area. 3-4 pages.

---

## 8.4 COST MANAGEMENT CONSIDERATIONS

**File:** `8.4-cost-management.md`

**Content:**
Define AI cost expectations and optimization strategies.

**Cost analysis:**

1. **Per-Calculator AI Budget**
   - Expected AI requests per session:
     - Lending calculators: 1-2 requests (explain DSCR, explain total cost)
     - Cash flow calculators: 1-2 requests (explain runway, suggest cost cuts)
     - Profitability calculators: 1-2 requests (explain margin, pricing suggestions)
   - Average tokens per request:
     - Prompt (system + user): 300-500 tokens
     - Completion (AI response): 200-300 tokens
     - Total: 500-800 tokens per request
   - Cost per request (Claude API pricing example):
     - Input: 500 tokens at $3 per million = $0.0015
     - Output: 300 tokens at $15 per million = $0.0045
     - Total: ~$0.006 per request

2. **Usage Caps and Throttling**
   - Free tier: 0 AI requests (feature blocked)
   - AI tier: 50 requests per month (hard cap)
   - Overage handling:
     - Show message: "You've used all 50 AI requests this month. Your limit resets on [date]. Upgrade for more or wait until next month."
     - No pay-per-use overage in v1
     - Consider higher-tier plans (100 requests for $35/month, 200 for $60/month)

3. **Cost Optimization Strategies**
   - **Prompt caching:**
     - Cache system prompts (same for all users, high reuse)
     - Anthropic API supports prompt caching, reduces costs by 90% for cached portions
   - **Response caching:**
     - Cache AI responses for identical inputs (same calculator, same values)
     - Cache key: hash of (calculator_slug, inputs, request_type)
     - Cache TTL: 1 hour (balance freshness vs cost)
     - Expected cache hit rate: 20-30% (many users test similar scenarios)
   - **Batching requests:**
     - Not applicable for real-time UI interactions
     - Could batch for async report generation (future feature)

4. **AI Provider Selection and Fallback**
   - Primary provider: Anthropic Claude (Sonnet 4)
   - Fallback provider (if primary is down):
     - Option 1: OpenAI GPT-4
     - Option 2: Show message "AI is temporarily unavailable" and disable AI features
   - Cost vs quality tradeoffs:
     - Claude Sonnet: Higher quality, higher cost (current choice)
     - Claude Haiku: Lower cost, acceptable quality (consider for simple explanations)
     - GPT-3.5: Much lower cost, lower quality (not recommended for CFO-grade)

**Monthly cost projections:**
- Assume 1,000 active AI tier users
- Average 30 requests per user per month (60% of cap)
- 30,000 AI requests per month
- Cost per request: $0.006
- Total monthly AI cost: $180
- Revenue from AI tier: 1,000 users x $20/month = $20,000
- AI cost as percentage of revenue: 0.9% (very healthy margin)

**Format:** Section per cost area, tables for projections. 3-4 pages.

---

## Overall Guidelines

**Tone:** Technical and cost-conscious. Writing for product and engineering teams.

**Formatting:**
- Markdown headers: ##, ###, ####
- Tables for cost projections
- Quote blocks for prompt examples
- Bullet lists for rules and requirements

**Consistency:**
- Reference Section 1.4 for AI guardrails
- Reference Section 1.5 for AI tier limits
- Use consistent terminology (AI tier, AI request, AI narrative)
- Always emphasize: AI is advisory only, not decisions

**Length:** 13-16 pages total across 4 files

**Output Files:**
- 8.1-ai-roles.md
- 8.2-prompt-design.md
- 8.3-redaction-logging.md
- 8.4-cost-management.md

Each file starts with # header matching title.