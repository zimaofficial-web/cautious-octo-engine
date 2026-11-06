# Knowledge Source Register

This register logs every external resource (documentation, book, paper, video, article, or release note) ingested into this knowledge base.

---

## Registered Sources

| ID | Date Ingested | Source Title & Author | Type | Target Documentation Updated | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `SRC-001` | 2026-11-06 | Google Antigravity Blueprint (Zima) | Master Charter | Entire system baseline (`v1.0.0`) | Approved |
| `SRC-002` | 2026-11-06 | OWASP Top 10 Web Application Security Risks | Standard | `01-engineering/06-application-security/` | Approved |
| `SRC-003` | 2026-11-06 | W3C Web Content Accessibility Guidelines (WCAG 2.1/2.2) | Standard | `01-engineering/09-accessibility/` | Approved |
| `SRC-004` | 2026-11-06 | Supabase Row Level Security Architecture Guide | Official Docs | `01-engineering/05-supabase/` | Approved |
| `SRC-005` | 2026-11-06 | PostgreSQL 16 Official Manual (Indexes & Concurrency) | Official Docs | `01-engineering/04-database-engineering/` | Approved |

---

## Log Template for New Ingestions

```markdown
### [SRC-XXX]: [Source Title]
- **Author / Publisher:** [Name]
- **Date Published / Ingested:** [YYYY-MM-DD]
- **Source URL / Medium:** [Link or Book Citation]
- **Domain:** Engineering | Creative | Skill | Checklist
- **Core Insights Extracted:**
  - Insight 1...
  - Insight 2...
- **Target Files Updated:** [`path/to/file.md`]
- **Conflict Assessment:** [None / Conflict resolved with user approval]
```

