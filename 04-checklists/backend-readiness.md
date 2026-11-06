# Backend Readiness Checklist

> [!IMPORTANT]
> **VERIFICATION MANDATE:** Cite route handlers, validation schemas, and middleware files for every assertion.
> Status tags: `[REQUIRED]`, `[RECOMMENDED]`, `[OPTIONAL]`, `[N/A]`, `[REQUIRES HUMAN VERIFICATION]`.

---

## 1. API Architecture & Request Ingress
- [ ] `[REQUIRED]` Resource-oriented RESTful endpoints using plural nouns and standard HTTP methods (GET, POST, PUT, PATCH, DELETE).
- [ ] `[REQUIRED]` Zero-trust ingress validation: All body, query, and path parameters parsed with schemas with unknown properties stripped.
- [ ] `[REQUIRED]` Correlation IDs attached to incoming requests and propagated across all logs and response headers.
- [ ] `[REQUIRED]` Security headers enforced (Helmet, CORS whitelist).

## 2. Authentication, Rate Limiting & Authorization
- [ ] `[REQUIRED]` Authentication middleware verifies tokens/sessions on all non-public endpoints.
- [ ] `[REQUIRED]` Authorization guards prevent horizontal privilege escalation (IDOR) on all `/api/resource/{id}` endpoints.
- [ ] `[REQUIRED]` Rate limiting enforced on authentication, password reset, and resource-heavy mutation routes.

## 3. Error Handling, Logging & Workers
- [ ] `[REQUIRED]` Standard RFC 7807 structured JSON error response envelope implemented.
- [ ] `[REQUIRED]` Zero stack traces or internal database query strings leaked in production responses.
- [ ] `[REQUIRED]` Structured single-line JSON logging with mandatory PII scrubbing (passwords, tokens, credit cards redacted).
- [ ] `[RECOMMENDED]` Long-running tasks offloaded to asynchronous queues with Dead-Letter Queue (DLQ) retry policies.

---

### Verification Sign-Off
- **Assessed By:**
- **Date:**
- **Evidence Summary:**

