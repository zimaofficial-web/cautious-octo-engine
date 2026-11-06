---
name: ui-ux-design-audit
description: >-
  Use this skill to audit interface design quality, evaluate visual hierarchy, verify the 8pt spacing grid, inspect typography scales, review dark mode implementation, and eliminate generic AI design clichés.
---

# UI/UX Design Audit Skill

## 1. Purpose
Conducts an aesthetic and usability design review of web and mobile interfaces to ensure intentional visual hierarchy, rigorous design token usage (spacing, typography, color), intuitive interaction states, and eradication of generic AI-generated aesthetics.

## 2. When to Use
- Reviewing UI components, wireframes, or frontend views before design sign-off.
- Critiquing an interface that looks unpolished, inconsistent, or distinctly "AI-generated".
- Auditing dark mode transitions and component interactive feedback states.

## 3. Required Inputs
- Component markup and stylesheets (CSS, Tailwind classes, Figma design tokens).
- Screen mockups, live UI preview, or visual artifacts.
- Target brand identity guidelines and color palette specifications.

## 4. Step-by-Step Inspection Procedure
1. **Generic AI Aesthetic Scan:** Search for and eliminate overused AI design clichés: arbitrary neon purple/cyan gradients, unreadable heavy glassmorphic blurs, floating disconnected blobs, and generic hero cards.
2. **Visual Hierarchy & Typographic Scale:**
   - Verify font sizes adhere to a defined ratio (Major Second, Minor Third, etc.).
   - Confirm heading levels (`h1`, `h2`, `h3`) show distinct contrast in weight and size.
   - Verify line length for body text is constrained to 45–75 characters (600px–750px max width).
3. **Spatial Grid System Verification:** Confirm all margins, paddings, and component heights adhere strictly to the 8pt grid (4px, 8px, 16px, 24px, 32px, 48px, 64px). Flag arbitrary pixel values (e.g., `margin-top: 17px`).
4. **Color Distribution & Semantic Tokens:**
   - Verify the 60-30-10 distribution rule (60% canvas background, 30% structural panels/typography, 10% intentional accent).
   - Check that functional statuses (success, warning, error) use semantic tokens rather than arbitrary colors.
5. **Interactive Component States:** Audit interactive controls (buttons, inputs, dropdowns) to confirm all 6 states are styled: default, hover, focus-visible, active/pressed, disabled, and loading.
6. **Dark Mode Execution:**
   - Ensure the dark theme uses deep slate/charcoal surfaces (`#0f172a`, `#121212`) rather than pure `#000000`.
   - Verify elevation is communicated through increasing surface lightness rather than invisible drop shadows.

## 5. Common Oversights
- Inconsistent spacing where components lack a shared baseline grid.
- Overusing saturated accent colors for non-interactive elements, creating visual clutter.
- Pure black dark mode causing eye strain.
- Discarding button dimensions during loading states, causing jarring layout shifts.

## 6. Output Report Format
```markdown
# UI/UX Design Quality Audit Report

## 1. Visual Hierarchy & Aesthetic Score
- **Design Intentionality:** [High / Moderate / Needs Polish / Generic AI Look]
- **Grid & Spacing Adherence:** [Consistent 8pt / Multiple arbitrary values]

## 2. Visual Flaws & Anti-Patterns
- **[CRITICAL] Issue 1:** [Location, arbitrary spacing/color token violation, fix]
- **[WARNING] Issue 2:** [Location, weak contrast / missing interactive state, fix]

## 3. Dark Mode & Theming Audit
- [Contrast evaluation / elevation surface checks]

## 4. Actionable Polish Checklist
- [ ] Align margins in component X to 16px/24px tokens
- [ ] Add explicit loading skeleton to component Y
```

## 7. Related Documentation
- [UI/UX & Design Quality Standards](../../01-engineering/08-ui-ux-design-quality/standards.md)
- [Accessibility Standards](../../01-engineering/09-accessibility/standards.md)
- [Frontend Quality Audit Skill](../frontend-quality-audit/SKILL.md)

