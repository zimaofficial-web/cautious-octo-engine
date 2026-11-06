---
name: architecture-review
description: >-
  Use this skill to evaluate the system design, directory layout, modularity, component coupling, and scalability of an existing codebase or proposed system architecture.
---

# Architecture Review Skill

## 1. Purpose
Conducts an in-depth architectural evaluation of a codebase or proposed design to identify structural weaknesses, tight coupling, circular dependencies, technical debt, and scalability bottlenecks.

## 2. When to Use
- Prior to major refactoring or scaling milestones.
- When onboarding an existing codebase or reviewing pull requests with significant structural changes.
- When diagnosing architectural complexity, build slowdowns, or cross-module coupling.

## 3. Required Inputs
- Repository directory structure and dependency files (`package.json`, `go.mod`, `Cargo.toml`, etc.).
- High-level system architecture diagrams or specifications.
- Key integration points with third-party APIs and databases.

## 4. Step-by-Step Inspection Procedure
1. **Examine Directory & Module Boundaries:** Verify if features are isolated into high-cohesion modules or tangled across monolithic folders.
2. **Trace Dependency Direction:** Check if business logic directly imports UI components or raw database drivers (inward dependency violation).
3. **Assess Third-Party Decoupling:** Confirm whether external SDKs (Stripe, Supabase, SendGrid) are encapsulated behind adapter interfaces.
4. **Inspect Concurrency & State Handling:** Review shared global mutable state, caching strategies, and race condition vulnerabilities.
5. **Evaluate Scalability & Single Points of Failure:** Identify bottlenecks (e.g., synchronous long-running operations in HTTP workers, unindexed queries).
6. **Quantify Technical Debt:** Catalog anti-patterns, god objects (>400 lines), and circular dependencies.

## 5. Common Oversights
- Coupling domain models directly to database ORM entities.
- Lack of timeouts and circuit breakers on external HTTP calls.
- Mixing business validation with presentation UI logic.
- Leaking infrastructure secrets or database logic into the frontend.

## 6. Output Report Format
```markdown
# Architectural Evaluation Report

## 1. Structural Health Assessment
- **Modularity Rating:** [High / Moderate / Critical Risk]
- **Coupling & Cohesion:** [Summary of findings]

## 2. Architectural Violations & Bottlenecks
- **[CRITICAL] Issue 1:** [Location, violation, risk]
- **[WARNING] Issue 2:** [Location, violation, risk]

## 3. Technical Debt Catalog
- `path/to/file.ts`: [God object / tightly coupled logic]

## 4. Actionable Refactoring Roadmap
1. Step 1: [Isolate module X behind interface]
2. Step 2: [Extract database logic to repository layer]
```

## 7. Related Documentation
- [Architecture & System Design Standards](../../01-engineering/01-architecture-system-design/standards.md)
- [Database Architecture Review Skill](../database-architecture-review/SKILL.md)
- [Architecture Readiness Checklist](../../04-checklists/architecture-readiness.md)

