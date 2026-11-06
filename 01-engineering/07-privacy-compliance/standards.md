# Privacy & Compliance Standards

> [!IMPORTANT]
> **DISCLAIMER:** The technical standards, data architectures, and checklists in this document represent software engineering best practices for data protection and privacy engineering. They do **not** constitute formal legal advice. Always review jurisdictional compliance (GDPR, CCPA, HIPAA, etc.) with qualified legal counsel.

---

## 1. Privacy by Design & Data Minimization

### 1.1 The Principle of Data Minimization
- **Collect Only What is Essential:** Do not collect telephone numbers, dates of birth, precise geolocation, or hardware identifiers unless strictly necessary for delivering the core product feature.
- **Default to Private:** Systems must be configured with maximum privacy settings by default (opt-in for analytics, telemetry, and marketing; never opt-out).
- **Pseudonymization & Tokenization:** Separate identifiable personal data (names, emails) from behavioral data (event logs, transaction records) using surrogate keys.

### 1.2 Data Retention & Deletion Lifecycle
- **Retention Schedules:** Define explicit retention periods for all collected data categories:
  - Application access logs: Retain for 30–90 days, then automatically purge.
  - Inactive user accounts: Flag after 12–24 months of inactivity; notify user before permanent deletion.
- **Soft Deletes vs. Hard Purges:**
  - *Soft Delete (`deleted_at TIMESTAMP`):* Useful for immediate application rollback and audit windows.
  - *Hard Purge (GDPR Right to Erasure):* An erasure request must trigger an automated workflow that cascades through all active databases, caches, search indexes (Algolia/Elastic), backups (via retention expiry), and third-party SaaS vendors.

---

## 2. Consent Management & Cookie Tracking

- **Strict Separation of Cookie Categories:**
  1. *Strictly Necessary:* Essential for session, security, and load balancing (No prior consent required).
  2. *Functional / Preferences:* Language selection, UI theme (Consent requested).
  3. *Analytics & Performance:* Page view metrics, heatmaps (Explicit opt-in required in GDPR regions before scripts execute).
  4. *Marketing & Advertising:* Retargeting pixels, social trackers (Explicit opt-in required before script loading).
- **No Pre-Ticked Checkboxes:** Consent must be freely given, specific, informed, and unambiguous. Pre-checked consent toggles are strictly prohibited.
- **Auditable Consent Logs:** Store timestamps, privacy policy versions, and user consent choices in an auditable database table.

---

## 3. Third-Party SDK & SaaS Audits

- **Inventory All Vendor Data Sharing:** Maintain an explicit registry of every third-party service receiving user data (e.g., Google Analytics, PostHog, Stripe, Sentry, Resend).
- **Data Scrubbing at the Border:**
  - Configure error tracking (Sentry, Datadog) to scrub personal identifiable information (PII), credit card numbers, passwords, and IP addresses before telemetry leaves the client or server.
  - Strip query parameters containing sensitive tokens or emails from analytics page-view URLs.
- **Subprocessor Disclosures:** Ensure all third-party processors are listed in the public privacy documentation.

---

## 4. User Rights Architecture: Access, Portability & Deletion

- **Right of Access & Portability (GDPR Art. 15 & 20):**
  - Provide an automated or self-serve "Export My Data" feature.
  - Return user records in a structured, commonly used, machine-readable format (JSON or CSV), including profile data, activity logs, and uploaded content.
- **Right to Erasure / "Be Forgotten" (GDPR Art. 17):**
  - Implement a self-serve "Delete Account" flow with re-authentication (require password or 2FA confirmation).
  - Handle shared records gracefully (e.g., if a user leaves a shared organization, anonymize their author attribution: `user_id = NULL`, `author_name = "Deleted User"` while preserving organization records).

---

## 5. Communications, Email & Marketing Compliance

- **CAN-SPAM & GDPR Email Mandates:**
  - Every non-transactional email (newsletters, marketing updates, announcements) must include a visible, functional **One-Click Unsubscribe link**.
  - Include `List-Unsubscribe` and `List-Unsubscribe-Post` headers in outbound emails for native email client unsubscribe buttons.
  - Clearly display the sender’s valid physical business mailing address in the email footer.
- **Transactional vs. Marketing Separation:**
  - Never bundle marketing subscriptions into essential transactional flows (e.g., password reset, order confirmation receipts).

---

## 6. Asset Licensing & Intellectual Property

- **Open Source License Verification:**
  - Audit dependencies for viral copyleft licenses (GPL, AGPL) in proprietary commercial codebases.
  - Maintain attribution files for MIT, Apache 2.0, and BSD-licensed libraries.
- **Media & Fonts:**
  - Verify commercial web font licenses (desktop vs. web font domain allowances).
  - Use verified royalty-free or purchased stock photography; never scrape imagery from search engines.

