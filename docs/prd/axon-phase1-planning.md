# Axon - Phase 1: Planning Document

**Project:** Axon - End-to-End Engineering Workflow Platform  
**Date:** March 8, 2026  
**Status:** Phase 1 - In Progress

---

## 1. Product Definition

### 1.1 Name & Tagline
- **Name:** Axon
- **Tagline:** Engineering Workflow
- **Slogan:** End-to-End Engineering Workflow

### 1.2 Problem Statement
EPCM projects (€6M-€70M) are managed across fragmented, disconnected systems:

1. **Financial CAPEX approval processes** - separate from everything else
2. **Engineering processes** (often waterfall) - siloed
3. **Procurement processes** - disconnected from engineering and project management
4. **EPCM project management** - the final piece, often reinventing the wheel

**This creates:**
- Complete disconnect between phases
- Company standards not communicated to EPCM suppliers
- Poor visibility across the full lifecycle
- Manual coordination across teams
- Document chaos and version conflicts
- Reactive (not proactive) project control

**Axon Solves This By:**
- Unified end-to-end platform connecting CAPEX → Engineering → Procurement → EPCM
- Agentic-first workflow automation - AI agents that act autonomously
- Company standards integrated and communicated to all stakeholders
- Single source of truth across the entire project lifecycle

### 1.3 Target Users
- **Project Director / Program Manager** - Leads €10M-€50M CAPEX projects
- **Engineering Manager** - Design deliverables, technical reviews
- **Procurement Lead** - Vendor contracts, material delivery
- **Contractor / Field Manager** - Field execution, on-site coordination
- **Supplier / Vendor** - Equipment delivery, compliance documentation

### 1.4 Scope

**MVP (6-month prototype):**
- Unified project structure with WBS
- End-to-end workflow templates (CAPEX → Engineering → Procurement → EPCM)
- AI agentic workflow (foundation for everything)
- Methodology templates (Prince2, PMP, Kanban, Scrumban, Scrum)
- Basic Gantt scheduling
- Document management with version control
- Team collaboration (comments, mentions)
- Dashboard & reporting
- User roles & permissions
- Company standards library (accessible to suppliers)

**Post-MVP (12-month full product):**
- Marketplace for AI agent skills (standard libraries)
- Mobile app
- Resource management
- Cost tracking & budget control
- Procurement workflow automation
- Analytics
- CAD kernel (build our own)

### 1.5 Core Features

**Wave 1 (MVP):**
- End-to-end workflow templates
- Project structure & WBS
- AI agentic workflow (foundation)
- Methodology templates (Prince2, PMP, Kanban, Scrumban, Scrum)
- Dashboard & reporting
- Team collaboration
- User roles & permissions
- Document management
- Company standards library
- Gantt scheduling

**Wave 2:**
- Marketplace for AI skills
- Mobile app
- Resource management
- Cost tracking
- Procurement automation
- Analytics

### 1.6 Success Metrics

**Category A: Platform KPIs (For Axon Admin)**
- Project activation rate
- Methodology adoption rate
- AI agent utilization rate
- User registration / active users
- Day 7 / Day 30 retention
- NPS score
- Customer satisfaction rating

**Category B: Customer KPIs (For User Dashboard)**

*CAPEX Management:*
- CAPEX approval status
- CAPEX governance compliance
- CAPEX spending vs budget
- Budget variance

*Project Success:*
- Schedule Performance Index (SPI)
- Cost Performance Index (CPI)
- Schedule variance
- Budget variance
- Change order frequency
- Task completion rate
- Earned Value (EV)

---

## 2. Technical Architecture

### 2.1 Tech Stack
- **Framework:** Tamagui Takeout v2
- **Stack:** React + Tamagui v2 + One (Universal Router) + Zero + Bun
- **Platform:** Web + iOS + Android from single codebase

### 2.2 External Services

**Core Services:**
- Database: PostgreSQL via **Neon**
- Hosting: **Uncloud** (Hetzner)
- Auth: Better Auth (built into Takeout)
- File Storage: AWS S3 or Cloudflare R2
- Real-time Sync: Zero (built into Takeout)

**AI & Agents:**
- LLM Gateway (LiteLLM or custom)
- API Key Manager (BYOK support)
- Credit/Token System
- Model Registry
- Agent Runtime
- LLM Providers: OpenAI, Anthropic, Google, etc.

**Operational:**
- Analytics: **PostHog**
- Error Tracking: **Sentry**
- Email: **Resend**
- Payments: **Stripe**

