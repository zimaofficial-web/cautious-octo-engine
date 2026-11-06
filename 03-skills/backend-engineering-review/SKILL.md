---
name: backend-engineering-review
description: >-
  Use this skill to review backend servers, API route handlers, middleware pipelines, error handling envelopes, structured logging, rate limiting, and asynchronous worker queues.
---

# Backend Engineering Review Skill

## 1. Purpose
Conducts an engineering audit of backend APIs and server runtimes to ensure robust request validation, reliable error handling, security boundaries, high throughput, and observability.

## 2. When to Use
- Reviewing new API routes, controllers, and backend services.
- Auditing microservices or serverless functions before production deployment.
- Investigating unhandled server errors, memory leaks, or slow endpoint response times.

## 3. Required Inputs
- Backend controllers, routes, middleware, and service layers.
- Validation schemas (Zod, Joi, class-validator) and API contracts.
- Logging and error handling middleware implementations.

## 4. Step-by-Step Inspection Procedure
1. **API URI & HTTP Method Compliance:** Verify resource-oriented endpoints (plural nouns, no verbs in paths, correct status codes: 200, 201, 204, 400, 401, 403, 404, 409, 429).
2. **Ingress Validation:** Ensure all incoming parameters (body, query, params, headers) pass through strict schema validation with unknown keys stripped.
3. **Middleware Pipeline Audit:** Verify orderly flow: Request ID $\rightarrow$ Security Headers $\rightarrow$ Rate Limiting $\rightarrow$ Authentication $\rightarrow$ Authorization Guard $\rightarrow$ Controller.
4. **Error Management & Information Leakage:**
   - Confirm errors follow an RFC 7807 standard JSON structure.
   - Verify that stack traces and database internal error strings are completely hidden in production responses.
5. **Logging & Observability Check:** Confirm logs emit machine-readable JSON with correlation IDs and strict PII redaction.
6. **Asynchronous Offloading:** Ensure heavy workloads (emails, media processing, PDF generation) are offloaded to background queues (BullMQ/Redis/SQS) with DLQ retry handling.

## 5. Common Oversights
- Returning 200 OK for business error payloads.
- Missing rate limits on public, computationally expensive, or brute-force targets.
- Unbounded SQL queries without cursor/offset pagination.
- Performing long synchronous HTTP calls to third-party APIs inside incoming request lifecycles.

## 6. Output Report Format
```markdown
# Backend Engineering Review Report

## 1. API Architecture & Pipeline Health
[Overall health assessment of backend controllers and middleware]

## 2. Ingress & Validation Vulnerabilities
- **[CRITICAL] Issue 1:** [Endpoint, missing schema validation, mass-assignment risk]
- **[WARNING] Issue 2:** [Endpoint, inconsistent HTTP status code]

## 3. Error Handling & Information Disclosure
- [Leakage of database internals / stack traces]
- [Missing correlation IDs in error logs]

## 4. Asynchronous & Performance Recommendations
- [Offloading synchronous task X to queue Y]
- [Enforcing cursor pagination on endpoint Z]
```

## 7. Related Documentation
- [Backend Engineering Standards](../../01-engineering/03-backend-engineering/standards.md)
- [Application Security Standards](../../01-engineering/06-application-security/standards.md)
- [Backend Readiness Checklist](../../04-checklists/backend-readiness.md)

