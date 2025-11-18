# SECTION 3: PLATFORM ARCHITECTURE & TECH STACK - BUILD INSTRUCTIONS

## Overview
You are building Section 3 of the CFO Business Intelligence Calculator Suite PDR. This section defines the technical architecture, technology stack, module boundaries, and performance/scalability approach.

## Context
- Reference Section 1 for: performance SLAs (1.6), constraints (1.7), internal dashboards (1.8)
- Reference Section 2 for: 8 MVP calculators, phased roadmap
- Must support: WordPress embedding, Free/Pro/AI tiers, exports, AI narratives
- Target: p95 calc < 150ms, exports < 3s, AI < 3s p95

## Your Task
Create 4 markdown files:
1. `3.1-architecture-layers.md`
2. `3.2-technology-choices.md`
3. `3.3-module-boundaries.md`
4. `3.4-performance-scalability.md`

---

## 3.1 ARCHITECTURE LAYERS

**File:** `3.1-architecture-layers.md`

**Content:**
Describe a 7-layer architecture. For each layer:
- Purpose and responsibilities
- What components live here
- Communication patterns (how it talks to other layers)
- Stateful vs stateless

**The 7 layers:**
1. **Client Layer** - WordPress embeds, standalone web app, future mobile
2. **API Gateway / Backend Services** - REST API, authentication, rate limiting
3. **Calculation Engine** - Core math, formula library, validation
4. **Data Storage** - Scenarios, users, configs, sessions
5. **Export/Reporting Services** - PDF, CSV, Excel generation
6. **AI Services** - Prompt management, LLM calls, response formatting
7. **Analytics/Telemetry** - Event tracking, logging, monitoring

**Include:**
- ASCII or text-based architecture diagram if helpful
- Data flow between layers (what requests/responses look like)

**Format:** Section per layer with subsections. 3-4 pages.

---

## 3.2 TECHNOLOGY CHOICES

**File:** `3.2-technology-choices.md`

**Content:**
For each tech choice, provide: the technology, why chosen, alternatives considered, tradeoffs.

**Tech decisions needed:**
- **Frontend:** React + TypeScript (why?), state management (Redux/Zustand/Context?), styling (Tailwind/MUI/custom?)
- **Backend:** Node.js + TypeScript (why?), framework (Express/Fastify/NestJS?), hosting (Vercel/Railway/AWS?)
- **Databases:** 
  - Scenarios storage (PostgreSQL/MongoDB?)
  - Sessions (Redis?)
  - Analytics (ClickHouse/PostgreSQL?)
- **Message Queue:** For async exports and AI (BullMQ/AWS SQS/RabbitMQ?)
- **File Storage:** For exports (S3/Cloudflare R2/local?)
- **CDN:** For static assets (Cloudflare/CloudFront?)

**Format:** Subsection per tech category with comparison tables where useful. 4-5 pages.

---

## 3.3 MODULE BOUNDARIES

**File:** `3.3-module-boundaries.md`

**Content:**
Define shared modules that all calculators use. For each:
- Purpose
- API/interface (inputs and outputs)
- How calculators consume it
- Versioning approach

**Shared modules:**
1. **Finance Math Library** - Formulas, validators, formatters
2. **UI Component Library** - Calculator shell, input fields, result cards, warnings
3. **Export Module** - PDF/CSV/Excel generation from calculation results
4. **Analytics Client** - Event tracking, error logging
5. **Tier Gate Module** - Check Free/Pro/AI access, show upgrade prompts
6. **Calculator Config Schema** - JSON structure for defining new calculators

**Format:** Section per module. 3-4 pages.

---

## 3.4 PERFORMANCE AND SCALABILITY

**File:** `3.4-performance-scalability.md`

**Content:**
How we hit the performance SLAs from Section 1.6.

**Cover:**
- **Calculation Engine:** Stateless design, horizontal scaling, caching strategy (what gets cached, TTL)
- **Export Service:** When to use sync vs async, queue design, worker pools
- **AI Service:** Rate limiting, batching requests, fallback when AI is down
- **Database:** Read/write patterns, indexing strategy, connection pooling
- **CDN:** What gets cached at edge, cache invalidation

**Include:**
- Specific caching examples (e.g., "Cache calculation results for 5 minutes keyed by inputs hash")
- Scaling triggers (e.g., "Add export workers when queue depth > 100")

**Format:** Section per component. 3-4 pages.

---

## Overall Guidelines

**Tone:** Technical but clear. Writing for a dev team.

**Formatting:**
- Markdown headers: ##, ###, ####
- Tables for comparisons
- Code blocks for examples (configs, APIs, not full implementations)
- Bullet lists for features

**Consistency:**
- Reference Section 1.6 for SLAs
- Reference Section 1.7 for constraints
- Use consistent terminology

**Length:** 12-15 pages total across 4 files

**Output Files:**
- 3.1-architecture-layers.md
- 3.2-technology-choices.md
- 3.3-module-boundaries.md
- 3.4-performance-scalability.md

Each file starts with # header matching title.