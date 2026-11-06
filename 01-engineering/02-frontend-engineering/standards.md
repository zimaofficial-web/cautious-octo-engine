# Frontend Engineering Standards

## 1. Core Principles & Code Standards

### 1.1 Languages & Modern Standards
- **TypeScript First:** All production code must use strict TypeScript (`"strict": true`). Avoid `any` types; prefer discriminated unions, generics, and `unknown` with runtime type narrowing.
- **Semantic HTML:** Use native elements (`<main>`, `<nav>`, `<article>`, `<section>`, `<button>`, `<dialog>`) rather than `<div>` soup.
- **CSS Architecture:** Prefer utility-first (Tailwind CSS) or CSS Modules with design tokens. Avoid unscoped global CSS styles or arbitrary inline styles.

---

## 2. Component Architecture

### 2.1 Component Design Rules
- **Single Responsibility Principle:** A component should do one thing well. If a component exceeds 150–200 lines, extract sub-components or custom hooks.
- **Composition over Inheritance:** Use `children`, slots, and compound component patterns (e.g., `<Select><Select.Trigger/><Select.Content/></Select>`) rather than complex prop drilling or monster config props.
- **Container / Presentational Separation:**
  - **Presentational Components:** Pure, UI-focused, receive data and callbacks via props, easily testable in Storybook.
  - **Container / Feature Components:** Connect to hooks, manage side effects, and pass prepared data down.

---

## 3. State Management Standards

### 3.1 State Categorization
Distinguish and isolate the three primary types of state:
1. **Server State (Remote Data):** Cache and synchronize server data using dedicated libraries (TanStack Query, SWR, RTK Query). Do **NOT** duplicate remote API responses into global Redux/Zustand state.
2. **URL State:** Filter, pagination, tab selection, and modal visibility should reside in query params (`?tab=billing&page=2`) to enable bookmarking, browser back navigation, and deep linking.
3. **Local Component State:** Ephemeral UI state (dropdown open/close, hover, input text) belongs in component state (`useState`, `useReducer`).
4. **Global Client State:** Strictly reserved for true cross-cutting application state (e.g., active user theme, sidebar collapsed state, global session state). Use lightweight stores like Zustand.

---

## 4. Form Handling & Validation

- **Controlled vs. Uncontrolled:** Use uncontrolled or hybrid performant form libraries (e.g., React Hook Form) to avoid re-rendering entire forms on every keystroke.
- **Schema-Driven Validation:** Couple form validation with strict schema validators (Zod, Valibot). Validate on blur and re-validate on change after first submission error.
- **Comprehensive Lifecycle States:** Every form must handle and communicate 5 explicit states:
  1. *Pristine / Untouched*
  2. *Dirty / Modified*
  3. *Submitting / In-Flight* (disable submit button, display spinner, prevent double-click)
  4. *Success* (clear feedback, focus management, redirect or reset)
  5. *Error* (field-level inline error messages and summary banner for server-level errors)

---

## 5. UI Resilience: Loading, Empty & Error States

Never leave the user staring at a blank screen or a broken layout:
- **Skeleton Screens:** Prefer skeleton placeholders that mirror the expected content structure over generic full-page spinners to minimize perceived latency.
- **Empty States:** When a list or query returns zero results, display an intentional empty state explaining *why* it's empty and offering a primary action (e.g., "No projects found. Create your first project").
- **Error Boundaries:** Wrap major application regions (navigation, widgets, views) in React Error Boundaries so that a runtime crash in one component does not crash the entire app. Provide a retry mechanism.

---

## 6. Performance Optimization & Core Web Vitals

- **Core Web Vitals Thresholds:**
  - **LCP (Largest Contentful Paint):** $\le 2.5\text{s}$ (Preload hero images, avoid render-blocking scripts).
  - **INP (Interaction to Next Paint):** $\le 200\text{ms}$ (Offload heavy calculations, avoid blocking the main thread).
  - **CLS (Cumulative Layout Shift):** $\le 0.1$ (Always declare explicit `width` and `height` or aspect-ratio on images, media, and dynamic embeds).
- **Bundle Optimization:**
  - Route-based code splitting via dynamic imports (`React.lazy`, Next.js dynamic).
  - Audit bundles regularly; eliminate duplicate lodash/moment instances (prefer `date-fns` or native `Intl`).
- **Rendering Discipline:**
  - Do not prematurely memoize (`useMemo`/`useCallback`) without profiling, but always memoize expensive calculations or stable references passed to memoized children.
  - Virtualize long lists (>100 items) using TanStack Virtual.

---

## 7. Frontend Security

- **XSS Prevention:** Never use `dangerouslySetInnerHTML` unless explicitly sanitized with DOMPurify. Always escape dynamic content.
- **CORS & Credentials:** Ensure cookies use `SameSite=Lax` or `Strict` with `HttpOnly` and `Secure`.
- **Content Security Policy (CSP):** Enforce strict CSP headers preventing unauthorized inline scripts and unauthorized external font/script domains.
- **Sensitive Data Storage:** Never store sensitive API secrets, service-role keys, or raw credentials in `localStorage` or `sessionStorage` (they are vulnerable to XSS extraction).

