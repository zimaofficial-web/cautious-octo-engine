# Database Readiness Checklist

> [!IMPORTANT]
> **VERIFICATION MANDATE:** Cite SQL schemas, migration scripts, and index definitions for every assertion.
> Status tags: `[REQUIRED]`, `[RECOMMENDED]`, `[OPTIONAL]`, `[N/A]`, `[REQUIRES HUMAN VERIFICATION]`.

---

## 1. Schema Design & Constraints
- [ ] `[REQUIRED]` Baseline 3NF normalization verified.
- [ ] `[REQUIRED]` Explicit Primary Key defined on every table (UUIDv4/v7 or BigInt Identity).
- [ ] `[REQUIRED]` Foreign key constraints enforced on all relationships with explicit `ON DELETE` rules (`RESTRICT`, `CASCADE`, `SET NULL`).
- [ ] `[REQUIRED]` `NOT NULL` default: Columns are non-nullable unless explicitly justified.
- [ ] `[REQUIRED]` Business bounds enforced via SQL `CHECK` and `UNIQUE` constraints.

## 2. Indexing Strategy & Query Optimization
- [ ] `[REQUIRED]` Every foreign key column has a dedicated supporting index.
- [ ] `[REQUIRED]` Compound indexes adhere to the leftmost prefix rule matching actual query filters.
- [ ] `[REQUIRED]` No unindexed table scans on large tables detected via `EXPLAIN ANALYZE`.
- [ ] `[REQUIRED]` Zero N+1 query loops in application ORM code (eager loading or batch joins used).
- [ ] `[RECOMMENDED]` Partial indexes configured for high-frequency filtered status queries.

## 3. Concurrency, Migrations & Disaster Recovery
- [ ] `[REQUIRED]` Multi-step mutations executed within explicit atomic database transactions (`BEGIN ... COMMIT`).
- [ ] `[REQUIRED]` Schema migrations version-controlled and forward-migrating.
- [ ] `[REQUIRED]` Zero-downtime expand/contract pattern used for breaking column/table changes.
- [ ] `[REQUIRED]` Indexes created concurrently (`CREATE INDEX CONCURRENTLY` in Postgres) to avoid locking production tables.
- [ ] `[REQUIRED]` Automated backups verified with tested restore drill.

---

### Verification Sign-Off
- **Assessed By:**
- **Date:**
- **Evidence Summary:**

