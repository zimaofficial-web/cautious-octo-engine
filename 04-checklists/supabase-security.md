# Supabase Security Checklist

> [!CAUTION]
> **VERIFICATION MANDATE:** Enabling Supabase Auth does NOT protect your tables. Every table must be verified in SQL.
> Status tags: `[REQUIRED]`, `[RECOMMENDED]`, `[OPTIONAL]`, `[N/A]`, `[REQUIRES HUMAN VERIFICATION]`.

---

## 1. Row Level Security (RLS) Hardening
- [ ] `[REQUIRED]` RLS enabled on ALL public tables (`ALTER TABLE ... ENABLE ROW LEVEL SECURITY;`).
- [ ] `[REQUIRED]` RLS forced for table owners (`ALTER TABLE ... FORCE ROW LEVEL SECURITY;`).
- [ ] `[REQUIRED]` Granular CRUD policies: Separate explicit policies for `SELECT`, `INSERT`, `UPDATE`, and `DELETE` (no blanket `FOR ALL` shortcuts).
- [ ] `[REQUIRED]` `INSERT` policies enforce `WITH CHECK (auth.uid() = user_id)`.
- [ ] `[REQUIRED]` `UPDATE` policies include both `USING` and `WITH CHECK` clauses.
- [ ] `[REQUIRED]` Foreign key columns evaluated in RLS policies have B-tree indexes.
- [ ] `[RECOMMENDED]` `(SELECT auth.uid())` subquery wrapping used to prevent per-row function re-evaluation.

## 2. API Keys, Functions & Storage
- [ ] `[REQUIRED]` `service_role` key is ZERO-exposed to client code, Git, or frontend environment variables.
- [ ] `[REQUIRED]` All `SECURITY DEFINER` database functions declare explicit `SET search_path = public, auth;`.
- [ ] `[REQUIRED]` Public execute privileges revoked from sensitive database functions (`REVOKE EXECUTE ON FUNCTION ... FROM PUBLIC;`).
- [ ] `[REQUIRED]` Sensitive user files placed in **Private** storage buckets.
- [ ] `[REQUIRED]` Storage bucket RLS policies restrict read/write access to authorized user folders.
- [ ] `[REQUIRED]` File upload MIME-type whitelist and size limits enforced on all storage buckets.

---

### Verification Sign-Off
- **Assessed By:**
- **Date:**
- **Evidence Summary:**

