# Core Web Vitals, Lighthouse & Accessibility Playbook

Objective: maximize Lighthouse scores through real engineering improvements while preserving the existing UI, UX, animations, and functionality.

**Targets:** Performance 100 (or highest legitimately achievable) · Accessibility 100 · Best Practices 100 · SEO 100.

- Never manipulate or fake Lighthouse scores.
- Never optimize by removing functionality, animations, accessibility, or design.

---

## LCP (Largest Contentful Paint)

**Identify the actual LCP element before making changes.**

Optimize LCP by:
- ✓ Prioritizing the true LCP image or content
- ✓ Using `next/image` correctly
- ✓ Setting `priority` **only** on the LCP image
- ✓ Providing accurate `sizes`
- ✓ Avoiding oversized images
- ✓ Optimizing image formats (AVIF/WebP when appropriate)
- ✓ Preloading only truly critical assets
- ✓ Optimizing above-the-fold rendering
- ✓ Reducing render-blocking resources
- ✓ Reducing hydration work
- ✓ Moving non-critical work below the fold
- ✓ Using Server Components whenever possible
- ✓ Deferring heavy Client Components
- ✓ Reducing JavaScript executed before the LCP element renders
- ✓ Optimizing font loading
- ✓ Minimizing layout recalculations
- ✓ Removing unnecessary synchronous work

Never preload unnecessary resources. Never mark multiple images as priority. Always identify the real LCP bottleneck first.

---

## CLS (Cumulative Layout Shift)

Prevent layout shifts by:
- ✓ Reserving image dimensions
- ✓ Reserving video dimensions
- ✓ Reserving iframe dimensions
- ✓ Preventing font shifts
- ✓ Maintaining stable layouts
- ✓ Preventing dynamic content jumps
- ✓ Avoiding layout-changing animations
- ✓ Using transform-based animations

Never introduce new layout shifts.

---

## INP (Interaction to Next Paint)

Improve responsiveness by:
- ✓ Reducing unnecessary re-renders
- ✓ Memoizing expensive components
- ✓ Optimizing event handlers
- ✓ Avoiding expensive synchronous work
- ✓ Splitting heavy components
- ✓ Lazy loading non-critical code
- ✓ Keeping interactions responsive

---

## FCP & TBT

Reduce blocking work by:
- ✓ Removing unused JavaScript
- ✓ Splitting bundles
- ✓ Dynamic imports
- ✓ Optimizing hydration
- ✓ Eliminating unnecessary client-side rendering
- ✓ Reducing long tasks
- ✓ Optimizing expensive calculations

---

## Accessibility (WCAG 2.2 AA)

Accessibility is **mandatory**. Always target a Lighthouse Accessibility score of 100.

Audit and improve:
- ✓ Semantic HTML
- ✓ Proper heading hierarchy
- ✓ ARIA roles
- ✓ ARIA labels
- ✓ ARIA descriptions
- ✓ Button accessibility
- ✓ Link accessibility
- ✓ Form labels
- ✓ Error messages
- ✓ Keyboard navigation
- ✓ Focus management
- ✓ Skip links where appropriate
- ✓ Screen reader compatibility
- ✓ Color contrast
- ✓ Image alt text
- ✓ Accessible SVGs
- ✓ Accessible dialogs
- ✓ Accessible modals
- ✓ Accessible dropdowns
- ✓ Accessible accordions
- ✓ Accessible tabs
- ✓ Accessible navigation
- ✓ Accessible tables
- ✓ Accessible tooltips

Never reduce accessibility to improve performance. Never remove focus indicators unless replacing them with an accessible alternative. Never use non-semantic elements where semantic HTML is appropriate.

---

> Per-Change Decision Process (stated in full in `SKILL.md`): identify bottleneck → estimate impact → smallest safe fix → verify parity → continue only if measurable gain.
