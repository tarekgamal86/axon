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
- File Storage: **Cloudflare R2** (built into Takeout)
- Real-time Sync: Zero (built into Takeout)

**AI & Agents:**
- LLM Gateway: **LiteLLM**
- Agent Framework: **LangGraph**
- Agent Runtime: Custom based on LangGraph
- Skills Registry: Custom

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
- Zero (CRDT real-time sync - enables Figma/Miro-style collaboration)

**SERVER LAYER:**
- API Routes (One)
- Mutators (Zero)
- Better Auth (Self-hosted authentication)

**DATA LAYER:**
- PostgreSQL (Neon)
- Zero Sync (CVR)
- Cloudflare R2 (File Storage)

### 2.4 Security Requirements

**Authentication:**
- Better Auth (built into Takeout)
- OAuth, magic links, password authentication
- Multi-factor authentication (MFA)

**Authorization:**
- Role-Based Access Control (RBAC)
- Organization-level permissions
- Project-level access
- Row-level security in database (PostgreSQL RLS)

**Encryption:**
- TLS in transit (HTTPS everywhere)
- Encrypted at rest (database encryption)
- API keys encrypted with AES-256

**AI Agent Security:**
- API keys encrypted at rest
- User data isolation (multi-tenant)
- Agent permission scopes (what agents can/cannot do)
- Human-in-the-loop for critical actions
- Audit logging for AI actions
- Rate limits per user

**Compliance:**
- GDPR (EU data residency with Neon)
- SOC 2 (future goal)
- ISO 27001 (future goal)

### 2.5 Data Model (Comprehensive)

**LEVEL 1: Organization & Access**

*Organization*
- id, name, settings, created_at
- Financial Approval Workflows (CAPEX templates)
- Engineering Templates (company standards)

*User*
- id, email, name, organization_id
- Role within Organization: Admin, Member, Viewer
- Project Access: which projects user can see/work on

*Project Access (how external users get access)*
- Internal: Direct assignment by org admin
- External (EPCM/Supplier): Via procurement winning - auto-provisioned access

---

**LEVEL 2: Projects & Work**

*Project*
- id, name, organization_id, status, budget, start_date, end_date
- CAPEX Request linked
- Methodology (Prince2, PMP, Kanban, Scrumban, Scrum)

*Work Breakdown Structure (WBS)*
- id, project_id, parent_id, name, description
- Hierarchical structure

*Task*
- id, wbs_id, assignee_id, status, priority, due_date
- Board: Kanban / Scrum / Gantt views

*Milestone*
- id, project_id, name, target_date, status

*Board*
- id, project_id, type (Gantt, Kanban, Scrum)
- Columns, swimlanes configuration

---

**LEVEL 3: Documents & Collaboration**

*Document*
- id, project_id, name, type, current_version_id
- Collaborative editing (via Zero/CRDT)

*DocumentVersion*
- id, document_id, version_number, file_path, created_by, created_at

*Comment / Review*
- id, document_id, user_id, content, position
- Review status: Pending, Approved, Rejected

*Standards Library*
- id, organization_id, name, category, file_path
- Accessible to external suppliers

---

**LEVEL 4: CAPEX & Finance**

*CAPEX Request*
- id, organization_id, project_id, title, amount, status
- Approval Workflow: Draft → Submitted → Approved → Rejected

*Budget*
- id, project_id, total_amount, spent_amount

*Cost Tracking*
- id, project_id, category, amount, date, description

---

**LEVEL 5: AI Agents**

*Agent Definition*
- id, organization_id, name, description, skills

*Skill*
- id, name, description, action_schema
- Can be marketplace skills (external)

*Workflow Automation*
- id, project_id, trigger, agent_id, actions
- AI-driven workflows

*Credit / Token*
- id, organization_id, balance, subscription_tier

*API Key (BYOK)*
- id, organization_id, provider, encrypted_key
- User-provided keys for their own LLM billing

---

### 2.6 AI Agent Architecture (Agent-First Platform)

**Core Principle:** Axon IS an agent-first platform. AI agents are not a feature added on top - they ARE the platform. Every workflow, every action, every process is driven by agents. The UI is just the interface to interact with agents.

**Components:**

1. **LLM Gateway (LiteLLM)**
   - Unified API for multiple LLM providers
   - OpenAI, Anthropic, Google, Azure, AWS Bedrock
   - Rate limiting and quota management

2. **Agent Runtime (LangGraph)**
   - Stateful, multi-step agent workflows
   - Graph-based control flow
   - Loops, backtracking, complex logic

3. **Skills Registry**
   - Pre-built skills for EPCM industry
   - Custom skills per organization
   - Marketplace skills (Wave 2)

4. **BYOK Manager**
   - Secure encrypted storage for user-provided API keys
   - Routing to use user's key for their requests

5. **Credit System**
   - Track usage for Axon-managed subscriptions
   - Tier-based credit allocation

---

## 3. Design System & UX

### 3.1 Typography (Arabic + Latin)

**Selected Fonts:**

*Latin:* Inter
- Clean, modern, highly readable
- Used for: body text, UI labels, headings (Latin content)
- Source: Google Fonts (free, open source)

