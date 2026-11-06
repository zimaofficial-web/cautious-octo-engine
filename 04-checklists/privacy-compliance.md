# Privacy & Compliance Checklist

> [!IMPORTANT]
> **DISCLAIMER:** This checklist reflects technical software engineering best practices for data privacy. It does **not** constitute legal advice.
> Status tags: `[REQUIRED]`, `[RECOMMENDED]`, `[OPTIONAL]`, `[N/A]`, `[REQUIRES HUMAN VERIFICATION]`.

---

## 1. Data Minimization & Consent
- [ ] `[REQUIRED]` Personal data collection audited: Only data strictly necessary for core functionality is captured.
- [ ] `[REQUIRED]` Cookie banner gates all non-essential analytics and tracking scripts until explicit user consent is given.
- [ ] `[REQUIRED]` Zero pre-ticked consent checkboxes across all forms.
- [ ] `[REQUIRED]` User consent decisions logged with timestamps and policy version.

## 2. User Rights & Data Lifecycle (GDPR / CCPA)
- [ ] `[REQUIRED]` Automated or self-serve "Export My Data" functionality provides structured JSON/CSV records (GDPR Art. 15 & 20).
- [ ] `[REQUIRED]` "Delete Account" flow executes a true hard purge across databases, search indexes, and caches (GDPR Art. 17).
- [ ] `[REQUIRED]` Shared attribution handled cleanly upon deletion (anonymizing author attribution while preserving organization integrity).
- [ ] `[REQUIRED]` Automated data retention schedule configured (purging access logs older than 90 days).

## 3. Telemetry, Communications & Licensing
- [ ] `[REQUIRED]` Error monitoring (Sentry) and analytics scrub PII, passwords, and tokens before telemetry transmission.
- [ ] `[REQUIRED]` Marketing emails feature a 1-click unsubscribe link and `List-Unsubscribe` headers.
- [ ] `[REQUIRED]` Third-party dependency licenses audited (zero viral copyleft GPL/AGPL in proprietary software).
- [ ] `[REQUIRES HUMAN VERIFICATION]` Privacy policy, Terms of Service, and Subprocessor registers reviewed by legal counsel.

---

### Verification Sign-Off
- **Assessed By:**
- **Date:**
- **Evidence Summary:**