### 2.3 System Architecture

**CLIENT LAYER:**
- Web App (One SSR)
- iOS App (Expo)
- Android App (Expo)

**SYNC LAYER:**
- Zero (CRDT real-time sync between client/server)

**SERVER LAYER:**
- API Routes (One)
- Mutators (Zero)
- Better Auth (Self-hosted authentication)

**DATA LAYER:**
- PostgreSQL (Neon)
- Zero Sync (CVR)
- S3/R2 (File Storage)

### 2.4 AI Agent Architecture (Deep Dive)

**Vision:** Cursor-like Model Marketplace - similar to how Cursor IDE allows users to bring their own API keys or subscribe to managed models.

**OPTION A: Bring Your Own API Key (BYOK)**
- Users connect their own API keys (OpenAI, Anthropic, Google, Azure, AWS Bedrock)
- Users pay their own LLM bills directly through their provider
- Axon provides the agent infrastructure and workflow automation

**OPTION B: Axon Model Subscription**
- Users subscribe to Axon's managed models
- Axon handles LLM costs and provides credits/tokens
- Similar to OpenRouter or LiteLLM

**AI AGENT LAYER COMPONENTS:**

1. **LLM Gateway/Proxy**
   - Abstraction layer that routes to any LLM provider
   - Open source options: LiteLLM, OpenRouter
   - Handles provider-specific API differences
   - Rate limiting and quota management

2. **API Key Manager**
   - Secure encrypted storage for user-provided keys
   - Routing logic to use user's key for their requests
   - Key validation and refresh handling

3. **Credit/Token System**
   - Track usage for Axon-managed subscriptions
   - Tier-based credit allocation
   - Usage analytics and billing

4. **Model Registry**
   - List of available models (by provider)
   - Model capabilities and pricing info
   - User preference storage

5. **Agent Runtime**
   - Executes AI agents using selected models
   - Workflow orchestration
   - Skill execution engine
   - Memory and context management

6. **Skills Registry**
   - Marketplace for AI agent skills
   - Standard libraries for EPCM industry
   - User-created custom skills

---

### 2.5 Security Requirements

**Essential Security:**
- HTTPS everywhere
- Password hashing (Argon2 or bcrypt)
- SQL injection protection (via ORM)
- XSS protection (input sanitization)
- CSRF tokens for state-changing operations
- Rate limiting on auth endpoints
- Security headers (CSP, HSTS, X-Frame-Options)

**AI Agent Security:**
- API keys encrypted at rest
- User data isolation
- Agent permission scopes
- Human-in-the-loop for critical actions
- Audit logging for AI actions

---

## 3. Pricing Model

### 3.1 Platform Subscription (Traditional)

| Tier | Price | Users | Projects | Features |
|------|-------|-------|----------|----------|
| Starter | €15/user/month | Up to 5 | 3 | Basic PM, Templates |
| Professional | €35/user/month | Up to 20 | Unlimited | Full PM, Workflows, AI Assist |
| Enterprise | Custom | Unlimited | Unlimited | Everything + CAD, Custom |

### 3.2 AI Credits Model (Cursor-style)

| Plan | Price | AI Credits | Use Case |
|------|-------|------------|----------|
| Free | €0 | 100/mo | Try AI features |
| Pro | €29/mo | 5,000/mo | Individual users |
| Team | €99/mo | 25,000/mo | Small teams |
| Enterprise | Custom | Unlimited | Full access |

**Credit Usage:**
- Simple query: 1 credit
- Document analysis: 5 credits
- Workflow automation: 20 credits
- Complex reasoning task: 50 credits

**BYOK Option:** Users can connect their own API keys and pay directly to LLM providers (no credits used)

### 3.3 LLM Cost Reference (per 1M tokens)

| Model | Input | Output |
|-------|-------|--------|
| GPT-5.2 Pro | $21 | $168 |
| Claude Opus 4.6 | $15 | $75 |
| Gemini 2.5 Pro | $1.25 | $5 |
| GPT-5 mini | $0.25 | $2 |
| Claude Haiku | $0.25 | $1.25 |
| Gemini Flash | $0.075 | $0.30 |
| Grok 4.1 | $0.20 | $0.50 |

---

## 4. Next Steps

- [ ] Complete Phase 1: Planning (in progress)
- [ ] Phase 2: Setup & Infrastructure
- [ ] Phase 3: Development (MVP)
- [ ] Phase 4: Maintenance

---

*Document generated for Axon Phase 1 Planning*
*Axon - End-to-End Engineering Workflow*
