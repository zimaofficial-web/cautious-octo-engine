# Web Accessibility (A11y) Standards

## 1. Compliance Baseline & Core Philosophy

- **Standard:** All digital interfaces must comply with **WCAG 2.1 & 2.2 Level AA** as an absolute minimum standard.
- **The First Rule of ARIA:** *Do not use ARIA if a native HTML element or attribute with the semantics and behavior already exists.*
  - Use `<button>` instead of `<div role="button" tabindex="0">`.
  - Use `<nav>`, `<main>`, `<header>`, `<footer/>`, `<aside>` instead of generic `<div>` landmarks.
  - Use `<details>` and `<summary>` for native disclosure accordions.

---

## 2. Color Contrast Ratios (WCAG AA)

| Element Type | Minimum Contrast Ratio | Evaluation Standard |
| :--- | :--- | :--- |
| **Normal Body Text** (<18pt / <14pt bold) | **$4.5:1$** | Foreground text vs. Background canvas |
| **Large Text** ($\ge$18pt / $\ge$14pt bold) | **$3.0:1$** | Headlines, large titles vs. Background |
| **UI Components & Borders** | **$3.0:1$** | Active input borders, checkboxes, button boundaries |
| **Icons & Graphical Objects** | **$3.0:1$** | Meaningful icons conveying status or action |

- **Color is Never the Sole Indicator:** Never convey information, errors, or states through color alone:
  - Error fields must feature both a red border AND an icon/text error message.
  - Links inside prose paragraphs must be underlined or have high contrast with surrounding text.

---

## 3. Keyboard Navigation & Focus Management

### 3.1 Unbroken Keyboard Operability
- **All Actions Reachable:** Every interactive element must be operable using only the keyboard (`Tab`, `Shift+Tab`, `Enter`, `Space`, `Arrow` keys).
- **Logical Tab Order:** DOM order must mirror visual reading order. Never rearrange components with CSS `order` or absolute positioning in a way that creates jarring keyboard jumps.
- **No Focus Traps:** Focus must never become trapped in an element unless it is a modal dialog.
- **Skip Links:** Provide a hidden "Skip to main content" link as the first focusable element on every page to allow keyboard users to bypass repetitive navigation bars.

### 3.2 Visible Focus Indicators
- **Never Suppress Outlines:** `outline: none` is strictly prohibited unless immediately paired with an enhanced, high-contrast `:focus-visible` styling.
- **Focus Indicator Standard:** Focus rings must have a minimum 2px width and a contrast ratio $\ge 3:1$ against adjacent backgrounds.

### 3.3 Modal Dialog Focus Protocol
When a modal or drawer opens:
1. **Focus Trap:** Confine keyboard `Tab` navigation strictly within the dialog elements.
2. **Initial Focus:** Direct focus to the first interactive field or close button inside the modal.
3. **Escape Key Dismissal:** Pressing `Escape` must immediately close the dialog.
4. **Focus Restoration:** Upon closing, focus must be returned to the triggering element that opened the dialog.

---

## 4. Screen Reader Compatibility & Assistive Tech

### 4.1 Heading Structure
- Maintain a strict hierarchical heading order: `<h1>` $\rightarrow$ `<h2>` $\rightarrow$ `<h3>`.
- Never skip heading levels (e.g., jumping from `<h2>` directly to `<h4>` for styling convenience; use CSS classes for visual sizing).
- Exactly one `<h1>` per page representing the core subject.

### 4.2 Image Alternative Text (`alt`)
- **Informative Images:** `alt="Clear concise description of what the image shows"` (e.g., `alt="Quarterly revenue chart showing 24% growth in Q3"`).
- **Purely Decorative Images:** Explicitly declare `alt=""` or `aria-hidden="true"` so screen readers ignore them without reading raw filenames.
- **Avoid Redundant Words:** Do not include "image of" or "picture of" in the alt text.

### 4.3 Accessible Links & Buttons
- **Descriptive Text:** Link text must be meaningful out of context. Never use "Click here", "Read more", or "Learn more".
  - *Bad:* `<a href="/pricing">Learn more</a>`
  - *Good:* `<a href="/pricing">View enterprise pricing plans</a>`
- **Icon-Only Buttons:** Any button displaying only an icon must include an accessible label via `aria-label` or an internal visually hidden span (`sr-only` class):
  ```html
  <button aria-label="Close dialog">
    <XIcon aria-hidden="true" />
  </button>
  ```

---

## 5. Accessible Forms & Error Feedback

- **Explicit Labeling:** Every form input must have a programmatic `<label>` bound via `htmlFor="input_id"` and `id="input_id"`.
  - Placeholder text is **never** a replacement for a visible `<label>`.
- **Descriptive Error Binding:**
  - Connect error messages to inputs using `aria-describedby="error_id"`.
  - Mark failing fields with `aria-invalid="true"`.
- **Live Announcements:** Dynamic validation errors and toast notifications must use ARIA live regions:
  ```html
  <div role="alert" aria-live="polite">
    Please enter a valid business email address.
  </div>
  ```

---

## 6. Motion & Cognitive Preferences

- Always implement `@media (prefers-reduced-motion: reduce)` to disable autoplay videos, spinning loaders, and parallax shifts for users with motion sensitivity.
- Never use blinking or flashing content that flashes more than three times per second (prevents photosensitive seizures).

