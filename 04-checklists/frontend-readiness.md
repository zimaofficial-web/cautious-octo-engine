# Frontend Readiness Checklist

> [!IMPORTANT]
> **VERIFICATION MANDATE:** Cite component files, state configurations, and lighthouse metrics for every assertion.
> Status tags: `[REQUIRED]`, `[RECOMMENDED]`, `[OPTIONAL]`, `[N/A]`, `[REQUIRES HUMAN VERIFICATION]`.

---

## 1. Component Architecture & Code Standards
- [ ] `[REQUIRED]` Strict TypeScript mode enabled with zero unhandled `any` types.
- [ ] `[REQUIRED]` Component Single Responsibility: No UI component exceeds 200 lines without sub-component decomposition.
- [ ] `[REQUIRED]` Server state managed via caching libraries (TanStack Query/SWR) rather than duplicated into global Redux/Zustand state.
- [ ] `[RECOMMENDED]` Filter, tab, and pagination states synchronized with URL search params.

## 2. Form Handling & UI Resilience
- [ ] `[REQUIRED]` All forms validate via strict runtime schemas (Zod, Valibot) and handle all 5 states (pristine, dirty, submitting, success, error).
- [ ] `[REQUIRED]` Double-submission prevention: Submit buttons disabled during in-flight network requests.
- [ ] `[REQUIRED]` React Error Boundaries wrapped around all major views and widgets.
- [ ] `[REQUIRED]` Intentional empty states and skeleton loaders implemented (no blank white flashes during loads).

## 3. Web Performance & Security
- [ ] `[REQUIRED]` Cumulative Layout Shift (CLS) $\le 0.1$: Explicit dimensions or aspect-ratios set on all images and dynamic media.
- [ ] `[REQUIRED]` Zero unescaped user inputs or unsanitized `dangerouslySetInnerHTML` occurrences.
- [ ] `[REQUIRED]` Zero secret API keys or service-role tokens exposed in frontend client bundles or `.env` files.
- [ ] `[RECOMMENDED]` Route-level code splitting configured via dynamic imports.
- [ ] `[RECOMMENDED]` Long list virtualization implemented for lists exceeding 100 DOM nodes.

---

### Verification Sign-Off
- **Assessed By:**
- **Date:**
- **Evidence Summary:**

