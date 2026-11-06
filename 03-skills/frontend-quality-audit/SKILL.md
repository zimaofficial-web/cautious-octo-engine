---
name: frontend-quality-audit
description: >-
  Use this skill to audit frontend codebases, React components, state management patterns, form lifecycles, web performance (Core Web Vitals), and client-side error handling.
---

# Frontend Quality Audit Skill

## 1. Purpose
Performs a rigorous code and performance audit on client-side applications, evaluating component decomposition, state isolation, rendering efficiency, form resilience, and client security.

## 2. When to Use
- Reviewing frontend pull requests or newly developed UI features.
- Diagnosing slow page renders, layout shifts (CLS), or sluggish interactions.
- Prior to releasing major client-facing web applications.

## 3. Required Inputs
- Component source files (`.tsx`, `.jsx`, `.vue`, `.svelte`).
- State management stores and API fetching hooks (`tanstack-query`, `zustand`, etc.).
- Styling definitions (Tailwind classes, CSS Modules, or CSS files).

## 4. Step-by-Step Inspection Procedure
1. **Component Decomposition & Modularity:** Verify components adhere to the Single Responsibility Principle ($\le 200$ lines). Identify bloated container components.
2. **State Categorization Review:**
   - Confirm server data is cached via dedicated libraries (TanStack Query/SWR) rather than duplicated into global client stores.
   - Verify filter/pagination state lives in URL query params.
3. **Rendering & Performance Profiling:**
   - Identify unnecessary re-renders, unstable object/array prop references, and missing keys in lists.
   - Check if long lists (>100 items) utilize virtualization.
   - Verify images specify explicit dimensions or `aspect-ratio` to prevent layout shifts.
4. **Form & Input Lifecycle Audit:** Ensure all forms explicitly handle 5 states: pristine, dirty, submitting, success, and error.
5. **Resilience & Graceful Degradation:** Check for Error Boundaries around major views and verify skeleton loaders prevent blank screens.
6. **Frontend Security Check:** Search for `dangerouslySetInnerHTML`, unescaped user inputs, and exposed API secrets in client bundles.

## 5. Common Oversights
- Duplicating remote API response state into Redux/Zustand.
- Unhandled empty states when queries return zero results.
- Overusing `useEffect` for derived state calculations.
- Missing keyboard focus rings on custom buttons/inputs.

## 6. Output Report Format
```markdown
# Frontend Quality Audit Report

## 1. Executive Summary & Health Score
[Overview of component health, performance risks, and UX polish]

## 2. Critical Findings & Anti-Patterns
- **[CRITICAL] Issue 1:** [Location, issue description, impact]
- **[WARNING] Issue 2:** [Location, issue description, impact]

## 3. Performance & Core Web Vitals Analysis
- **LCP Risk:** [Preloading hero assets / render blocking]
- **CLS Risk:** [Missing aspect-ratios / dynamic layout shifts]
- **INP / Re-render Risk:** [Expensive unmemoized renders]

## 4. Remediation Checklist
- [ ] Refactor component X into compound components
- [ ] Migrate server state in Y to TanStack Query
```

## 7. Related Documentation
- [Frontend Engineering Standards](../../01-engineering/02-frontend-engineering/standards.md)
- [UI/UX Design Standards](../../01-engineering/08-ui-ux-design-quality/standards.md)
- [Frontend Readiness Checklist](../../04-checklists/frontend-readiness.md)

