# Deployment & DevOps Standards

## 1. CI/CD Pipeline Architecture

Every code change must pass through an automated, immutable CI/CD pipeline prior to production release:
```text
Pull Request Created
  │
  ├── 1. Lint & Format Check (ESLint, Prettier, ShellCheck)
  ├── 2. Type Check (`tsc --noEmit`)
  ├── 3. Unit & Integration Test Suite
  ├── 4. Security Scan (Secret scanning, dependency vulnerability audit)
  ├── 5. Build Artifact Generation (Ensure clean, deterministic production build)
  │
Merged to Main
  │
  ├── 6. Deploy to Staging / Preview Environment
  ├── 7. Automated Smoke Tests & E2E Validation
  │
Approved for Production
  │
  ├── 8. Zero-Downtime Production Deployment
  └── 9. Post-Deployment Health Check & Automated Rollback Guard
```

---

## 2. Environment Configuration & Secret Hygiene

### 2.1 The 12-Factor Configuration Standard
- Store configuration in the environment, never in the code repository.
- Maintain separate environments: `development`, `preview / staging`, and `production`.

### 2.2 Strict Separation of Build-Time vs. Run-Time Variables
- **Public Build-Time Variables:** Variables baked into the client bundle at build time (e.g., `NEXT_PUBLIC_API_URL`, `VITE_APP_NAME`). Must never contain credentials.
- **Server Run-Time Secrets:** Injected into running containers or serverless runtimes at runtime (e.g., `DATABASE_URL`, `JWT_SECRET`, `STRIPE_SECRET_KEY`).
- **Schema Validation on Startup:** Validate all environment variables during application startup using Zod or Envalid. If a required secret is missing or malformed, crash immediately with an explicit error rather than failing mysteriously during a user request.

---

## 3. Production Hardening, Domains & SSL/TLS

- **Strict HTTPS Enforcement:** Redirect all HTTP traffic to HTTPS with an HTTP 301 redirect.
- **Security Headers:**
  - `Strict-Transport-Security (HSTS)`: `max-age=63072000; includeSubDomains; preload`
  - `X-Content-Type-Options`: `nosniff`
  - `X-Frame-Options`: `DENY`
  - `Referrer-Policy`: `strict-origin-when-cross-origin`
- **DNS Security:** Enable DNSSEC and configure valid SPF, DKIM, and DMARC records for any domain sending emails.

---

## 4. Zero-Downtime Deployments & Rollback Strategies

### 4.1 Deployment Strategies
- **Rolling Deployments:** Gradually replace instances of the previous version with the new version behind a load balancer.
- **Blue/Green Deployments:** Spin up the new version in an identical parallel environment (Green). Run smoke tests. Switch load balancer traffic instantly from Blue to Green.
- **Canary Deployments:** Route 5% of production traffic to the new release; monitor error rates and latency for 15 minutes before routing 100%.

### 4.2 Automated & Instant Rollback
- Maintain the previous build artifact/container image in ready state.
- If health check endpoints report failures or error rates spike by $\ge 2\%$ post-deployment, trigger an automated rollback within 60 seconds.
- Database changes must be backwards-compatible (expand/contract pattern) so that rolling back the application code never breaks the database.

---

## 5. Observability, Health Checks & Monitoring

### 5.1 Dual Health Check Endpoints
Every backend service must expose two distinct health probes:
1. **Liveness Probe (`/health/live`):** Returns `200 OK` if the process is alive and responsive. Used by orchestrators to know when to restart a hung container.
2. **Readiness Probe (`/health/ready`):** Returns `200 OK` only when all external dependencies (database connections, Redis cache, required queues) are reachable. If the database is unreachable, return `503 Service Unavailable` so the load balancer stops routing traffic to this instance.

### 5.2 Metrics & Alerts
- Monitor the **Four Golden Signals**:
  1. *Latency:* Track p50, p95, and p99 request duration.
  2. *Traffic:* Requests per second (RPS).
  3. *Errors:* Rate of 5xx HTTP responses.
  4. *Saturation:* CPU, memory, and database connection pool utilization.
- Set actionable alerts directed to communication channels (Slack, Discord, PagerDuty). Never create noisy alerts that lead to alert fatigue.

---

## 6. Post-Deployment Verification (Smoke Testing)

Immediately following any production release, execute automated or manual smoke tests:
- [ ] Confirm `/health/ready` returns `200 OK`.
- [ ] Verify homepage and key marketing landing pages render.
- [ ] Execute an automated test user login.
- [ ] Check Sentry/error tracking dashboard for new unhandled exceptions.
- [ ] Verify external webhook endpoints (e.g., Stripe, Resend) are responding.

