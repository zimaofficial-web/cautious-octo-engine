# New Project Assessment Checklist

> [!IMPORTANT]
> **VERIFICATION MANDATE:** Never mark an item complete without citing concrete evidence (file paths, configurations, or verified requirements).
> Status tags: `[REQUIRED]`, `[RECOMMENDED]`, `[OPTIONAL]`, `[N/A]`, `[REQUIRES HUMAN VERIFICATION]`.

---

## 1. Problem Definition & Scope Boundaries
- [ ] `[REQUIRED]` Clear problem statement and target audience defined. *(Evidence: Project brief or context document)*
- [ ] `[REQUIRED]` Scope separation: Explicitly documented MVP feature set vs. Phase 2 deferred features.
- [ ] `[RECOMMENDED]` Primary user journey mapped from initial landing to core value delivery.
- [ ] `[REQUIRES HUMAN VERIFICATION]` Business viability, unit economics, or user acquisition model validated by project owner.

## 2. Technology Stack & Feasibility
- [ ] `[REQUIRED]` Primary language and framework selected with LTS/maintenance verification.
- [ ] `[REQUIRED]` Database and persistence layer chosen to match relational and scaling requirements.
- [ ] `[REQUIRED]` Third-party SaaS dependencies identified (Auth, Email, Payments, Hosting) with pricing tiers reviewed.
- [ ] `[RECOMMENDED]` Local development environment reproducible via container (Docker) or standardized runtime tool (`.nvmrc`, mise).

## 3. Regulatory, Privacy & Constraints
- [ ] `[REQUIRED]` Data residency and jurisdictional privacy constraints identified (GDPR, CCPA, HIPAA).
- [ ] `[REQUIRED]` Target deployment platforms confirmed (Web, iOS, Android, Desktop) with minimum browser/OS support targets.
- [ ] `[RECOMMENDED]` Technical constraints documented (e.g., maximum budget, offline-first requirement, latency limits).

---

### Verification Sign-Off
- **Assessed By:**
- **Date:**
- **Evidence Summary:**

