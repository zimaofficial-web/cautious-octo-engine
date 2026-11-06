# Antigravity Master Blueprint
## Personal AI Development Standards, Knowledge Base & Skills System

- **Owner:** Zima
- **Environment:** Google Antigravity
- **Version:** `1.0.0`
- **Default AI Behavior:** Reference-Only (Implementation authority requires explicit user approval)

---

## 1. System Overview

This repository is a permanent, modular, and maintainable AI knowledge base and operating system designed for **Google Antigravity**. It eliminates the need to repeatedly write lengthy prompts, re-explain development standards, or remind the AI of critical security, architectural, and design requirements.

Whenever you open a project or ask a question, the AI references these guidelines to:
1. Understand the underlying business and technical requirements.
2. Uncover unmentioned edge cases, security vectors, and architectural dependencies (**"Think Beyond the Prompt"**).
3. Provide rigorous, modular recommendations with trade-offs.
4. Wait for your explicit approval before modifying code or executing commands.

---

## 2. Directory Architecture & Master Index

```text
.
├── AGENTS.md                                 # Master AI operating charter & behavioral rules
├── README.md                                 # System overview, navigation map & usage guide
│
├── 01-engineering/                           # Software Engineering Knowledge Base
│   ├── 01-architecture-system-design/        # Patterns, modularity, technical debt, API design
│   ├── 02-frontend-engineering/              # TypeScript, state isolation, forms, Core Web Vitals
│   ├── 03-backend-engineering/               # REST standards, validation pipelines, JSON logging
│   ├── 04-database-engineering/              # 3NF, composite indexing, zero-downtime migrations
│   ├── 05-supabase/                          # Mandatory RLS, service-role isolation, edge functions
│   ├── 06-application-security/              # STRIDE threat modeling, OWASP Top 10, sanitization
│   ├── 07-privacy-compliance/                # Privacy by design, cookie gating, GDPR Art. 15/17
│   ├── 08-ui-ux-design-quality/              # 8pt spatial grid, typography, anti-generic AI design
│   ├── 09-accessibility/                    # WCAG 2.1/2.2 AA standards, keyboard focus, ARIA
│   ├── 10-testing-qa/                        # Testing Trophy, MSW mocking, flakiness elimination
│   ├── 11-deployment-devops/                 # CI/CD, zero-downtime releases, health probes
│   └── 12-technical-documentation/           # 5-minute onboarding, ADR specifications, OpenAPI
│
├── 02-creative-communication/                # Non-Engineering Workspace (Strictly Isolated)
│   ├── 01-creative-visual-design/            # Branding, slide decks, posters, AI image prompts
│   ├── 02-writing-communication/             # Context-adaptive writing: WhatsApp to academic
│   ├── 03-research-information-processing/   # CRAAP source evaluation, fact-checking, synthesis
│   └── 04-personal-writing-preferences.md    # Editable voice profile, tone sliders, banned cliches
│
├── 03-skills/                                # Antigravity Native Specialized Skills
│   ├── project-intake-requirements/          # Scope definition & initial problem framing
│   ├── architecture-review/                  # Structural & system design assessment
│   ├── frontend-quality-audit/               # Component, rendering & client state audit
│   ├── backend-engineering-review/           # API, lifecycle, error & scalability audit
│   ├── database-architecture-review/         # Schema, constraints, relations & indexes audit
│   ├── supabase-security-review/             # RLS, edge security & permission policy check
│   ├── application-security-audit/           # Threat assessment & OWASP vulnerability scan
│   ├── privacy-compliance-review/            # Data retention, disclosures & privacy audit
│   ├── ui-ux-design-audit/                   # Interface aesthetics, visual hierarchy & usability
│   ├── accessibility-audit/                  # WCAG compliance & assistive-tech review
│   ├── code-quality-review/                  # Readability, maintainability, lint & anti-patterns
│   ├── testing-qa-review/                    # Coverage, edge cases, failure scenarios audit
│   ├── deployment-readiness-review/          # Release verification, env config & rollback audit
│   ├── documentation-review/                 # Completeness of docs, guides & API schemas
│   └── requirements-gap-analysis/            # Identifying missing/unspoken task requirements
│
├── 04-checklists/                            # Reusable Verification Checklists
│   ├── new-project-assessment.md             # Inception & viability verification
│   ├── architecture-readiness.md             # System design sign-off
│   ├── frontend-readiness.md                 # UI/UX & client layer sign-off
│   ├── backend-readiness.md                  # Server, API & business logic sign-off
│   ├── database-readiness.md                 # Schema & indexing sign-off
│   ├── supabase-security.md                  # RLS & policy verification
│   ├── application-security.md               # Security hardening verification
│   ├── privacy-compliance.md                 # Data handling & compliance verification
│   ├── accessibility-verification.md         # A11y manual & automated checklist
│   ├── testing-qa-signoff.md                 # Test suite & edge case verification
│   ├── production-deployment.md              # Pre-release checklist
│   └── final-project-review.md               # Comprehensive end-of-project sign-off
│
├── 05-templates/                             # Reusable Project Context & Specs
│   ├── project-context-template.md           # Master project brief (stack, constraints, goals)
│   ├── architecture-decision-record.md       # ADR format for recording critical technical decisions
│   └── feature-specification-template.md     # Feature definition template with edge cases
│
└── 06-knowledge-management/                  # Dynamic Learning & Ingestion System
    ├── intake-procedure.md                   # 10-step ingestion runbook for new sources
    ├── source-register.md                    # Registry of ingested books, articles, videos & docs
    └── changelog.md                          # Version history of blueprint modifications
```

---

## 3. Strict Context Isolation Firewall

This system enforces an absolute boundary between technical engineering and creative writing tasks:

| Domain | Active Standards | Boundary Rule |
| :--- | :--- | :--- |
| **Engineering** | `01-engineering/`<br>`03-skills/`<br>`04-checklists/` | Rigorous architectural and security standards apply. |
| **Creative & Communication** | `02-creative-communication/` | **NEVER** apply programming rules, code formatting, or developer constraints to writing, messaging, or design critique. |

---

## 4. How to Use This System in Antigravity

### Starting a New Project
1. Copy [`05-templates/project-context-template.md`](./05-templates/project-context-template.md) to your new project root as `PROJECT_CONTEXT.md` (or `.agents/context.md`).
2. Fill in the stack, features, and constraints.
3. The AI will automatically parse the context and consult the corresponding engineering chapters.

### Activating Skills
Antigravity automatically discovers skills in `03-skills/`. Each skill includes YAML frontmatter that activates progressively when you request tasks such as:
- *"Audit the database schema for missing indexes"* $\rightarrow$ triggers `database-architecture-review`
- *"Review our Supabase RLS policies"* $\rightarrow$ triggers `supabase-security-review`
- *"Perform an accessibility audit on this modal"* $\rightarrow$ triggers `accessibility-audit`
- *"Help me write a WhatsApp message to coordinate a meeting"* $\rightarrow$ triggers `02-writing-communication` and applies [`04-personal-writing-preferences.md`](./02-creative-communication/04-personal-writing-preferences.md).

### Continuous Ingestion
When you discover a great article, video, or engineering post-mortem, follow the 10-step ingestion runbook in [`06-knowledge-management/intake-procedure.md`](./06-knowledge-management/intake-procedure.md) to incorporate the learnings without overriding existing standards.

