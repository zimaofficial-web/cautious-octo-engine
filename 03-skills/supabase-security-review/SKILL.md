---
name: supabase-security-review
description: >-
  Use this skill to audit Supabase projects, verify Row Level Security (RLS) policies, inspect PostgreSQL functions and triggers, protect service-role keys, and audit storage bucket security.
---

# Supabase Security & Configuration Review Skill

## 1. Purpose
Performs an exhaustive security audit of a Supabase architecture to ensure every table and storage bucket is protected by verified Row Level Security (RLS) policies, database functions are hardened against privilege escalation, and service-role keys are securely quarantined.

## 2. When to Use
- Before launching any Supabase-backed project or exposing public Data APIs.
- After creating new tables, storage buckets, or database functions.
- Auditing client-side codebases for accidental secret exposure.

## 3. Required Inputs
- SQL migration scripts, schema definitions, and RLS policy statements.
- Database functions (PL/pgSQL) and trigger definitions.
- Client `.env` files and API initialization code.
- Storage bucket configurations.

## 4. Step-by-Step Inspection Procedure
1. **Mandatory RLS Verification:**
   - Run verification query: Confirm `rowsecurity = true` on ALL public tables.
   - Verify `FORCE ROW LEVEL SECURITY` is applied to prevent bypass.
2. **Granular CRUD Policy Audit:**
   - Confirm explicit, separate policies exist for `SELECT`, `INSERT`, `UPDATE`, and `DELETE`.
   - Ensure `INSERT` policies use `WITH CHECK (auth.uid() = user_id)`.
   - Ensure `UPDATE` policies include both `USING` and `WITH CHECK` clauses.
3. **RLS Performance Check:**
   - Ensure all columns referenced in RLS policies (e.g., `user_id`, `organization_id`) have explicit B-tree indexes.
   - Verify `(SELECT auth.uid())` wrapping to prevent per-row re-evaluation.
4. **Service-Role Key Quarantine:**
   - Search client bundles and `.env` files for `service_role` or secret keys.
   - Confirm `service_role` is strictly confined to secure server runtimes / Edge Functions.
5. **Database Functions Hardening (`SECURITY DEFINER`):**
   - Check all `SECURITY DEFINER` functions: Verify `SET search_path = public, auth;` is explicitly declared.
   - Verify `REVOKE EXECUTE ON FUNCTION ... FROM PUBLIC;`.
6. **Storage Bucket & File Security:**
   - Confirm sensitive user files live in **Private** buckets.
   - Verify RLS policies on `storage.objects` match user authentication.
   - Verify `allowed_mime_types` and `file_size_limit` are configured.

## 5. Common Oversights
- Assuming enabling Supabase Auth automatically secures database tables (tables without RLS are wide open via the Data API).
- Leaving `service_role` keys inside frontend Next.js/Vite environment variables.
- Omitting `search_path` on `SECURITY DEFINER` functions, enabling search-path hijacking attacks.
- Missing indexes on user foreign key columns evaluated in RLS policies.

## 6. Output Report Format
```markdown
# Supabase Security Audit Report

## 1. Row Level Security (RLS) Posture
- Total Tables Evaluated: [X]
- Tables with RLS Enabled: [X]
- **[CRITICAL] Tables with RLS DISABLED:** [List tables immediately accessible without policy]

## 2. Policy Vulnerabilities & Missing CRUD Rules
- `table_name`: [Missing DELETE policy / permissive FOR ALL policy]
- `table_name`: [Missing WITH CHECK on UPDATE]

## 3. Function & Trigger Security (`SECURITY DEFINER`)
- `function_name()`: [Missing search_path hardening / public execution risk]

## 4. Storage Bucket & Secret Quarantine
- Bucket `documents`: [Public status / missing RLS on storage.objects]
- Client Env Check: [Clean / Service-role key detected in client bundle]
```

## 7. Related Documentation
- [Supabase Standards](../../01-engineering/05-supabase/standards.md)
- [Application Security Standards](../../01-engineering/06-application-security/standards.md)
- [Supabase Security Checklist](../../04-checklists/supabase-security.md)

