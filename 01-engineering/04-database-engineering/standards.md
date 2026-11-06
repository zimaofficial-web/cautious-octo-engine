# Database Engineering Standards

## 1. Relational Modeling & Schema Design

### 1.1 Normalization & Design Philosophy
- **Third Normal Form (3NF) Baseline:** Always start with 3NF to eliminate duplicate data, update anomalies, and maintain strict referential integrity.
- **Pragmatic Denormalization:** Only denormalize for performance after measuring real query bottlenecks under load. Every denormalized field (e.g., cached totals or counts) must have an automated synchronization strategy (database triggers or atomic background jobs).

### 1.2 Primary & Foreign Key Conventions
- **Primary Keys:**
  - Standardize on `UUIDv4` or time-ordered `UUIDv7` (or `ULID`) for distributed scale, public IDs, and to prevent sequential ID enumeration attacks.
  - Use `BIGSERIAL` / `BIGINT GENERATED ALWAYS AS IDENTITY` only for internal lookup tables or high-write append-only logs where compact index size is critical.
- **Foreign Keys:**
  - Foreign keys are mandatory for relational integrity. Never rely solely on application code to enforce relationships.
  - Explicitly define `ON DELETE` behaviors:
    - `ON DELETE RESTRICT` / `NO ACTION` (Default): Prevent deletion of parent if children exist.
    - `ON DELETE CASCADE`: Only for tightly owned child entities (e.g., deleting an Order cascades to OrderItems).
    - `ON DELETE SET NULL`: For optional relationships (e.g., deleting a User sets `assigned_to_id = NULL` on an Issue).

### 1.3 Constraints are Non-Negotiable
- **NOT NULL:** Columns must be `NOT NULL` by default. Only permit `NULL` when an attribute is genuinely optional or inapplicable.
- **CHECK Constraints:** Enforce business bounds directly in SQL (e.g., `CHECK (price >= 0)`, `CHECK (status IN ('draft', 'published', 'archived'))`).
- **UNIQUE Constraints:** Guarantee unique values at the database engine level (e.g., unique email, unique slug per tenant).

---

## 2. Indexing Strategy & Performance

### 2.1 Indexing Best Practices
- **Foreign Key Indexing:** Always create an index on foreign key columns. Relational joins and cascaded deletes suffer severe sequential table scans without them.
- **Compound Indexes (Leftmost Rule):** When querying across multiple columns (e.g., `WHERE tenant_id = ? AND status = ?`), create a composite index `(tenant_id, status)`. Remember that column order matters: queries filtering only by `tenant_id` can use this index, but queries filtering only by `status` cannot.
- **Partial / Filtered Indexes:** When querying a small subset of rows frequently, use partial indexes to conserve disk and memory:
  ```sql
  CREATE INDEX idx_orders_unprocessed ON orders (created_at) WHERE status = 'pending';
  ```
- **Avoid Over-Indexing:** Every index speeds up `SELECT` queries but imposes write, CPU, and disk overhead on every `INSERT`, `UPDATE`, and `DELETE`.

---

## 3. Query Optimization & Preventing N+1 Queries

- **Audit with EXPLAIN ANALYZE:** Always inspect query execution plans for slow queries. Look for sequential scans on large tables, costly nested loops, or excessive disk spills.
- **The N+1 Query Problem:**
  - **Anti-pattern:** Fetching 50 records in query 1, then looping in code to issue 50 separate queries for each record's child relation.
  - **Solution:** Use explicit SQL joins, subquery aggregations, or ORM eager loading (`include` / `preload`).
- **Never Use `SELECT *`:** Always specify explicit column names. This reduces network payload, avoids memory bloat, and prevents bugs when schemas evolve.

---

## 4. Transactions, Concurrency & Data Integrity

- **ACID Guarantees:** Multi-step write operations that must succeed or fail together must be enclosed in an explicit transaction (`BEGIN ... COMMIT`).
- **Race Condition Prevention:**
  - **Atomic Operations:** Prefer atomic database updates: `UPDATE accounts SET balance = balance - 50 WHERE id = ? AND balance >= 50;`
  - **Optimistic Locking:** Use a `version` integer column. On update, check `WHERE id = ? AND version = ?`. If rows affected is 0, someone else updated first; abort and retry.
  - **Pessimistic Locking:** Use `SELECT ... FOR UPDATE` sparingly for critical sections (e.g., seat reservations) to lock rows during a brief transaction.

---

## 5. Safe Schema Migrations

- **Version Controlled Migrations:** All schema changes must be declared in forward-migrating script files tracked in Git.
- **Zero-Downtime Migration Pattern (Expand / Contract):**
  1. *Expand:* Add the new column/table nullable or with default. Deploy application code that reads the old and writes to both old and new.
  2. *Backfill:* Run a background script backfilling old records into the new structure in batches.
  3. *Switch:* Deploy application code that reads from the new column/table.
  4. *Contract:* Drop the old column/table in a subsequent migration after verifying stability.
- **Non-Blocking Operations:** In PostgreSQL, avoid long table locks:
  - Add indexes concurrently: `CREATE INDEX CONCURRENTLY`.
  - Add columns with default values in modern Postgres versions (Postgres 11+ stores defaults in catalog without rewriting table).

---

## 6. Backups, RPO & RTO

- **RPO (Recovery Point Objective):** Maximum acceptable data loss window (e.g., Continuous WAL archiving / Point-in-time recovery allows recovery down to seconds).
- **RTO (Recovery Time Objective):** Maximum acceptable duration to restore database functionality after catastrophic failure.
- **Automated Verification:** An untested backup is not a backup. Automated restore drills must run periodically into an isolated environment to verify backup integrity.

