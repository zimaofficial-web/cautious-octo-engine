---
name: documentation-review
description: >-
  Use this skill to audit project documentation, verify the 5-minute onboarding rule, check README completeness, review API contracts (OpenAPI), and audit Architecture Decision Records (ADRs).
---

# Documentation Review Skill

## 1. Purpose
Audits the technical documentation of a project to ensure developer onboarding takes under 5 minutes, API contracts are up to date, system architecture is transparently recorded via ADRs, and maintenance procedures are documented.

## 2. When to Use
- Preparing a repository for open source release, client handover, or team onboarding.
- After implementing major architectural changes or modifying environment configurations.
- Auditing documentation drift where code and docs have diverged.

## 3. Required Inputs
- Root `README.md`, `.env.example`, and setup guides.
- API specifications (`openapi.yaml`, Swagger, or generated types).
- Architecture Decision Records folder (`docs/adr/`).
- `CHANGELOG.md` and contribution guidelines.

## 4. Step-by-Step Inspection Procedure
1. **The 5-Minute Onboarding Verification:**
   - Review root `README.md`: Does it clearly state prerequisites, package manager versions, exact install commands, and local dev startup steps?
   - Test or trace setup instructions: Can a new developer clone and boot the app with zero tribal knowledge?
2. **Environment Variable Documentation:**
   - Cross-reference `.env.example` against actual application config schemas.
   - Verify every environment variable has an accompanying explanation, default value, and format guidance.
3. **API Contract & Schema Alignment:**
   - Verify OpenAPI specs or API schemas reflect current route signatures, parameter requirements, and response envelopes.
   - Check that error status codes (400, 401, 403, 404, 500) and sample payloads are documented.
4. **Architecture Decision Records (ADRs) Hygiene:**
   - Confirm major technical choices (ORM adoption, authentication method, state library) have corresponding ADRs explaining context, trade-offs, and consequences.
5. **Changelog & Semantic Versioning:**
   - Confirm `CHANGELOG.md` follows Keep a Changelog standards (Added, Changed, Deprecated, Removed, Fixed, Security) and matches git release tags.

## 5. Common Oversights
- Outdated setup steps referring to deprecated tools or missing prerequisites.
- Undocumented environment variables causing local boot failure for new engineers.
- Architecture docs that describe an aspirational system rather than the actual running implementation.
- Merging PRs with breaking API changes without updating OpenAPI contracts.

## 6. Output Report Format
```markdown
# Technical Documentation Audit Report

## 1. Onboarding & Usability Score
- **Onboarding Velocity:** [Ready (<5 mins) / Broken / Missing Prerequisites]
- **Documentation Drift:** [Synchronized / Moderate Drift / Severely Outdated]

## 2. Documentation Deficiencies & Drift
- **[CRITICAL] Missing Env Doc:** `.env.example` missing newly added `API_SECRET_KEY`
- **[WARNING] Outdated Command:** `README.md` references `npm run start` instead of `pnpm dev`

## 3. API Contract & ADR Status
- OpenAPI Spec: [Current / 3 endpoints missing]
- ADRs: [Present / Missing ADR for Supabase migration]

## 4. Documentation Action Plan
- [ ] Update README with Docker setup instructions
- [ ] Add missing environment variable descriptions to `.env.example`
```

## 7. Related Documentation
- [Technical Documentation Standards](../../01-engineering/12-technical-documentation/standards.md)
- [Architecture Decision Record Template](../../05-templates/architecture-decision-record.md)
- [Project Intake & Requirements Skill](../project-intake-requirements/SKILL.md)

