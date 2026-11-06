---
name: project-intake-requirements
description: >-
  Use this skill when initiating a new project, starting a major feature, or when the user provides an initial concept or idea that needs structured requirements analysis, scope definition, and tech stack alignment.
---

# Project Intake & Requirements Analysis Skill

## 1. Purpose
Guides the structured analysis of a new project or major feature request to define scope, surface implicit requirements, identify constraints, and establish technical foundations before writing any code.

## 2. When to Use
- Starting a brand-new application or service from scratch.
- Evaluating a high-level feature pitch or user story.
- Transitioning from an informal conversation into a formal technical specification.

## 3. Required Inputs
- Initial project description, problem statement, or user prompt.
- Target audience / user personas.
- Intended deployment environment (web, mobile, desktop, CLI).
- Known business or regulatory constraints (e.g., budget, timeline, compliance).

## 4. Step-by-Step Inspection Procedure
1. **Analyze Business Objectives:** Extract the core problem being solved and the primary value proposition.
2. **Identify Primary User Journeys:** Outline the key paths (e.g., Guest $\rightarrow$ Sign Up $\rightarrow$ Core Action $\rightarrow$ Export).
3. **Map Non-Functional Requirements:**
   - Scale & Traffic: Expected concurrent users, throughput, storage growth.
   - Latency & Availability: SLA expectations, geographic distribution.
   - Compliance: GDPR, CCPA, HIPAA, financial data regulations.
4. **Evaluate Technology Stack Selection:** Review proposed languages, frameworks, and databases against team skills and maintenance overhead.
5. **Surface Hidden Dependencies:** Uncover required third-party services (auth providers, transactional emails, payment gateways, CDN).
6. **Define Scope Boundaries:** Explicitly document what is **in scope** for MVP and what is deferred to Phase 2.

## 5. Common Oversights
- Forgetting authentication edge cases (password resets, email verification, OAuth account linking).
- Overlooking administrative tools (user moderation, support inspection, audit logs).
- Assuming zero cost for third-party API tiers.
- Skipping data backup and rollback planning.

## 6. Output Report Format
Present findings in this structure:
```markdown
# Project Intake Analysis: [Project Name]

## 1. Executive Summary & Problem Framing
[Concise summary of project goal and target outcome]

## 2. Core Feature Scope (MVP vs. Phase 2)
- **In-Scope (MVP):**
  - Feature 1...
- **Deferred (Phase 2):**
  - Feature 2...

## 3. Recommended Technology Stack & Architecture
- Frontend: [...]
- Backend / API: [...]
- Database & Persistence: [...]
- External Services: [...]

## 4. Key Risks, Constraints & Hidden Dependencies
- Risk 1: [Mitigation]
- Dependency: [Vendor & potential cost]

## 5. Next Steps & Clarifying Questions
[3-5 focused questions for user approval]
```

## 7. Related Documentation
- [Architecture & System Design Standards](../../01-engineering/01-architecture-system-design/standards.md)
- [Requirements Gap Analysis Skill](../requirements-gap-analysis/SKILL.md)
- [Project Context Template](../../05-templates/project-context-template.md)

