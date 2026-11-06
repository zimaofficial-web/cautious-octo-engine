---
name: privacy-compliance-review
description: >-
  Use this skill to evaluate privacy-by-design standards, data minimization practices, cookie consent management, GDPR/CCPA user rights (export and deletion), and open-source asset licensing.
---

# Privacy & Compliance Review Skill

> [!IMPORTANT]
> **DISCLAIMER:** This skill evaluates software engineering data architectures and compliance readiness best practices. It does **not** provide legal advice.

## 1. Purpose
Audits an application’s data collection practices, user consent mechanisms, data retention lifecycles, user rights endpoints (access and erasure), and open-source software licensing.

## 2. When to Use
- Preparing an application for international deployment (GDPR, CCPA, CPRA).
- Integrating marketing analytics, telemetry SDKs, or third-party tracking scripts.
- Reviewing account deletion, data export, or user privacy settings.

## 3. Required Inputs
- Data models showing collected personal identifiable information (PII).
- Cookie banner and consent management code.
- User data export and account deletion workflows.
- Dependency license manifest.

## 4. Step-by-Step Inspection Procedure
1. **Data Minimization Audit:** Identify all collected user fields. Flag any data collected without a direct functional purpose (e.g., phone numbers, birth dates, precise GPS).
2. **Consent & Cookie Banner Inspection:**
   - Confirm cookies are classified into 4 tiers (Strictly Necessary, Functional, Analytics, Marketing).
   - Verify non-essential cookies and analytics scripts are blocked **before** user consent is granted.
   - Verify no pre-ticked consent checkboxes exist.
3. **Third-Party Telemetry & PII Scrubbing:**
   - Inspect error tracking (Sentry) and analytics (PostHog/GA4) configurations to ensure passwords, tokens, and PII are redacted before leaving the app.
4. **User Rights Workflows (GDPR Art. 15 & 17):**
   - Confirm users can download their complete profile and activity records in machine-readable JSON/CSV.
   - Verify account deletion executes a true hard purge across all databases, search indexes, and caches, or securely anonymizes shared attribution.
5. **Marketing Communications & Unsubscribe:**
   - Confirm all non-transactional outbound emails include a working 1-click unsubscribe link and `List-Unsubscribe` headers.
6. **Open Source License Verification:**
   - Audit dependencies for viral copyleft licenses (GPL, AGPL) in proprietary commercial software.

## 5. Common Oversights
- Firing tracking pixels (Meta Pixel, Google Analytics) prior to receiving explicit cookie consent.
- Soft-deleting records (`deleted_at`) but never executing a permanent hard purge upon a GDPR erasure request.
- Bundling marketing newsletter sign-ups into required service terms during account creation.
- Failing to scrub user credentials or auth tokens from error telemetry.

## 6. Output Report Format
```markdown
# Privacy & Compliance Readiness Report

## 1. Data Minimization & Privacy Posture
- PII Collected: [Summary of user data captured]
- Minimization Rating: [Good / Excess Data Captured]

## 2. Consent & Cookie Banner Compliance
- **Script Gating:** [PASS / Scripts execute prior to consent]
- **Pre-ticked Checkboxes:** [NONE / Detected on form X]

## 3. User Rights Implementation (GDPR/CCPA)
- **Data Export (Art. 15):** [Automated JSON download / Missing]
- **Right to Erasure (Art. 17):** [Hard purge cascades verified / Incomplete]

## 4. Third-Party Telemetry & License Risks
- Telemetry Scrubbing: [Active / Leaking query parameters]
- License Conflicts: [All permissive (MIT/Apache) / Viral GPL detected]
```

## 7. Related Documentation
- [Privacy & Compliance Standards](../../01-engineering/07-privacy-compliance/standards.md)
- [Application Security Standards](../../01-engineering/06-application-security/standards.md)
- [Privacy & Compliance Checklist](../../04-checklists/privacy-compliance.md)

