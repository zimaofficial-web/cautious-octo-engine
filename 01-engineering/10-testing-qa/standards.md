# Testing & Quality Assurance Standards

## 1. The Testing Philosophy: The Testing Trophy

Rather than relying purely on brittle unit tests or slow end-to-end tests, structure your test suite according to the **Testing Trophy**:
```text
        /  E2E Tests  \          -> Critical user journeys only (5-10%)
       /===============\
      / Integration     \        -> Primary confidence layer (60-70%)
     /   Tests (APIs/UI) \
    /=====================\
   /      Unit Tests       \     -> Pure functions, calculations & utilities (20-30%)
  /=========================\
 /     Static Analysis       \   -> TypeScript strict mode, ESLint, Prettier (100%)
/-----------------------------\
```

---

## 2. Test Layer Specifications

### 2.1 Static Analysis (Continuous)
- Zero TypeScript errors (`tsc --noEmit`).
- Zero ESLint warnings or errors on commit.
- Strict code formatting enforced via automated git hooks (Husky + lint-staged).

### 2.2 Unit Testing Standards
- **Focus:** Pure logic, calculation engines, utility helpers, date formatters, and complex algorithmic functions.
- **Rule:** Fast, deterministic, in-memory execution. No network requests, real databases, or browser DOM rendering.
- **Naming Convention:** `[module].test.ts` or `[module].spec.ts` located adjacent to the source file.

### 2.3 Integration Testing Standards (The Workhorse)
- **Backend API Integration:**
  - Spin up real, ephemeral test databases (Docker container or test database branch).
  - Execute real HTTP requests through the route controllers and verify database state changes and HTTP response schemas.
  - Test happy paths AND edge cases (invalid tokens, duplicate emails, malformed payloads).
- **Frontend Component Integration:**
  - Use React Testing Library to test components as users interact with them.
  - Query elements by accessible role (`getByRole('button', { name: /submit/i })`), never by CSS class or test ID unless unavoidable.
  - Mock external network requests with MSW (Mock Service Worker) at the network layer rather than mocking component internal hooks.

### 2.4 End-to-End (E2E) Testing Standards
- **Tooling:** Playwright or Cypress.
- **Scope:** Reserve E2E tests for mission-critical business flows:
  1. User registration, email verification, and onboarding.
  2. Authentication and session persistence across page reloads.
  3. Checkout, payment processing, and receipt generation.
  4. Core product CRUD workflow (e.g., creating a project, editing, and publishing).
- **Flakiness Zero-Tolerance:** Never use arbitrary sleeps (`sleep(3000)`). Always wait for deterministic DOM assertions or network idle states (`waitForSelector`, `waitForResponse`).

---

## 3. Test Coverage & Edge Case Discipline

### 3.1 Meaningful Coverage vs. Vanity Metrics
- Aim for $\ge 80\%$ test coverage on core business logic, application services, and database repositories.
- Do **not** write tests solely to pump coverage percentages (avoid testing plain getters, boilerplate config files, or trivial UI pass-through components).

### 3.2 Mandatory Error & Edge Case Scenarios
Every critical workflow must have tests for:
- **Null / Undefined / Empty States:** What happens when an array is empty or an optional field is missing?
- **Boundary Values:** Zero values, negative numbers, maximum allowed string lengths, dates at month/year boundaries.
- **Network Failures:** How does the UI behave when an API returns 500, drops the connection, or times out?
- **Authorization Rejection:** Does an unauthenticated or unauthorized user receive an immediate 401/403?

---

## 4. Test Environments & Isolation

- **Test Data Factory Pattern:** Use factories to generate realistic, random test fixtures (e.g., Fishery, Faker). Never hardcode static IDs that collide when tests run in parallel.
- **Parallel Test Execution:** Every test file must be completely self-contained and run in isolation. A test must never depend on state left behind by a previous test.
- **Automated Teardown:** Reset database state (truncate tables or rollback transactions) between test runs.

---

## 5. Security & Performance Testing

- **Automated Vulnerability Audits:** Run `npm audit` / `pnpm audit` or Dependabot/Snyk in CI to block builds with known vulnerabilities.
- **Performance Budgets:** Automated lighthouse checks in CI for public pages (e.g., Lighthouse performance score $\ge 90$).

