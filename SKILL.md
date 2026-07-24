---
name: nextjs-lighthouse-skill
description: Optimize Next.js apps for Lighthouse (Performance, Accessibility, Best Practices, SEO) and Core Web Vitals (LCP, CLS, INP, FCP, TBT) WITHOUT changing the existing UI, UX, animations, or branding. Use when improving Lighthouse scores, Core Web Vitals, load/hydration performance, bundle size, or accessibility of a Next.js (App Router, React 19) app while keeping the exact visual design and animation behavior. Enforces a design-preservation contract, a 6-phase baseline → audit → optimize → verify workflow, and a visual-parity gate.
---

# Next.js Lighthouse Skill

Maximize Lighthouse scores through **real engineering improvements** while preserving the existing UI, UX, animations, and functionality **exactly**.

## Golden Rule

**Optimize the implementation — not the design.**

Improve the code, rendering strategy, architecture, and runtime performance while preserving the exact visual experience, animation quality, interaction patterns, branding, and overall feel of the application.

> Think like a Principal Frontend Engineer improving the engine of a high-performance sports car without changing its exterior, interior, or driving experience.

## Targets

- **Performance: 100** (or the highest legitimately achievable)
- **Accessibility: 100**
- **Best Practices: 100**
- **SEO: 100**

Never manipulate or fake Lighthouse scores. Never optimize by removing functionality, animations, accessibility, or design.

## Iron Rules (read before touching code)

1. The existing UI/UX/animation is **production-approved and locked.** Treat it as read-only design.
2. You may change **implementation only.** See allowed list in `references/design-preservation-contract.md`.
3. Every optimization must produce a **visually identical** result (appearance, layout, animation, motion, timing, responsiveness, interaction, branding).
4. The only acceptable visual differences are those required to fix: accessibility issues, browser rendering bugs, CLS (layout shift), broken layouts.
5. **Never** remove/disable/simplify/shorten animations, delays, stagger, page/scroll/hover/loading effects, or replace animations with static elements — to gain score.
6. When an optimization *requires* changing appearance or animation behavior → **STOP.** Do not change it. Explain why + the perf benefit, then wait for explicit approval (Phase 6).
7. Never assume permission to modify the UI or animations.

Full non-negotiable list: read **`references/design-preservation-contract.md`** at the start of every job.
Full per-vital + accessibility playbook: read **`references/vitals-and-accessibility-playbook.md`**.

## Per-Change Decision Process

Apply to **every** optimization, in order:

1. **Identify** the actual bottleneck (measure, don't guess).
2. **Estimate** its Lighthouse impact.
3. Apply the **smallest safe optimization**.
4. **Verify** UI, UX, animations, and functionality remain identical.
5. **Continue only** if measurable improvements remain.

No speculative optimizations. Prefer measurable engineering improvements over unnecessary refactoring.

---

# Workflow — 6 Phases

Run in order. Do not skip Phase 1. Do not fix in Phase 1–2.

## Phase 1 — Lock & Baseline
Establish ground-truth *before any change*.

- [ ] Read and accept the design-preservation contract (`references/design-preservation-contract.md`). Design + animations = locked.
- [ ] Run Lighthouse (mobile **and** desktop). Record all 4 category scores + each metric: LCP, CLS, INP, FCP, TBT.
- [ ] Identify the **real LCP element** — do not guess.
- [ ] Screenshot every page and every state: hover, active, focus, dark/light mode, open modals/menus/dropdowns, each breakpoint.
- [ ] Inventory every animation: GSAP timelines, ScrollTrigger, Framer Motion, CSS keyframes, Motion One, page/route transitions, parallax, stagger, reveal/entrance/exit, hover, loading skeletons. Record timing, duration, easing, delay, sequence, intensity.
- [ ] Save all baseline artifacts (scores, screenshots, animation inventory). This is the parity ground-truth for Phase 5.
- [ ] **Zero code change in this phase.**

## Phase 2 — Audit
Diagnose only. Still no fixes.

Map each bottleneck to the vital it hurts (details in playbook):

- **LCP** — LCP element priority, image size/format, render-blocking resources, hydration before LCP, font loading, JS executed before the LCP element renders.
- **CLS** — missing image/video/iframe dimensions, font shifts, dynamic content jumps, layout-changing animations.
- **INP** — unnecessary re-renders, heavy event handlers, expensive sync work, oversized components.
- **FCP / TBT** — unused JS, bundle size, hydration cost, long tasks, needless client-side rendering.

For each finding: run decision-process steps 1–2 (identify bottleneck → estimate impact), then **tag it**:

- `safe` — implementation-only, visually identical → goes to Phase 3/4.
- `needs-approval` — would touch visuals/animation → goes to Phase 6.

Rank findings by estimated impact. Drop speculative items.

## Phase 3 — Safe Optimize (implementation only)
Apply `safe` findings. Run the per-change decision process on each.

**Architecture / rendering:**
- Convert Client Components → Server Components where possible.
- Defer or `dynamic()`-import heavy Client Components and heavy libraries.
- `next/image`: `priority` on the **LCP image only** (never multiple), accurate `sizes`, no oversized images, AVIF/WebP where appropriate.
- Preload **only** truly critical assets — never unnecessary ones.
- `next/font`: correct loading, prevent font shift.
- Memoize expensive components/calculations; cut re-renders; optimize event handlers.
- Bundle-split; remove unused JS and dead code; reduce hydration.
- Optimize Suspense boundaries, caching, and API calls.
- Improve metadata, structured data, SEO.

**Accessibility (WCAG 2.2 AA — target 100):** own dedicated pass — see the playbook's full checklist. Never reduce accessibility for performance. Never remove a focus indicator without an accessible replacement. Never use non-semantic elements where semantic HTML fits.

## Phase 4 — Animation Optimize (implementation only, visual identical)
Preserve the animation; optimize *how* it runs, never *what* it does.

**Allowed:**
- GPU-accelerated `transform` + `opacity` (replace layout-triggering properties).
- Eliminate layout thrashing and layout recalculations.
- Optimize animation triggers and ScrollTrigger configuration.
- Lazy-load animation libraries when appropriate.
- Optimize `requestAnimationFrame` usage and animation lifecycle management.
- Prevent unnecessary React re-renders; memoize animated components; reduce needless state updates.

**Forbidden (score is never a reason):** removing, disabling, simplifying, shortening animations; cutting delays or stagger; removing page/scroll/hover/loading animations; replacing an animation with a static element.

Output must look, feel, and time **exactly** the same before and after.

## Phase 5 — Verify Parity
Prove the result is identical.

- [ ] Re-run Lighthouse (mobile + desktop). Record deltas vs Phase 1.
- [ ] Re-screenshot every page and state; diff against baseline.
- [ ] Re-check every animation: timing, duration, easing, delay, sequence, feel — all identical.
- [ ] Confirm the only visual diffs (if any) are allowed fixes: accessibility, browser render bug, CLS, broken layout.
- [ ] Report: score before → after per category + metric, what changed, and parity confirmation.

If parity fails, revert the offending change and re-evaluate.

## Phase 6 — Gate (when unsure)
Any optimization that requires changing visual appearance or animation behavior → **STOP.** Do not make the change.

Instead:
1. Explain **why** the change is needed.
2. Explain the **performance benefit**.
3. **Wait for approval.**

Never assume permission to modify the UI or animations.

---

## Definition of Done

- Lighthouse targets pursued honestly (no faking); scores reported before → after.
- Every change is implementation-only OR explicitly approved via Phase 6.
- Visual + animation parity verified against Phase 1 baseline.
- Accessibility at 100 (WCAG 2.2 AA), with no design regressions.
