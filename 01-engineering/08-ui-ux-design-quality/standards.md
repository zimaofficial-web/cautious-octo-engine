# UI/UX & Design Quality Standards

## 1. Intentional Design & Anti-Generic Mandate

> [!WARNING]
> **REJECT GENERIC AI AESTHETICS:** Do not default to clichéd, cookie-cutter AI design patterns (e.g., gratuitous purple/cyan neon gradients, arbitrary glassmorphic cards with heavy blur, unreadable floating blobs, or disconnected illustration styles). Every layout, color choice, and animation must be intentional, functional, and tailored directly to the product's identity and audience.

---

## 2. Visual Hierarchy & Typographic Foundations

### 2.1 The Typographic Scale
- **Harmonic Ratio:** Standardize on a clear typographic scale (Major Second `1.125`, Minor Third `1.2`, or Major Third `1.25`):
  - `Display / Hero`: 36px–48px / Line height: 1.1–1.2
  - `H1 (Page Title)`: 28px–32px / Line height: 1.25
  - `H2 (Section Header)`: 22px–24px / Line height: 1.3
  - `H3 (Card / Subhead)`: 18px–20px / Line height: 1.35
  - `Body (Primary Text)`: 15px–16px / Line height: 1.5–1.6 (Never go below 14px for body prose)
  - `Caption / Metadata`: 12px–13px / Line height: 1.4
- **Font Pairing Restraint:** Use no more than two font families across an entire application:
  - One high-legibility sans-serif for UI elements and data tables (e.g., Inter, Geist, SF Pro).
  - Optional distinct display or serif typeface for editorial or brand headlines.
- **Line Length (Measure):** Keep reading line lengths between 45 and 75 characters (approx. 600px–750px max width for articles) to prevent visual fatigue.

---

## 3. The 8pt Spatial Grid System

- **Linear Spacing Tokens:** All margins, paddings, gaps, and component heights must align to an 8-point grid (with a 4-point sub-grid for tight inline controls):
  - `4px` (xs): Tight icon gaps, badges.
  - `8px` (sm): Internal button padding, small form gaps.
  - `16px` (md): Standard card padding, input padding, standard grid gap.
  - `24px` (lg): Card-to-card spacing, section sub-headers.
  - `32px` (xl): Major component margins.
  - `48px`–`64px` (2xl/3xl): Section margins, hero padding.
- **No Arbitrary Numbers:** Prohibit ad-hoc spacing values (avoid `margin-top: 19px` or `padding: 13px`). Every layout value must map directly to a defined spacing token.

---

## 4. Color Systems & Semantic Tokens

### 4.1 The 60-30-10 Distribution Rule
- **60% Dominant Background / Canvas:** Neutral, low-contrast foundation (light grays/whites or dark slates).
- **30% Secondary Structure:** Cards, sidebars, borders, typography, and surface elevations.
- **10% Intentional Accent:** Call-to-action buttons, active navigation states, key status badges.

### 4.2 Semantic Token Hierarchy
Never hardcode raw hex values into components. Structure palettes into semantic tokens:
- **Surface & Backgrounds:** `surface-primary`, `surface-secondary`, `surface-elevated`
- **Text & Foreground:** `text-primary` (high contrast), `text-secondary` (subtle), `text-muted` (captions)
- **Status / Feedback Colors:**
  - `Success`: Green (confirmation, completion)
  - `Warning`: Amber / Warm Yellow (requires caution, expiring)
  - `Error / Destructive`: Red (deletions, failures, validation errors)
  - `Info`: Blue / Indigo (system announcements, guidance)

---

## 5. Component Consistency & Interactive States

Every interactive element (buttons, form inputs, links, list items) must explicitly support and style all 6 states:
1. **Default / Rest:** Clean, legible, unambiguous affordance.
2. **Hover:** Subtle brightness or surface shift indicating interactability (desktop).
3. **Focus-Visible:** High-contrast 2px outline ring for keyboard navigation.
4. **Active / Pressed:** Physical depression feedback (e.g., `transform: scale(0.98)` or deeper shade).
5. **Disabled:** Reduced opacity (40–50%), `cursor: not-allowed`, pointer events disabled.
6. **Loading:** Spinner replacement or skeleton placeholder preserving original component dimensions to prevent layout shifting.

---

## 6. Micro-Interactions & Animation Standards

- **Purpose-Driven Motion:** Animations must serve a cognitive function:
  - Explain spatial relationships (e.g., drawer sliding in from the right).
  - Direct user attention to state changes (e.g., shake on form error).
  - Smooth visual transitions (e.g., accordion expand/collapse).
- **Duration & Easing Bounds:**
  - Micro-interactions (hover, click, toggle): 150ms–200ms with `ease-out`.
  - Layout transitions (modals, drawers, toasts): 250ms–350ms with `cubic-bezier(0.16, 1, 0.3, 1)`.
  - Never exceed 400ms for routine UI interactions.
- **Respect Motion Preferences:** Always wrap animations in `@media (prefers-reduced-motion: reduce)` to disable non-essential motion for vestibular disorder accessibility.

---

## 7. Dark Mode Architecture

- **Never Invert to Pure Black (`#000000`):** Pure black causes stark eye strain and harsh contrast against white text. Use deep slate or charcoal neutrals (`#0f172a`, `#121212`).
- **Elevation via Surface Lightness:** In light mode, elevation is communicated with drop shadows. In dark mode, drop shadows are virtually invisible; communicate elevation by making higher surfaces slightly lighter:
  - Base background: `#0f172a`
  - Card / Panel: `#1e293b`
  - Elevated Popover / Modal: `#334155`
- **Desaturate Vibrant Accents:** High-saturation bright brand colors vibrate against dark backgrounds. Slightly desaturate accent colors in dark mode to maintain readability and comfort.

