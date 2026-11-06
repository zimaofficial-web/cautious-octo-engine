# Application Security Standards

## 1. Threat Modeling & Core Security Tenets

### 1.1 The STRIDE Threat Model
Before writing code for sensitive subsystems (auth, payments, data export), evaluate against STRIDE:
- **Spoofing:** Can an attacker pretend to be another user or service?
- **Tampering:** Can an attacker modify data in transit or at rest without detection?
- **Repudiation:** Can an attacker perform an action and plausibly deny doing it?
- **Information Disclosure:** Can sensitive data be leaked via logs, errors, or unencrypted channels?
- **Denial of Service:** Can resource exhaustion crash the application or database?
- **Elevation of Privilege:** Can an unprivileged user execute admin commands?

### 1.2 Defense in Depth & Zero Trust
- Never rely on a single defensive layer (e.g., client-side validation is a convenience, never a security barrier).
- Every internal boundary (API gateways, microservices, database connections) must authenticate and verify authorization for every request.

---

## 2. OWASP Top 10 Defenses & Common Vulnerabilities

### 2.1 Injection Attacks (SQL, NoSQL, Command)
- **Parameterized Queries:** Never concatenate user input into SQL or query strings. Always use parameterized queries or trusted ORM/query builder abstractions.
- **System Command Execution:** Avoid invoking shell commands (`exec`, `system`, `popen`) with dynamic user arguments. If external CLI execution is unavoidable, pass arguments strictly as sanitized arrays, never shell strings.

### 2.2 Broken Authentication & Session Security
- **Password Storage:** Hash passwords using modern, memory-hard algorithms: Argon2id (preferred) or bcrypt with work factor $\ge 12$. Never use MD5, SHA-1, or plain SHA-256.
- **Session Tokens & Cookies:**
  - Mark session cookies: `HttpOnly` (blocks XSS token theft), `Secure` (HTTPS only), and `SameSite=Lax` or `Strict` (mitigates CSRF).
  - Short-lived access tokens (15–60 minutes) paired with strictly rotated refresh tokens stored in secure, revokable server-side storage.
- **Brute Force Protection:** Enforce progressive delays or IP/account rate limits on login, password reset, and 2FA endpoints.

### 2.3 Broken Access Control & Authorization Boundaries
- **Horizontal Privilege Escalation (IDOR):** An authenticated user accessing `/api/documents/456` must have their tenant/user ownership validated (`WHERE id = 456 AND organization_id = current_user.org_id`). Never assume having an ID grants access to it.
- **Vertical Privilege Escalation:** Verify user roles and permissions on every protected endpoint, not just in UI route guards.

### 2.4 Cross-Site Scripting (XSS)
- **Contextual Encoding & Escaping:** Automatically escape all user-rendered data in templates.
- **Sanitize HTML:** If rich text HTML must be accepted, sanitize it using a battle-tested library (DOMPurify) configured with a strict tag whitelist.
- **Content Security Policy (CSP):** Serve strict HTTP headers:
  ```http
  Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none'; frame-ancestors 'none';
  ```

### 2.5 Cross-Site Request Forgery (CSRF)
- Use `SameSite=Lax` cookies for standard web navigation.
- For state-changing mutations (`POST`, `PUT`, `DELETE`), require custom headers (e.g., `X-Requested-With` or `Authorization: Bearer`) or validate anti-CSRF tokens for cookie-authenticated web forms.

---

## 3. Secrets Management & Environment Configuration

- **Zero Hardcoded Secrets:** No API keys, JWT secrets, database connection strings, or certificates may ever be committed to Git.
- **Automated Secret Scanning:** Maintain pre-commit hooks (e.g., Gitleaks) and CI scanning to reject commits containing API tokens.
- **Environment Isolation:** Maintain strictly segregated credentials across environments:
  - Development, Staging, and Production must use completely distinct databases, third-party API accounts (e.g., Stripe test keys vs live keys), and encryption salts.

---

## 4. File Upload Security

When accepting user file uploads:
1. **Validate Magic Bytes:** Do not rely solely on the file extension or the client-supplied `Content-Type` header. Inspect the file's initial bytes (magic numbers) to verify its true MIME type.
2. **Restrict File Extensions:** Enforce a strict whitelist of allowed extensions (e.g., `.png`, `.jpg`, `.pdf`). Never allow executable extensions (`.exe`, `.sh`, `.php`, `.svg` with inline scripts).
3. **Randomize Filenames:** Discard the user's original filename on disk/cloud storage. Store files under cryptographically random UUIDs to prevent directory traversal (`../../etc/passwd`).
4. **Isolate Storage:** Store uploads on an isolated CDN or cloud bucket (S3, Supabase Storage) with public execution disabled.

---

## 5. Security Logging, Monitoring & Incident Preparedness

- **Security Events to Log:**
  - Failed authentication attempts (with source IP, user identifier, and timestamp).
  - Password change or reset requests.
  - Role or permission modifications.
  - Access control rejections (403 Forbidden).
  - Rate limit triggers.
- **Alerting Thresholds:** Set real-time alerts for anomaly spikes (e.g., 50+ failed logins in 5 minutes against a single IP or user account).
- **Incident Response Plan:** Maintain a documented runbook for rotating leaked secrets and invalidating active user sessions in an emergency.

