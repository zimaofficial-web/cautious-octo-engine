---
name: database-architecture-review
description: >-
  Use this skill to audit database schemas, relational modeling, foreign key constraints, indexing strategies, query efficiency, migration safety, and transaction boundaries.
---

# Database Architecture Review Skill

## 1. Purpose
Provides a rigorous audit of relational database schemas, data integrity constraints, index optimization, query execution plans, and migration safety to prevent performance bottlenecks and data corruption.

## 2. When to Use
- Proposing or reviewing new database tables, columns, or relations.
- Auditing existing schemas for missing indexes and referential integrity violations.
- Investigating slow database queries or database locking issues.

## 3. Required Inputs
- SQL schema definitions (`schema.sql`, Prisma schema, Drizzle schema, or migration files).
- Slow query logs or `EXPLAIN ANALYZE` output.
- Data access patterns and anticipated read/write volumes.

## 4. Step-by-Step Inspection Procedure
1. **Relational Integrity & Normalization:** Verify baseline 3NF normalization. Confirm every table has an explicit Primary Key (UUIDv4/v7 or BigInt).
2. **Foreign Key & Cascade Audit:** Ensure all relations enforce database-level Foreign Keys with explicit `ON DELETE` behaviors (`RESTRICT`, `CASCADE`, `SET NULL`).
3. **Constraint Enforcement:** Check that columns are `NOT NULL` by default. Verify business logic bounds use `CHECK` and `UNIQUE` constraints.
4. **Index Optimization & Foreign Keys:**
   - Confirm every Foreign Key column has a supporting index.
   - Audit compound indexes for leftmost column usage.
   - Check for partial/filtered indexes on high-frequency status queries.
5. **N+1 Query & Access Pattern Inspection:** Identify code loops generating individual queries instead of batch joins or eager loading.
6. **Migration Safety & Locking:** Verify migrations do not lock production tables (e.g., ensuring indexes are created `CONCURRENTLY` in Postgres).

## 5. Common Oversights
- Missing indexes on foreign key columns, causing full table scans during joins and deletes.
- Nullable columns that should strictly be `NOT NULL`.
- Unbounded `SELECT *` queries consuming excessive server memory.
- Lack of atomic database transactions for multi-step mutations.

## 6. Output Report Format
```markdown
# Database Architecture Review Report

## 1. Schema & Referential Integrity Assessment
[Evaluation of 3NF compliance, constraints, and relationships]

## 2. Missing Indexes & Performance Risks
- **[CRITICAL] Missing Index:** `table_name(foreign_key_column)`
- **[WARNING] Query Inefficiency:** [Unindexed compound query or table scan]

## 3. Constraint & Data Integrity Deficiencies
- `table_name.column`: [Should be NOT NULL with default]
- `table_name.column`: [Missing CHECK constraint]

## 4. Migration & Concurrency Recommendations
- [Add index CONCURRENTLY recommendation]
- [Transaction boundary fix for checkout workflow]
```

## 7. Related Documentation
- [Database Engineering Standards](../../01-engineering/04-database-engineering/standards.md)
- [Supabase Security Standards](../../01-engineering/05-supabase/standards.md)
- [Database Readiness Checklist](../../04-checklists/database-readiness.md)

