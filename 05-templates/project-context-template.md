# Project Context Specification: [Project Name]

> **Purpose:** Copy this template into a new project's root as `PROJECT_CONTEXT.md` (or `.agents/context.md`). It provides complete, self-contained architectural and business context to the AI, preventing cross-project confusion.

---

## 1. Executive Summary & Objectives
- **Project Name:** [e.g., Nexus Dashboard]
- **Owner / Lead:** Zima
- **Current Development Stage:** [Idea / Inception / MVP Build / Production Refactor / Maintenance]
- **Core Purpose:** [Concise statement of what the project does]
- **Problem Being Solved:** [The pain point or business inefficiency addressed]
- **Target Audience & Personas:** [Who uses this product and how]

---

## 2. Technology Stack & Infrastructure
- **Primary Platform:** [Web Application / Mobile App / REST API / CLI]
- **Frontend Framework:** [e.g., Next.js 15 (App Router), React 19, Tailwind CSS v4, TypeScript]
- **Backend Framework / Runtime:** [e.g., Node.js / Hono / Express / Supabase Edge Functions]
- **Database & Persistence:** [e.g., PostgreSQL (Supabase), Redis for caching]
- **Authentication Approach:** [e.g., Supabase Auth with JWT and RLS / Auth0 / NextAuth]
- **Hosting & Cloud Infrastructure:** [e.g., Vercel (Frontend), Supabase (Database/Auth), AWS S3 (Storage)]
- **External SaaS Integrations:**
  - Payments: [e.g., Stripe]
  - Transactional Emails: [e.g., Resend]
  - Error Tracking & Analytics: [e.g., Sentry, PostHog]

---

## 3. Core Features & Scope Boundaries
### 3.1 In-Scope (Current Phase / MVP)
- Feature 1: [Description and acceptance criteria]
- Feature 2: [Description and acceptance criteria]

### 3.2 Explicitly Out of Scope (Deferred to Future Phases)
- Deferred Feature 1: [Reason for deferral]
- Deferred Feature 2: [Reason for deferral]

---

## 4. Architectural Rules, Security & Design Preferences
- **Security Baseline:** [e.g., Strict RLS on all tables, no client-side service-role access, Argon2id hashing]
- **Privacy Requirements:** [e.g., GDPR compliant, explicit cookie gating, zero third-party tracking without opt-in]
- **Design System & A11y Standards:** [e.g., 8pt grid, WCAG 2.1 AA contrast, Inter font family, dark mode support]
- **Testing Requirements:** [e.g., $\ge 80\%$ coverage on business logic via Vitest, Playwright for checkout journey]

---

## 5. Constraints, Known Limitations & Outstanding Decisions
- **Hard Constraints:** [e.g., Hard deadline, serverless cold-start limitations, budget ceiling]
- **Known Technical Limitations:** [e.g., Max file upload 10MB, no real-time collaboration in MVP]
- **Outstanding Architectural Decisions:**
  - [ ] Decision 1: [e.g., Choosing between cursor vs offset pagination for activity feed]
  - [ ] Decision 2: [e.g., Image optimization on edge vs cloud bucket trigger]

