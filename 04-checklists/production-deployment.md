# Production Deployment Checklist

> [!IMPORTANT]
> **VERIFICATION MANDATE:** Cite CI/CD run outputs, environment configurations, and health check endpoints.
> Status tags: `[REQUIRED]`, `[RECOMMENDED]`, `[OPTIONAL]`, `[N/A]`, `[REQUIRES HUMAN VERIFICATION]`.

---

## 1. Pre-Deployment Verification
- [ ] `[REQUIRED]` CI/CD pipeline green: Lint, typecheck, tests, and security scans passed.
- [ ] `[REQUIRED]` Production environment variables validated via startup schema (zero missing secrets).
- [ ] `[REQUIRED]` Zero secrets or database passwords exposed in client-side build bundles.
- [ ] `[REQUIRED]` Database migrations follow the zero-downtime expand/contract pattern (no breaking locks).
- [ ] `[REQUIRED]` Previous production build artifact tagged and retained for instant automated rollback.

## 2. Infrastructure, Security & Monitoring
- [ ] `[REQUIRED]` `/health/live` and `/health/ready` probes configured and returning 200 OK.
- [ ] `[REQUIRED]` SSL/TLS certificates active with HTTPS redirection and HSTS headers enforced.
- [ ] `[REQUIRED]` Error monitoring (Sentry/Datadog) configured and actively capturing exceptions.
- [ ] `[REQUIRED]` Production database backups verified with a recent successful restore test.

## 3. Post-Deployment Smoke Test
- [ ] `[REQUIRED]` Live smoke test executed: Homepage renders, login works, and core mutation journey succeeds.
- [ ] `[REQUIRED]` Error rate and latency monitored for 15 minutes post-release with zero anomaly spikes.
- [ ] `[REQUIRED]` Webhook endpoints (Stripe, Resend) responding successfully with 200 OK.

---

### Verification Sign-Off
- **Assessed By:**
- **Date:**
- **Evidence Summary:**

