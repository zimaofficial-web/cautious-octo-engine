# Knowledge Management Ingestion Procedure

> [!IMPORTANT]
> **GOVERNANCE PRINCIPLE:** New information, tutorials, or industry articles must **never** automatically override existing standards. Every external recommendation must be verified for technical accuracy, classified, checked for conflicts with established rules, and approved before updating documentation.

---

## The 10-Step Ingestion Pipeline

Whenever incorporating new information from external sources (articles, technical documentation, tutorials, research papers, videos, post-mortems, or personal lessons learned), follow this strict 10-step protocol:

```text
External Source Received
  │
  ├── 1. Extract Core Information (Summarize essential arguments and technical claims)
  ├── 2. Identify Actionable Recommendations (Isolate actionable techniques vs general prose)
  ├── 3. Verify Technical Accuracy (Cross-reference with official specs, benchmarks, RFCs)
  ├── 4. Separate Facts from Opinions (Distinguish empirical benchmarks from author preferences)
  ├── 5. Filter Outdated / Questionable Advice (Flag deprecated APIs or vendor bias)
  ├── 6. Classify Knowledge Domain (Engineering, Creative, Skill, Checklist, or Reference)
  ├── 7. Determine Destination Target (Rule in AGENTS.md, Skill, Checklist, or 01-engineering)
  ├── 8. Check for Conflicts with Existing Docs (Highlight contradictions; prompt user review)
  ├── 9. Consolidate & Deduplicate (Synthesize into existing files; prevent token bloat)
  └── 10. Record in Source Register & Changelog (Log citation, date, summary, and version bump)
```

---

## Detailed Step Guidance

### Step 1: Extract Core Information
- Document the title, author, publication date, URL, and the author's primary technical thesis.
- Extract concrete code snippets, architectural patterns, or operational workflows.

### Step 2: Identify Actionable Recommendations
- Strip away promotional narrative, sponsor mentions, and personal anecdotes.
- Formulate recommendations as clear directives: *"Use X when Y because Z."*

### Step 3: Verify Technical Accuracy
- Does this recommendation reflect current framework versions (e.g., React 19, Next.js 15, PostgreSQL 16+)?
- Does the advice scale under load, or is it a "toy example" suitable only for local demos?

### Step 4: Separate Facts from Opinions
- **Fact:** *"Creating an index on a foreign key in PostgreSQL eliminates sequential scans during joins."* (Measurable, verifiable).
- **Opinion:** *"Every backend should be written in Go instead of TypeScript."* (Context-dependent trade-off).

### Step 5: Identify Outdated or Questionable Advice
- Reject advice promoting deprecated practices (e.g., using `var`, writing React class components, storing JWTs in `localStorage`, or using `uuid_generate_v4()` when native `gen_random_uuid()` exists).

### Step 6 & 7: Classify Domain & Determine Target
Map the verified recommendation to its structural home:
- **Foundational Standard:** `01-engineering/[category]/standards.md`
- **Creative / Writing Style:** `02-creative-communication/[category]/guidelines.md`
- **Operational Workflow / Audit:** `03-skills/[skill-name]/SKILL.md`
- **Stage-Gate Verification Item:** `04-checklists/[checklist-name].md`
- **Core AI Constraint:** `AGENTS.md` (requires explicit user confirmation)

### Step 8: Conflict Detection & Resolution
- If the new source recommends a practice that directly contradicts an established standard (e.g., a tutorial advocating Prisma over raw SQL, while our standard mandates granular RLS optimization):
  - **Do NOT overwrite existing files.**
  - Present the contradiction to Zima:
    > *"Source [X] recommends [Pattern A], which conflicts with our existing standard in [File Y] which mandates [Pattern B]. Here are the trade-offs: [...]. Would you like to update the standard or retain the current rule?"*

### Step 9: Consolidate & Deduplicate
- Integrate the approved change directly into the relevant modular markdown file.
- Avoid creating standalone 3-line files that fragment context. Maintain progressive disclosure.

### Step 10: Record & Audit
- Append an entry to [`source-register.md`](./source-register.md).
- Record the modification in [`changelog.md`](./changelog.md).

