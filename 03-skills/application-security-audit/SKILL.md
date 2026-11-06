---
name: application-security-audit
description: >-
  Use this skill to perform an application security audit, assess OWASP Top 10 vulnerabilities, evaluate authentication and authorization boundaries (IDOR), audit secrets hygiene, and review file upload security.
---

# Application Security Audit Skill

## 1. Purpose
Performs an end-to-end vulnerability and defense-in-depth security audit of an application to detect OWASP vulnerabilities, access control flaws, credential leaks, and injection vectors.

## 2. When to Use
- Before launching any web or mobile application to production.
- Reviewing sensitive authentication, payment, or data mutation workflows.
- Periodic security reviews or compliance readiness checks.

## 3. Required Inputs
- Full codebase access (controllers, models, middleware, templates).
- Environment variable schemas and configuration files.
- Authentication and session handling logic.

## 4. Step-by-Step Inspection Procedure
1. **Injection Vulnerability Scan:**
   - Verify all database queries use parameterized statements. Check for raw SQL string concatenation.
   - Audit system command execution (`exec`, `spawn`, `popen`) for unsanitized user arguments.
2. **Authentication & Session Analysis:**
   - Confirm password hashing uses Argon2id or bcrypt (cost $\ge 12$).
   - Verify session cookies set `HttpOnly`, `Secure`, and `SameSite=Lax/Strict`.
   - Ensure rate limiting is enforced on login, registration, and reset endpoints.
3. **Broken Access Control & IDOR Inspection:**
   - Audit all `/api/resource/{id}` endpoints to ensure user ownership / tenant isolation is validated against session context.
   - Verify role-based guards on administrative routes.
4. **Cross-Site Scripting (XSS) & Content Security:**
   - Check for unescaped HTML rendering or `dangerouslySetInnerHTML`.
   - Inspect Content Security Policy (CSP) headers.
5. **Secret Hygiene & Environment Security:**
   - Audit repo history and code for hardcoded API keys, private certificates, or database passwords.
   - Verify secrets are injected via secure runtime environments.
6. **File Upload Hardening:**
   - Verify uploads validate file magic bytes (not just extensions).
   - Ensure uploaded files are assigned randomized UUID filenames on isolated object storage.

## 5. Common Oversights
- Insecure Direct Object References (IDOR) where users can view another tenant's records by guessing the ID.
- Missing CSRF protection on cookie-authenticated mutation endpoints.
- Storing access tokens in `localStorage` where they are vulnerable to XSS theft.
- Permitting SVG uploads that contain embedded malicious `<script>` tags.

## 6. Output Report Format
```markdown
# Application Security Audit Report

## 1. Executive Security Posture & Risk Rating
- **Overall Threat Level:** [Low / Moderate / High / Critical]
- **Vulnerabilities Identified:** [X Critical, Y Warning, Z Informational]

## 2. Critical & High-Risk Vulnerabilities
- **[CRITICAL] Issue 1:** [Location, vulnerability type, proof of concept / exploit scenario, fix]
- **[HIGH] Issue 2:** [Location, vulnerability type, risk, fix]

## 3. OWASP Top 10 Coverage Matrix
- A01: Broken Access Control: [PASS / FAIL]
- A02: Cryptographic Failures: [PASS / FAIL]
- A03: Injection: [PASS / FAIL]
- A07: Identification and Authentication Failures: [PASS / FAIL]

## 4. Remediation Action Plan
1. Immediate Fix: [Patch SQL injection / IDOR endpoint]
2. Hardening: [Implement strict CSP and session flags]
```

## 7. Related Documentation
- [Application Security Standards](../../01-engineering/06-application-security/standards.md)
- [Supabase Security Standards](../../01-engineering/05-supabase/standards.md)
- [Application Security Checklist](../../04-checklists/application-security.md)

