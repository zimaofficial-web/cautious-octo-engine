# Accessibility (WCAG 2.1/2.2 AA) Verification Checklist

> [!IMPORTANT]
> **VERIFICATION MANDATE:** Cite color contrast ratios, screen reader tests, and keyboard navigation traces.
> Status tags: `[REQUIRED]`, `[RECOMMENDED]`, `[OPTIONAL]`, `[N/A]`, `[REQUIRES HUMAN VERIFICATION]`.

---

## 1. Visual Presentation & Color Contrast
- [ ] `[REQUIRED]` Contrast ratio $\ge 4.5:1$ verified for all normal body text (<18pt / <14pt bold).
- [ ] `[REQUIRED]` Contrast ratio $\ge 3.0:1$ verified for large text ($\ge 18\text{pt}$ or $\ge 14\text{pt}$ bold) and active UI borders/icons.
- [ ] `[REQUIRED]` Color is never the sole visual indicator of an error, selection, or status.
- [ ] `[REQUIRED]` `@media (prefers-reduced-motion: reduce)` respected (disabling non-essential motion, video autoplay, and parallax).

## 2. Keyboard Navigation & Focus Management
- [ ] `[REQUIRED]` 100% of interactive elements operable via keyboard alone (`Tab`, `Enter`, `Space`, `Arrows`).
- [ ] `[REQUIRED]` High-contrast visible focus rings ($\ge 2\text{px}$) present on all interactive elements (`:focus-visible`).
- [ ] `[REQUIRED]` Zero `outline: none` occurrences without immediate replacement.
- [ ] `[REQUIRED]` "Skip to main content" link present as the first focusable element on every page.
- [ ] `[REQUIRED]` Modal focus trap verified: Focus trapped inside modal, `Escape` key dismisses modal, and focus restored to trigger element upon closing.

## 3. Screen Reader Compatibility & Semantic HTML
- [ ] `[REQUIRED]` Native HTML elements used (`<button>`, `<dialog>`, `<nav>`, `<main>`) rather than `<div>` soup.
- [ ] `[REQUIRED]` Exactly one `<h1>` per view; heading hierarchy does not skip levels (`h1` $\rightarrow$ `h2` $\rightarrow$ `h3`).
- [ ] `[REQUIRED]` Meaningful `alt` text provided on informative images; decorative images explicitly declare `alt=""` or `aria-hidden="true"`.
- [ ] `[REQUIRED]` Icon-only buttons include accessible names via `aria-label` or `sr-only` text.
- [ ] `[REQUIRED]` Form inputs bound to programmatic `<label for="...">` elements (placeholder text is never a substitute for a label).
- [ ] `[REQUIRED]` Form error messages linked via `aria-describedby` and failing inputs marked `aria-invalid="true"`.

---

### Verification Sign-Off
- **Assessed By:**
- **Date:**
- **Evidence Summary:**

