---
name: accessibility-audit
description: >-
  Use this skill to audit web interfaces for accessibility (A11y), verify WCAG 2.1/2.2 Level AA compliance, test keyboard navigation and focus management, and inspect screen reader compatibility.
---

# Accessibility Audit Skill

## 1. Purpose
Performs an automated and heuristic accessibility evaluation of digital interfaces against **WCAG 2.1 & 2.2 Level AA** standards to ensure barriers are eliminated for keyboard users, screen reader users, and individuals with visual or motor impairments.

## 2. When to Use
- Prior to launching any public web application, customer portal, or design system update.
- Auditing interactive components (modal dialogs, dropdowns, accordions, tabs).
- Verifying legal and regulatory compliance (Section 508, ADA, European Accessibility Act).

## 3. Required Inputs
- HTML markup, JSX/TSX component files, and rendered DOM structures.
- Color palette tokens with contrast calculations.
- Lighthouse / Axe accessibility automated test output.

## 4. Step-by-Step Inspection Procedure
1. **Semantic HTML & First Rule of ARIA:** Verify native HTML elements (`<button>`, `<dialog>`, `<nav>`, `<main>`) are used. Flag any `<div role="button">` or click handlers attached to non-interactive elements.
2. **Color Contrast Verification (WCAG AA):**
   - Normal text ($<18\text{pt}$): Confirm contrast ratio $\ge 4.5:1$ against background.
   - Large text ($\ge 18\text{pt}$ or $\ge 14\text{pt}$ bold): Confirm contrast ratio $\ge 3.0:1$.
   - Interactive borders and icons: Confirm contrast ratio $\ge 3.0:1$.
   - Verify color is never the sole indicator of an error or status.
3. **Keyboard Operability & Focus Management:**
   - Verify every interactive element is reachable via `Tab` / `Shift+Tab` in logical DOM reading order.
   - Confirm visible, high-contrast focus rings ($\ge 2\text{px}$) are present (`:focus-visible`). Flag any `outline: none` without an active replacement.
   - Test modal dialogs: Confirm focus is trapped within the dialog, initial focus is set, `Escape` key dismisses the modal, and focus is restored to the triggering element upon close.
4. **Heading Hierarchy & Document Landmarks:**
   - Ensure exactly one `<h1>` per view. Verify headings do not skip levels (`h1` $\rightarrow$ `h2` $\rightarrow$ `h3`).
   - Check that landmark regions (`<header>`, `<main>`, `<footer>`, `<aside>`) encompass all content.
5. **Form Accessibility & Error Binding:**
   - Confirm every input has an explicit programmatic `<label for="...">`.
   - Verify field errors use `aria-invalid="true"` and `aria-describedby="error_id"`.
   - Check dynamic error announcements using `role="alert"` or `aria-live="polite"`.
6. **Images & Non-Text Content:**
   - Verify informative images have concise, descriptive `alt` text.
   - Verify decorative images explicitly declare `alt=""` or `aria-hidden="true"`.
   - Verify icon-only buttons include an accessible label (`aria-label`).

## 5. Common Oversights
- Suppressing browser focus rings (`outline: none`), leaving keyboard users blind.
- Building custom dropdowns or modals without keyboard trap or escape key handling.
- Using placeholder text as a substitute for visible `<label>` elements.
- Meaningful charts or diagrams lacking alternative text or data table equivalents.

## 6. Output Report Format
```markdown
# Accessibility (WCAG 2.1/2.2 AA) Audit Report

## 1. Compliance Score & Summary
- **WCAG Level AA Status:** [PASS / CONDITIONAL PASS / NON-COMPLIANT]
- **Automated Issues Detected:** [X Violations]

## 2. Critical A11y Violations (Blockers)
- **[CRITICAL] Contrast Failure:** [Element, contrast ratio X:1 vs. required 4.5:1]
- **[CRITICAL] Focus Trap Missing:** [Modal X does not trap focus or restore on close]

## 3. Screen Reader & Semantic Deficiencies
- `component_name`: [Icon button missing aria-label]
- `form_name`: [Inputs missing programmatic labels]

## 4. Remediation Steps
- [ ] Add explicit label to input field X
- [ ] Update button contrast from #71717a to #52525b
```

## 7. Related Documentation
- [Accessibility Standards](../../01-engineering/09-accessibility/standards.md)
- [UI/UX Design Standards](../../01-engineering/08-ui-ux-design-quality/standards.md)
- [Accessibility Verification Checklist](../../04-checklists/accessibility-verification.md)

