---
name: deployment-readiness-review
description: >-
  Use this skill to audit deployment readiness before releasing to production, verify CI/CD pipelines, inspect environment variable hygiene, check health probes, and confirm rollback strategies.
---

# Deployment Readiness Review Skill

## 1. Purpose
Conducts a pre-release operational audit to ensure an application, infrastructure, and database migration can be deployed to production with zero downtime, verifiable health monitoring, and a tested rollback plan.

## 2. When to Use
- Preparing a major release or production deployment.
- Reviewing new infrastructure-as-code, Dockerfiles, or CI/CD workflow configurations.
- Conducting pre-flight release gate reviews.

## 3. Required Inputs
- CI/CD pipeline definitions (`.github/workflows/*.yml`, GitLab CI, etc.).
- Environment variable schemas and production `.env.example` templates.
- Infrastructure configuration (Dockerfiles, Kubernetes manifests, Vercel/Railway configs).
- Database migration scripts associated with the release.

## 4. Step-by-Step Inspection Procedure
1. **CI/CD Pipeline Integrity:**
   - Confirm pipeline enforces sequential stages: Lint $\rightarrow$ Typecheck $\rightarrow$ Unit/Integration Tests $\rightarrow$ Security Scan $\rightarrow$ Build $\rightarrow$ Deploy.
   - Verify pipeline halts immediately on any test or type failure.
2. **Environment Variable & Secret Hygiene:**
   - Verify startup schema validation: Does the server crash immediately on launch if a required secret is missing or invalid?
   - Confirm complete separation: No production database URLs or API keys baked into client-side build bundles (`NEXT_PUBLIC_`, `VITE_`).
3. **Health Probes & Traffic Routing:**
   - Verify distinct `/health/live` (process alive) and `/health/ready` (database and external dependencies reachable) probes exist.
   - Confirm orchestrator/load balancer is configured to route traffic only when `/health/ready` returns 200.
4. **Database Migration Safety & Backwards Compatibility:**
   - Verify schema migrations adhere to the **Expand/Contract** zero-downtime pattern.
   - Ensure new columns are nullable or have defaults to avoid breaking running application instances during deployment.
5. **Rollback Strategy & Artifact Preservation:**
   - Confirm previous build artifacts/container images are tagged and retained for instant traffic cutback.
   - Verify that rolling back application code will not corrupt the database state.
6. **Post-Deployment Smoke Verification Plan:**
   - Review automated smoke tests or manual verification checklist for immediate post-release execution.

## 5. Common Oversights
- Renaming or dropping a database column in the same release as application code, causing 500 errors during rolling updates.
- Missing startup environment validation, leading to runtime crashes when first hitting an unconfigured route.
- Deploying without verified DNS SPF/DKIM records, causing transactional emails to land in spam.
- Inadequate alerts, leaving the team unaware of a spike in 500 errors post-deployment.

## 6. Output Report Format
```markdown
# Deployment Readiness (Pre-Flight) Audit Report

## 1. Release Gate Decision
- **Status:** [GO FOR RELEASE / CONDITIONAL GO / NO-GO]
- **Risk Assessment:** [Low / Moderate / High Risk]

## 2. Critical Release Blockers (If Any)
- **[BLOCKER] Breaking DB Migration:** [Migration drops column X without expand/contract cycle]
- **[BLOCKER] Missing Secret Validation:** [Production secret Y not in startup schema]

## 3. Operational Infrastructure & Probes
- Health Endpoints: [`/health/live` and `/health/ready` verified]
- Rollback Mechanism: [Automated rollback configured / Manual rollback needed]

## 4. Post-Deployment Smoke Verification Steps
1. [ ] Verify `/health/ready` returns 200 OK
2. [ ] Execute live smoke test on login flow
3. [ ] Monitor Sentry error rate for 15 minutes
```

## 7. Related Documentation
- [Deployment & DevOps Standards](../../01-engineering/11-deployment-devops/standards.md)
- [Database Engineering Standards](../../01-engineering/04-database-engineering/standards.md)
- [Production Deployment Checklist](../../04-checklists/production-deployment.md)

