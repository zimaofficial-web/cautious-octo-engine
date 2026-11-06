# Application Security Checklist

> [!IMPORTANT]
> **VERIFICATION MANDATE:** Cite code lines, header configurations, and dependency audit reports.
> Status tags: `[REQUIRED]`, `[RECOMMENDED]`, `[OPTIONAL]`, `[N/A]`, `[REQUIRES HUMAN VERIFICATION]`.

---

## 1. OWASP Defenses & Injection Prevention
- [ ] `[REQUIRED]` 100% of SQL queries use parameterized statements (zero raw SQL string concatenation).
- [ ] `[REQUIRED]` System command execution strictly avoids dynamic shell strings.
- [ ] `[REQUIRED]` HTML contextual escaping enforced on all user-supplied data in UI views.
- [ ] `[REQUIRED]` Strict Content Security Policy (CSP) headers configured.
- [ ] `[REQUIRED]` Anti-CSRF protections active on all cookie-authenticated mutation endpoints.

## 2. Authentication, Sessions & Secrets
- [ ] `[REQUIRED]` Password hashing uses Argon2id or bcrypt (work factor $\ge 12$).
- [ ] `[REQUIRED]` Session cookies set `HttpOnly`, `Secure`, and `SameSite=Lax/Strict`.
- [ ] `[REQUIRED]` Brute-force rate limiting active on login, registration, and reset routes.
- [ ] `[REQUIRED]` Zero hardcoded secrets, passwords, or private keys committed to Git repository history.
- [ ] `[REQUIRED]` Automated dependency vulnerability scanning (`npm audit` / Snyk) passes in CI.

## 3. File Uploads & Security Monitoring
- [ ] `[REQUIRED]` File uploads validate magic numbers / byte headers (not just extensions or Content-Type headers).
- [ ] `[REQUIRED]` Executable file extensions (`.exe`, `.sh`, `.php`, SVG with scripts) strictly rejected.
- [ ] `[REQUIRED]` Uploaded files renamed with random UUIDs and stored on an isolated object storage CDN.
- [ ] `[RECOMMENDED]` Security logging configured for failed logins, password changes, and 403 authorization rejections.

---

### Verification Sign-Off
- **Assessed By:**
- **Date:**
- **Evidence Summary:**

