# ADR-[NUMBER]: [Short Descriptive Title of Decision]

- **Status:** [Proposed | Accepted | Deprecated | Superseded by ADR-XXX]
- **Date:** YYYY-MM-DD
- **Author / Decision Makers:** Zima
- **Technical Area:** [Architecture | Database | Security | Frontend | DevOps]

---

## 1. Context & Problem Statement
*What technical challenge, architectural bottleneck, or business requirement necessitates this decision? What constraints (performance, cost, team familiarity, timelines) exist?*

## 2. Considered Alternatives

### Option A: [Name of Alternative]
- **Description:** [Brief summary of the approach]
- **Pros:**
  - Pro 1...
  - Pro 2...
- **Cons:**
  - Con 1...
  - Con 2...

### Option B: [Name of Alternative]
- **Description:** [Brief summary of the approach]
- **Pros:**
  - Pro 1...
  - Pro 2...
- **Cons:**
  - Con 1...
  - Con 2...

---

## 3. Decision Outcome & Rationale
*Which option was chosen and why? How does it resolve the problem while respecting our constraints?*

**Chosen Option:** **[Option A / B]**

### Justification:
1. Reason 1...
2. Reason 2...

---

## 4. Consequences & Impact Analysis

### Positive Impacts:
- [e.g., Eliminates N+1 query bottlenecks; improves p95 response time by 40%]
- [e.g., Unifies type definitions between frontend and backend via shared schemas]

### Negative Impacts / Trade-Offs (Managed Risks):
- [e.g., Requires engineers to learn new query syntax]
- [e.g., Adds 150KB to the serverless bundle size]

---

## 5. Implementation & Migration Plan
- [ ] Step 1: Install and configure dependency X
- [ ] Step 2: Implement repository adapter layer
- [ ] Step 3: Run integration test suite
- [ ] Step 4: Deprecate legacy implementation

