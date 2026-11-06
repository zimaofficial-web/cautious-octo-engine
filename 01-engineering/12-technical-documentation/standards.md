# Technical Documentation Standards

## 1. Documentation Philosophy & The "5-Minute Onboarding Rule"

- **Primary Goal:** Any engineer (or AI assistant) with repository access should be able to clone the project, install dependencies, configure environment variables, and run a working local development environment in under 5 minutes.
- **Living Documentation:** Treat documentation as code. When code, schemas, or environment variables change, updating documentation is a required acceptance criterion for merging the PR.

---

## 2. Standard Repository `README.md` Structure

Every codebase must feature a comprehensive root `README.md` containing the following sections:
```markdown
# [Project Name]

> Concise one-sentence summary of what the project does and who it serves.

## 1. Architecture & Tech Stack
- Frontend: [Framework, styling, state management]
- Backend: [Framework, runtime, API style]
- Database: [PostgreSQL / Supabase, ORM / query builder]
- Key Third-Party Services: [Stripe, Auth0, Resend]

## 2. Prerequisites
- Node.js version (specify via `.nvmrc`) or runtime environment
- Package manager (e.g., `pnpm >= 9.0.0`)
- Docker (if local services are containerized)

## 3. Quick Start / Local Setup
1. Clone repo: `git clone ...`
2. Install dependencies: `pnpm install`
3. Configure environment: `cp .env.example .env` (fill in required local values)
4. Initialize database: `pnpm db:migrate`
5. Start development server: `pnpm dev`

## 4. Environment Variables
Table of all required variables, default values, and description (see `.env.example`).

## 5. Available Scripts
- `pnpm dev`: Starts local dev server
- `pnpm build`: Generates production build
- `pnpm test`: Runs test suite
- `pnpm lint`: Runs linter and type-checker

## 6. Architecture & System Flow
Links to ADRs, database diagrams, or API specifications.
```

---

## 3. Architecture Decision Records (ADRs)

### 3.1 When to Write an ADR
An ADR is mandatory whenever a significant technical or architectural choice is made:
- Choosing or replacing a database, ORM, or primary framework.
- Adopting a new authentication or authorization architecture.
- Introducing a major third-party SaaS vendor (e.g., Stripe, Supabase, Algolia).
- Migrating between architectural patterns (e.g., Monolith to Microservices, REST to GraphQL/tRPC).

### 3.2 Standard ADR Format
Store ADRs in `docs/adr/` or `01-engineering/12-technical-documentation/adr/` using the format:
```markdown
# ADR-[NUMBER]: [Short Title of Decision]

- **Status:** Proposed | Accepted | Deprecated | Superseded by ADR-[X]
- **Date:** YYYY-MM-DD
- **Decision Makers:** [Names/Roles]

### 1. Context & Problem Statement
What technical or business problem are we solving? What constraints exist?

### 2. Considered Options
- Option 1: [Description, pros, cons]
- Option 2: [Description, pros, cons]

### 3. Decision Outcome & Rationale
Chosen option and why it best satisfies our requirements.

### 4. Positive & Negative Consequences
- Positive impacts: [Performance, developer velocity, cost]
- Negative impacts / Trade-offs: [Maintenance overhead, vendor lock-in]
```

---

## 4. API Documentation & Contracts

- **Machine-Readable Contracts:** Maintain an automated OpenAPI 3.1 specification (`openapi.yaml` or generated via tRPC/Zod/Fastify).
- **Endpoint Documentation Checklist:**
  - Clear summary and detailed description.
  - Authentication requirements (public, user, admin).
  - Request parameters, query filters, and JSON body schemas.
  - Realistic example request payloads.
  - All possible HTTP response codes with example payloads (200, 400, 401, 403, 404, 500).

---

## 5. Changelogs & Versioning Standards

- **Semantic Versioning (SemVer 2.0.0):**
  - `MAJOR` (v1.0.0 $\rightarrow$ v2.0.0): Breaking API or database changes.
  - `MINOR` (v1.1.0 $\rightarrow$ v1.2.0): Backwards-compatible new features.
  - `PATCH` (v1.1.1 $\rightarrow$ v1.1.2): Backwards-compatible bug fixes and security patches.
- **Keep a Changelog Convention:** Maintain `CHANGELOG.md` grouped under:
  - `Added` (new features)
  - `Changed` (modifications to existing functionality)
  - `Deprecated` (soon-to-be removed features)
  - `Removed` (features now removed)
  - `Fixed` (bug fixes)
  - `Security` (vulnerability patches)