*Arabic:* IBM Plex Sans Arabic
- Designed specifically for bilingual Arabic-Latin text
- Professional, modern, excellent readability
- Used for: Arabic content, RTL UI
- Source: Google Fonts (free, open source)

**Font Strategy:**
- System fonts fallback: -apple-system, BlinkMacSystemFont for Latin
- Font loading: Google Fonts CDN
- RTL support: Full right-to-left layout for Arabic
- Tamagui integration via font-face tokens

### 3.2 Iconography

**Primary:** Phosphor Icons (built into Takeout)
- Weights: Thin, Light, Regular, Bold
- Categories: UI, Files, Arrows, Charts

**Extended:** 
- Phosphor Duotone for visual hierarchy
- Custom icons for EPCM-specific: Gantt, Kanban, engineering symbols

### 3.3 Key User Flows

**FLOW 1: Organization Setup**
1. User signs up (email or OAuth)
2. Create Organization (name, type)
3. Set up Financial CAPEX Workflow (define approval stages)
4. Upload Engineering Templates (company standards)
5. Invite Team Members (roles: Admin, Member, Viewer)
6. Organization ready

**FLOW 2: Multi-Organization Project**
1. Project Owner creates Project
2. Select Methodology (Prince2, PMP, Kanban, Scrumban, Scrum)
3. Define Project Details (budget, timeline, milestones)
4. Link CAPEX Request (if already approved)
5. Invite External Organizations (select, define access level)
6. External orgs accept and join
7. Project live with multi-org collaboration

**FLOW 3: Engineering Process**
1. Select Engineering Template
2. Define Engineering Phases (FEED, Basic Design, Detailed Design)
3. AI Agent suggests task breakdown
4. Assign tasks to internal + external teams
5. Document upload and versioning
6. Review and approval workflow
7. Progress tracking via Gantt

**FLOW 4: Project Management (Gantt/Kanban/Scrum)**
1. View Project in default board (configurable)
2. Switch between: Gantt, Kanban, Scrum views
3. Drag-drop task management
4. AI Assistant helps optimize schedule
5. Real-time collaboration (Figma-style)
6. Comments, reviews, approvals inline

**FLOW 5: AI Agent Interaction**
1. User opens AI Panel
2. Select Agent Type (Planner, Reviewer, Coordinator)
3. Give natural language task
4. Agent executes via LangGraph
5. Results displayed with confidence score
6. Human approval for critical actions

**FLOW 6: CAPEX Approval**
1. Create CAPEX Request
2. Fill details (amount, justification, timeline)
3. Submit to workflow
4. Approvers notified
5. Each approver reviews and approves/rejects
6. Status updates in real-time
7. If approved, funds released for project

### 3.4 Wireframes

1. Landing / Login - Value prop, sign up, login
2. Organization Setup Wizard - Multi-step org creation
3. Dashboard - Projects overview, KPIs, AI insights
4. Project List - All projects with filters
5. Project View (Tabbed) - Overview, Gantt, Kanban, Documents, Team
6. Gantt View - Timeline, dependencies, critical path
7. Kanban/Scrum Board - Task cards, swimlanes, drag-drop
8. Document Editor - Collaborative, version history, comments
9. AI Agent Panel - Chat interface, agent selection
10. CAPEX Workflow - Approval stages, status tracking
11. Settings - Organization, users, integrations
12. Admin Panel - Platform analytics, user management

---

## 4. Pricing Model

**Based on market research of Asana, Monday, ClickUp, Trello, and AI tools (Cursor, GitHub Copilot)**

### 4.1 Platform Subscription

Based on market standard: €10-25/user/month for professional features

| Tier | Price | Users | Projects | Features |
|------|-------|-------|----------|----------|
| Free | €0 | Up to 3 | 1 | Limited features |
| Starter | €10/user/month | Up to 5 | 3 | Basic PM features |
| Professional | €25/user/month | Up to 20 | Unlimited | Full PM + Workflows |
| Enterprise | Custom | Unlimited | Unlimited | Everything + CAD |

### 4.2 AI Credits Model (Cursor-style)

Based on AI coding tools: $10-40/user/month

| Plan | Price | Credits | Notes |
|------|-------|---------|-------|
| Free | €0 | 100/mo | Try AI |
| Pro | €20/mo | 5,000/mo | Individual |
| Team | €80/mo | 25,000/mo | Teams |
| Enterprise | Custom | Unlimited | Custom |

**Credit Usage:**
- Simple query: 1 credit
- Document analysis: 5 credits
- Workflow automation: 20 credits
- Complex reasoning task: 50 credits

**BYOK Option:** Users can connect their own API keys and pay directly to LLM providers (no credits used)

### 4.3 LLM Cost Reference (per 1M tokens)

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

## 5. Next Steps

- [x] Complete Phase 1: Planning (Section 1-3 done)
- [ ] Phase 2: Setup & Infrastructure
- [ ] Phase 3: Development (MVP)
- [ ] Phase 4: Maintenance

---

*Document generated for Axon Phase 1 Planning*
*Axon - End-to-End Engineering Workflow*
