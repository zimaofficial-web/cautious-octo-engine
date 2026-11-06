---
name: requirements-gap-analysis
description: >-
  Use this skill whenever analyzing a user request, feature prompt, or issue ticket to "think beyond the prompt", uncover unstated requirements, identify edge cases, and evaluate architectural, security, and data implications.
---

# Requirements Gap Analysis Skill ("Think Beyond the Prompt")

## 1. Purpose
Implements the core operating principle to **"Think Beyond the Prompt."** Analyzes concise, partial, or feature-focused user requests to uncover missing architectural dependencies, data integrity requirements, security vectors, accessibility standards, and failure edge cases.

## 2. When to Use
- **Mandatory on every substantive feature request** before jumping into code recommendations.
- When a prompt requests a complex subsystem in a few brief words (e.g., "add authentication", "build a payment form", "add file uploads").
- When converting vague product ideas into bulletproof technical specifications.

## 3. Required Inputs
- The incoming user request or prompt.
- Current repository context (existing tech stack, database schema, framework).
- Known project constraints.

## 4. Step-by-Step Inspection Procedure
Whenever a request is received, run it across the **10 Essential Dimensions of Completeness**:
1. **Architectural Fit & Decoupling:** How does this feature integrate into the current modular structure? Does it violate inward dependency rules?
2. **Data & Schema Integrity:** What new tables, columns, or relations are required? Are there foreign key constraints, unique indexes, or cascade rules?
3. **Authentication & Authorization Boundaries:** Who is allowed to access/mutate this? How does it prevent horizontal privilege escalation (IDOR)?
4. **Input Validation & Ingress:** What runtime validation schema is needed? What are boundary limits (min/max lengths, allowed characters)?
5. **State Management & Lifecycles:** Does this introduce client state, server state, or URL state? Are all 5 form lifecycle states covered?
6. **Error States & Resilience:** What happens if the network drops, third-party APIs fail, or the database throws a duplicate key error?
7. **Security & OWASP Vectors:** Is rate limiting needed? Are inputs sanitized? Are secrets quarantined on the server?
8. **Privacy & Compliance:** Is PII collected? Does this require user consent or impact data deletion/export workflows?
9. **Accessibility (WCAG AA):** How will keyboard users navigate this? Are focus rings, ARIA labels, and screen reader announcements defined?
10. **Testing & QA Verification:** What unit, integration, and failure scenario tests are required to certify this feature?

## 5. Common Oversights (The "Forgotten Requirements")
- **Authentication:** Forgetting account verification, password reset flows, session revocation on password change, and brute-force rate limiting.
- **File Uploads:** Overlooking file magic-byte validation, virus scanning, storage quotas, and private vs. public bucket isolation.
- **Payments:** Missing webhook signature verification, idempotency handling for duplicate webhook events, and dispute handling.
- **State Deletions:** Failing to handle foreign key dependencies (what happens to child records when a parent is deleted?).

## 6. Output Report Format
```markdown
# Requirements Gap Analysis: [Requested Feature]

## 1. Objective & Direct Interpretation
[Summary of the explicit user request]

## 2. Critical Unstated Requirements ("Thinking Beyond the Prompt")

| Dimension | Missing Requirement | Why It Matters | Recommended Handling |
| :--- | :--- | :--- | :--- |
| **Security & Auth** | Rate limiting on endpoint | Prevents brute-force denial of service | Apply IP token bucket (5 req/min) |
| **Data Integrity** | Foreign key cascade rule | Prevents orphaned records | Add `ON DELETE CASCADE` |
| **Error Handling** | External API timeout | Prevents server workers from hanging | Set 5s timeout + circuit breaker |
| **Accessibility** | Modal focus trap | Blocks keyboard users from escaping | Implement dialog focus trap |

## 3. Implementation Trade-Offs & Architectural Options
- **Option A (Simpler / Faster):** [Trade-off description]
- **Option B (Robust / Highly Scalable):** [Trade-off description]

## 4. Questions & Direction Needed Before Implementation
[1-3 precise questions to confirm user preference]
```

## 7. Related Documentation
- [Master Operating Charter (AGENTS.md)](../../AGENTS.md)
- [Project Intake & Requirements Skill](../project-intake-requirements/SKILL.md)
- [Architecture Review Skill](../architecture-review/SKILL.md)

