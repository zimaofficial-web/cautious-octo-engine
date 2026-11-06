---
name: code-quality-review
description: >-
  Use this skill to perform a code quality review, identify anti-patterns, evaluate readability and maintainability, audit cyclomatic complexity, and detect premature abstraction or code duplication.
---

# Code Quality Review Skill

## 1. Purpose
Performs an analytical source code review to identify anti-patterns, maintainability risks, excessive cyclomatic complexity, premature abstractions, type-safety loopholes, and dead code.

## 2. When to Use
- Reviewing pull requests and code modifications before merge.
- Refactoring legacy files or cleaning up complex functions.
- Performing periodic code health and linting audits.

## 3. Required Inputs
- Source code files and test suites.
- Static analysis and linter configuration files (`tsconfig.json`, `.eslintrc`, Biome).
- Git diff or specific file paths to evaluate.

## 4. Step-by-Step Inspection Procedure
1. **Type Safety & Strictness:**
   - Verify strict typing (`noImplicitAny: true`). Flag any usage of `any` types that bypass compiler safety; replace with generics, discriminated unions, or `unknown`.
2. **Readability & Cognitive Load:**
   - Check function lengths: functions should fit within a single viewport ($\le 30–40$ lines).
   - Evaluate cyclomatic complexity: flag deeply nested `if/else` ladders ($>3$ levels); recommend early returns / guard clauses.
3. **DRY vs. Premature Abstraction:**
   - Apply the **Rule of Three**: Do not create complex generic abstractions for two similar lines of code. Tolerate clean duplication until a pattern clearly repeats 3 times.
   - Flag "clever" one-liners that compromise readability for brevity.
4. **Naming Intentionality:**
   - Confirm variable and function names reveal intent without requiring inline comments (e.g., `isEligibleForDiscount` instead of `flag` or `check()`).
5. **Side Effect & Mutation Control:**
   - Ensure pure utility functions do not mutate external state or arguments passed by reference.
6. **Dead Code & Comment Hygiene:**
   - Detect commented-out dead code blocks (remove them; git tracks history).
   - Verify comments explain *why* an unusual technical choice was made, not *what* the obvious code is doing.

## 5. Common Oversights
- Premature abstraction introducing complex inheritance or meta-programming for simple tasks.
- Catching exceptions and silently swallowing them (`catch (e) {}`).
- Deeply nested conditional logic creating hidden execution paths.
- Overusing type assertions (`as unknown as Type`) to bypass TypeScript errors.

## 6. Output Report Format
```markdown
# Code Quality & Maintainability Report

## 1. Code Health & Complexity Score
- **Maintainability Index:** [High / Moderate / Needs Refactoring]
- **Type Safety Rating:** [Strict / Leaky]

## 2. Anti-Patterns & Code Smells
- **[CRITICAL] Type Loophole:** `file.ts:24` [Bypassing type checker with `any`]
- **[WARNING] High Complexity:** `file.ts:60` [Cyclomatic complexity > 8; 4 nested loops/conditions]

## 3. Readability & Refactoring Opportunities
- `function_name()`: [Refactor nested if-statements to guard clauses]
- `module_name`: [Eliminate dead commented-out code]

## 4. Remediation Diff Examples
```typescript
// Before: Nested complexity
if (user) {
  if (user.isActive) {
    if (hasPermission) { ... }
  }
}

// After: Clean guard clauses
if (!user || !user.isActive || !hasPermission) return;
// proceed...
```
```

## 7. Related Documentation
- [Architecture & System Design Standards](../../01-engineering/01-architecture-system-design/standards.md)
- [Frontend Engineering Standards](../../01-engineering/02-frontend-engineering/standards.md)
- [Backend Engineering Standards](../../01-engineering/03-backend-engineering/standards.md)

