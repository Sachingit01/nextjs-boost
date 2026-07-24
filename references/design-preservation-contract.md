# Design Preservation Contract

The existing UI/UX is considered **production-approved**. Your highest priority is to preserve the current visual design, user experience, and animations. Read this file at the start of every job.

---

## DO NOT CHANGE

Without explicit user permission, **NEVER** change:

Layout · UI design · Color palette · Typography · Spacing · Padding · Margin · Border radius · Shadows · Glassmorphism · Gradients · Animations · Motion timing · Animation duration · Animation easing · Animation sequencing · Animation delays · Animation intensity · Hover effects · Active states · Focus styles (unless fixing accessibility) · Component sizing · Component positions · Grid layouts · Flex layouts · Responsive behavior · Breakpoints · Image cropping · Image positioning · Icons · SVGs · Visual hierarchy · Theme · Dark mode · Light mode · Navigation · Scroll behavior · Transitions · GSAP timelines · ScrollTrigger animations · Framer Motion animations · CSS animations · Keyframe animations · Motion One animations · Page transitions · Route transitions · Reveal animations · Entrance animations · Exit animations · Scroll animations · Stagger animations · Parallax effects · Mouse interactions · Drag interactions · Gesture interactions · Ripple effects · Blur effects · Glow effects · Particle effects · Liquid effects · Morphing effects · Micro interactions · Loading skeletons · Existing visual effects.

Also:
- Never redesign a component.
- Never remove decorative effects just to improve Lighthouse.
- Never disable animations for better scores.
- Never reduce animation quality, smoothness, or duration.
- Never remove stagger effects, page transitions, scroll animations, hover animations, or loading animations.
- Never alter branding. Never change the look and feel.
- **Treat the design and animations as locked.**

---

## Animation Preservation

Animations are a core part of the user experience and must be preserved.

**Do NOT optimize Lighthouse by:** removing, disabling, or simplifying animations; reducing animation duration; removing animation delays; removing stagger effects; removing page transitions; removing hover effects; removing scroll-triggered effects; removing GSAP timelines; removing Framer Motion components; replacing animations with static elements.

**Instead, optimize their implementation.** Allowed optimizations:

- ✓ GPU-accelerated transforms (`transform` and `opacity`)
- ✓ Reducing layout recalculations
- ✓ Avoiding layout thrashing
- ✓ Optimizing animation triggers
- ✓ Optimizing ScrollTrigger configuration
- ✓ Lazy loading animation libraries when appropriate
- ✓ Preventing unnecessary React re-renders
- ✓ Memoizing animated components
- ✓ Optimizing `requestAnimationFrame` usage
- ✓ Reducing unnecessary state updates
- ✓ Optimizing expensive calculations
- ✓ Improving animation lifecycle management
- ✓ Improving rendering performance while preserving identical visual output

The animation should look and feel **exactly the same** before and after optimization.

---

## What You MAY Change (implementation only)

- ✓ Reduce unnecessary React re-renders
- ✓ Convert Client Components into Server Components where possible
- ✓ Lazy load heavy sections
- ✓ Dynamic import heavy libraries
- ✓ Optimize images
- ✓ Improve font loading
- ✓ Reduce hydration
- ✓ Memoize expensive calculations
- ✓ Improve accessibility attributes
- ✓ Improve semantic HTML
- ✓ Remove dead code
- ✓ Reduce bundle size
- ✓ Improve folder structure
- ✓ Improve TypeScript
- ✓ Improve maintainability
- ✓ Optimize API calls
- ✓ Optimize Suspense boundaries
- ✓ Optimize caching
- ✓ Improve metadata
- ✓ Improve structured data
- ✓ Optimize rendering strategy
- ✓ Optimize CSS performance without changing appearance
- ✓ Optimize animations internally while keeping their visual appearance identical
- ✓ Remove **provably unreferenced** assets — unused images, videos, SVGs, and other static files nothing renders or imports
- ✓ Remove unused font files/weights/subsets that are shipped but never applied — **but never change the fonts that are in use** (keep the exact typefaces, weights, and styles; only self-host/subset/preload their loading)
- ✓ Remove unused dependencies, unused exports, and unused CSS (verify no visual diff)

---

## Visual Parity Requirement

Every optimization must produce a **visually identical** result. A user comparing before and after should not notice any difference in: Appearance · Layout · Animation · Motion · Timing · Responsiveness · Interaction · Branding.

The **only** acceptable visual differences are those required to fix:
- Accessibility issues
- Browser rendering bugs
- CLS (layout shift)
- Broken layouts

---

## When Unsure

If an optimization requires changing the visual appearance or animation behavior, **DO NOT** make the change. Instead:

1. Explain why.
2. Explain the performance benefit.
3. Wait for approval.

Never assume permission to modify the UI or animations.

---

> Golden Rule (stated in full in `SKILL.md`): optimize the implementation — not the design.
