# Software Architecture & System Design Standards

## 1. Architectural Philosophy & Principles

### 1.1 Separation of Concerns (SoC)
- Systems must be decomposed into distinct logical layers:
  1. **Presentation / Interface Layer:** Handles UI rendering, user events, and input capture. No direct business logic or raw database queries.
  2. **Application / Service Layer:** Coordinates business workflows, enforces domain rules, and orchestrates operations.
  3. **Domain / Business Layer:** Contains pure business logic, calculations, and domain entities independent of frameworks.
  4. **Data / Infrastructure Layer:** Handles persistence, database drivers, third-party API clients, and external messaging.
- Dependencies must strictly flow inward: Higher layers depend on lower layers or abstract interfaces; infrastructure must never dictate domain logic.

### 1.2 Modularity & High Cohesion / Low Coupling
- Modules should represent distinct functional domains (e.g., `auth/`, `billing/`, `notifications/`, `catalog/`), rather than grouping purely by technical artifact (avoid putting 100 unrelated controllers in one giant folder).
- Modules communicate across explicitly defined interfaces or contracts. Internal module implementations must remain private.

### 1.3 Folder & File Organization Standard
A predictable, feature-first structure is preferred:
```text
src/
├── app/                  # Application routing, top-level layout & bootstrapping
├── modules/ (or features/)
│   └── [feature-name]/
│       ├── components/   # Feature-specific UI components
│       ├── services/     # Feature-specific business logic & API calls
│       ├── hooks/        # Feature-specific state/hooks
│       ├── types/        # Feature data types and schemas
│       └── utils/        # Feature pure utility functions
├── shared/ (or common/)
│   ├── components/       # Design system primitives (Button, Modal, Input)
│   ├── hooks/            # Generic hooks (useDebounce, useMediaQuery)
│   ├── utils/            # Pure helpers (formatDate, currency)
│   └── types/            # Global application types
└── config/               # App-wide configuration, environment schema validation
```

---

## 2. Design Patterns & Best Practices

- **Repository Pattern:** Abstract database access behind repository interfaces to keep business services decoupled from specific ORMs or query builders.
- **Factory Pattern:** Use factories when object creation involves complex validation or runtime environment branching.
- **Adapter / Facade Pattern:** Wrap third-party SDKs and external APIs with internal adapters. If an external vendor changes their API or is replaced, only the adapter is updated.
- **Strategy Pattern:** Avoid sprawling `if/else` or `switch` statements for dynamic business behaviors (e.g., payment providers, notification channels). Use strategy objects implementing a unified interface.

---

## 3. Dependency Management & Technical Debt

### 3.1 Third-Party Dependency Rules
- **Minimal Footprint:** Evaluate whether a small custom utility can replace a heavy external package.
- **Maintenance & Health Checks:** Never adopt dependencies that:
  - Are unmaintained (no commits or releases in >12 months with unresolved security issues).
  - Have severe open CVEs.
  - Rely on deprecated runtime features.
- **Lockfile Integrity:** `package-lock.json`, `pnpm-lock.yaml`, or equivalent must always be committed and frozen in CI builds.

### 3.2 Technical Debt Identification & Management
- Every shortcut or temporary workaround must be tagged with a structured comment:
  `// TECHDEBT([TICKET_OR_DATE]): [Description of trade-off, consequence, and proper fix]`
- Architectural reviews must identify:
  - God classes or mega-files (>400 lines of mixed concerns).
  - Circular dependencies.
  - Leaky abstractions (e.g., database models passed directly into UI templates).

---

## 4. API Architecture & Third-Party Integrations

### 4.1 Integration Boundaries
- **Timeouts & Circuit Breakers:** Every external HTTP/gRPC request must specify an explicit timeout (default: 5–10s max). Never allow an external dependency to hang server workers indefinitely.
- **Retry with Exponential Backoff:** Implement jittered exponential backoff for idempotent operations (GET/PUT); never blindly retry non-idempotent operations (POST) without idempotency keys.
- **Payload Sanitization & Ingress Parsing:** Never trust third-party webhook payloads. Verify digital signatures (HMAC) and validate payloads with strict runtime schemas (e.g., Zod) before processing.

---

## 5. Architectural Review Checklist

When evaluating any system design, confirm:
- [ ] Is the proposed structure modular and feature-isolated?
- [ ] Are third-party dependencies isolated behind adapters?
- [ ] Can this component be tested in isolation without firing up a real database?
- [ ] Are boundaries between client, server, and data strictly enforced?
- [ ] Does this design allow for graceful scaling and easy refactoring?

