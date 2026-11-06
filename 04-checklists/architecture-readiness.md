# Architecture Readiness Checklist

> [!IMPORTANT]
> **VERIFICATION MANDATE:** Cite specific architectural boundaries, directory structures, and code interfaces for every verified item.
> Status tags: `[REQUIRED]`, `[RECOMMENDED]`, `[OPTIONAL]`, `[N/A]`, `[REQUIRES HUMAN VERIFICATION]`.

---

## 1. Modularity & Separation of Concerns
- [ ] `[REQUIRED]` Clear layer separation: Presentation, Application/Service, Domain, and Infrastructure/Data layers isolated.
- [ ] `[REQUIRED]` Inward dependency rule enforced: UI and database infrastructure depend on domain interfaces, never the inverse.
- [ ] `[REQUIRED]` Module boundaries defined: Code is organized into feature/domain modules rather than horizontal file-type dumps.
- [ ] `[RECOMMENDED]` Third-party SDKs (Stripe, Supabase, SendGrid) wrapped behind internal adapter/facade interfaces.

## 2. Scalability, Concurrency & State
- [ ] `[REQUIRED]` No shared mutable global state in server runtimes.
- [ ] `[REQUIRED]` External HTTP calls specify bounded timeouts ($\le 10$ seconds) and circuit-breaker handling.
- [ ] `[RECOMMENDED]` Heavy background workloads (PDF generation, bulk emails) decoupled to asynchronous job queues.
- [ ] `[RECOMMENDED]` Caching strategy documented (cache invalidation rules, TTL, and cache-stampede prevention).

## 3. Technical Debt & Documentation
- [ ] `[REQUIRED]` Significant technical choices documented via Architecture Decision Records (ADRs).
- [ ] `[RECOMMENDED]` Zero circular module dependencies detected in static analysis graph.
- [ ] `[RECOMMENDED]` File length ceiling: No single code file exceeds 400 lines without documented justification.

---

### Verification Sign-Off
- **Assessed By:**
- **Date:**
- **Evidence Summary:**

