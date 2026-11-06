---
name: testing-qa-review
description: >-
  Use this skill to audit automated test suites, evaluate unit/integration/E2E test distribution (Testing Trophy), verify edge-case coverage, eliminate test flakiness, and audit test isolation.
---

# Testing & QA Review Skill

## 1. Purpose
Evaluates automated test suites and QA procedures to ensure high-confidence testing coverage, robust failure and edge case testing, proper mock boundaries, and elimination of flaky tests.

## 2. When to Use
- Auditing test suites for new features or before production deployment.
- Investigating slow, nondeterministic, or intermittently failing CI builds.
- Establishing test strategies for greenfield or refactored projects.

## 3. Required Inputs
- Test files (`*.test.ts`, `*.spec.ts`, Playwright/Cypress suites).
- Test runner configuration (`vitest.config.ts`, `jest.config.js`, `playwright.config.ts`).
- Code coverage reports.

## 4. Step-by-Step Inspection Procedure
1. **Testing Trophy Distribution Check:**
   - Verify proper balance: Integration tests as the workhorse (60–70%), unit tests for pure logic (20–30%), and E2E tests for core happy paths (5–10%).
   - Flag inverted testing pyramids (e.g., hundreds of slow, brittle E2E tests with zero backend integration tests).
2. **Mocking Boundaries & Fidelity:**
   - Confirm external network requests are mocked at the network layer (MSW) rather than mocking internal application modules or hooks.
   - Verify database integration tests use real ephemeral test databases rather than in-memory mocks that mask SQL dialect differences.
3. **Mandatory Edge Case & Failure Scenario Coverage:**
   - Check that tests cover not only the "happy path" but also: empty lists, invalid inputs, duplicate records, expired auth tokens, network timeouts, and server 500 errors.
4. **Flakiness & Determinism Audit:**
   - Search for hardcoded delays (`sleep(2000)`, `setTimeout`). Replace with explicit event-driven waiters (`waitForResponse`, `findByRole`).
   - Verify parallel execution safety: ensure tests do not share mutable database state or hardcoded IDs that collide.
5. **Test Assertions & User-Centricity:**
   - Ensure frontend tests assert on visible user-facing behavior (`screen.getByRole`) rather than implementation details (component internal state or private class names).

## 5. Common Oversights
- Writing vanity tests that test simple framework getters to inflate coverage percentages.
- Relying on arbitrary timeouts in E2E tests, causing intermittent CI failures.
- Tests that depend on the execution order of previous tests.
- Testing mock configurations rather than actual business logic.

## 6. Output Report Format
```markdown
# Testing & QA Audit Report

## 1. Test Suite Distribution & Health
- **Testing Trophy Ratio:** [Healthy / Skewed toward brittle E2E / Lacking Integration]
- **Core Logic Coverage:** [$\ge 80\%$ / Deficient in modules X, Y]

## 2. Flakiness & Non-Deterministic Risks
- **[CRITICAL] Hardcoded Timeout:** `e2e/checkout.spec.ts:45` [Replace `sleep(3000)` with network waiter]
- **[WARNING] Shared State Collision:** `tests/user.test.ts` [Uses hardcoded UUID colliding in parallel runs]

## 3. Missing Critical Edge Case Tests
- `auth/login`: Missing test for account lockout after 5 failed attempts
- `billing/webhook`: Missing test for expired Stripe webhook signature

## 4. Test Strategy Recommendations
- [ ] Migrate component mocks in X to Mock Service Worker (MSW)
- [ ] Add integration test container for PostgreSQL triggers
```

## 7. Related Documentation
- [Testing & Quality Assurance Standards](../../01-engineering/10-testing-qa/standards.md)
- [Deployment & DevOps Standards](../../01-engineering/11-deployment-devops/standards.md)
- [Testing & QA Sign-off Checklist](../../04-checklists/testing-qa-signoff.md)

