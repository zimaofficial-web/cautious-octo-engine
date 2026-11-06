# Supabase Architecture & Security Standards

> [!CAUTION]
> **CRITICAL SECURITY RULE:** Enabling Supabase Authentication does **NOT** automatically protect your database tables or storage buckets. A table without explicit Row Level Security (RLS) policies is either completely inaccessible or completely vulnerable to unauthorized access via the Supabase Data API.

---

## 1. Row Level Security (RLS) Protocol

### 1.1 Mandatory Table Hardening
Every user-facing table created in Supabase must immediately have RLS enabled:
```sql
-- 1. Enable RLS
ALTER TABLE public.projects ENABLE ROW LEVEL SECURITY;

-- 2. Force RLS for table owners (prevents bypass by non-superusers)
ALTER TABLE public.projects FORCE ROW LEVEL SECURITY;
```

### 1.2 Granular Policy Definition
Never create catch-all `FOR ALL` policies unless the read/write permissions are genuinely identical. Always create separate policies for each operation:
```sql
-- READ (SELECT): Users can view their own projects or shared workspace projects
CREATE POLICY "Users can read own projects"
ON public.projects
FOR SELECT
TO authenticated
USING (
  auth.uid() = user_id OR 
  EXISTS (
    SELECT 1 FROM public.workspace_members wm 
    WHERE wm.workspace_id = projects.workspace_id 
    AND wm.user_id = auth.uid()
  )
);

-- CREATE (INSERT): Users can create projects where user_id matches their auth ID
CREATE POLICY "Users can create own projects"
ON public.projects
FOR INSERT
TO authenticated
WITH CHECK (
  auth.uid() = user_id
);

-- UPDATE: Users can only update their own projects
CREATE POLICY "Users can update own projects"
ON public.projects
FOR UPDATE
TO authenticated
USING (auth.uid() = user_id)
WITH CHECK (auth.uid() = user_id);

-- DELETE: Users can only delete their own projects
CREATE POLICY "Users can delete own projects"
ON public.projects
FOR DELETE
TO authenticated
USING (auth.uid() = user_id);
```

### 1.3 RLS Performance Optimization
- **Index Foreign Keys Used in Policies:** If an RLS policy references `projects.user_id` or queries `workspace_members.user_id`, those columns **MUST** have an index. Otherwise, every single row access causes a full table scan.
- **Wrap `auth.uid()` in `SELECT` for Optimization:** In Postgres query planning, using `(SELECT auth.uid())` prevents the engine from re-evaluating the authentication function for every single evaluated row:
  ```sql
  -- High performance pattern
  USING ((SELECT auth.uid()) = user_id)
  ```

---

## 2. API Keys & Service-Role Security

### 2.1 The Two Keys
- **`anon` (Public) Key:** Safe to expose in the browser, mobile client, and frontend `.env` files. Access is strictly constrained by your RLS policies.
- **`service_role` (Secret) Key:** Bypasses **ALL** Row Level Security policies completely. Grants full read/write admin access to the entire database.

### 2.2 Strict Protection Rules
- **ZERO Client Exposure:** The `service_role` key must **NEVER** appear in client-side code, git repositories, Vite/Next.js client bundles, mobile app bundles, or public `.env` files.
- **Server-Only Use:** Only use the `service_role` key inside secure server environments, backend microservices, or Supabase Edge Functions when performing legitimate admin maintenance.
- **Environment Variable Auditing:** Never prefix a service-role secret with `NEXT_PUBLIC_` or `VITE_`.

---

## 3. Database Functions & PL/pgSQL Security

### 3.1 `SECURITY DEFINER` vs `SECURITY INVOKER`
- **`SECURITY INVOKER` (Default & Preferred):** The function executes with the permissions of the calling user. RLS policies apply.
- **`SECURITY DEFINER` (Elevated Admin):** The function executes with the permissions of the function creator (superuser). Bypasses RLS.

### 3.2 Hardening `SECURITY DEFINER` Functions
Whenever a function must be `SECURITY DEFINER` (e.g., creating a profile record upon auth sign-up):
1. **Always explicitly set `search_path`:** Failing to set `search_path` leaves the function open to malicious schema hijacking attacks.
2. **Revoke default public permissions:**
```sql
CREATE OR REPLACE FUNCTION public.handle_new_user()
RETURNS TRIGGER 
SECURITY DEFINER
SET search_path = public, auth
LANGUAGE plpgsql
AS $$
BEGIN
  INSERT INTO public.profiles (id, email, full_name, created_at)
  VALUES (
    NEW.id,
    NEW.email,
    NEW.raw_user_meta_data->>'full_name',
    NOW()
  );
  RETURN NEW;
END;
$$;

-- Revoke execute from public
REVOKE EXECUTE ON FUNCTION public.handle_new_user() FROM PUBLIC;
```

---

## 4. Storage Buckets & File Security

- **Public vs. Private Buckets:**
  - Avatars and public marketing assets can live in **Public** buckets.
  - Invoices, contracts, identity documents, and private user files must reside in **Private** buckets.
- **Storage RLS Policies:**
  - Files in `storage.objects` are secured via RLS policies on the `storage.objects` table.
  - Validate folder hierarchy access (e.g., enforcing that `(storage.foldername(name))[1] = auth.uid()::text`).
- **Upload Restrictions:**
  - Always enforce `allowed_mime_types` (e.g., `['image/png', 'image/jpeg', 'application/pdf']`) and `file_size_limit` (e.g., 5MB max) at the bucket configuration level to prevent malicious file injection and denial-of-service storage exhaustion.

---

## 5. Edge Functions & Secrets Management

- **Isolated Runtime:** Supabase Edge Functions run in Deno. Use them for third-party integrations (Stripe webhooks, Resend emails) where backend secrets are required.
- **Secrets Storage:** Store API tokens and private keys using the Supabase CLI:
  `supabase secrets set STRIPE_SECRET_KEY=sk_live_...`
- **Webhook Signature Verification:** Always verify webhook signatures (e.g., `stripe.webhooks.constructEvent`) before trusting payload contents.

---

## 6. Connection Pooling & Migrations

- **Connection Modes:**
  - **Transaction Mode (Port 6543 / Supavisor):** Use for serverless functions (Vercel, AWS Lambda, Edge Functions) where thousands of ephemeral connections connect and disconnect rapidly.
  - **Session Mode (Port 5432):** Use for long-running server processes, direct database migrations, and tools that require prepared statements or session-level advisory locks.
- **Local Development Workflow:**
  - Develop locally with the Supabase CLI (`supabase start`, `supabase db diff`).
  - Never execute ad-hoc schema modifications in production via the Supabase Studio dashboard without committing corresponding migration files to Git.

