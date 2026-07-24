---
name: nextjs-lighthouse-skill
description: Optimize Next.js (App Router, React 19) apps for Lighthouse (Performance, Accessibility, Best Practices, SEO) and Core Web Vitals without changing the existing UI, UX, animations, or branding. Use when improving Lighthouse scores, Core Web Vitals, hydration/bundle/load performance, or accessibility while keeping the exact visual design and animation behavior. Enforces a design-preservation contract, a 6-phase baseline → audit → optimize → verify workflow, and a visual-parity gate.
---

# Next.js Lighthouse Skill

Maximize Lighthouse scores through **real engineering improvements** while preserving the existing UI, UX, animations, and functionality **exactly**.

**Read both reference files at the start of every job — they hold the full rules and tactics this workflow depends on:**
- `references/design-preservation-contract.md` — what is locked, what you may change, the visual-parity rule, and when to stop.
- `references/vitals-and-accessibility-playbook.md` — per-vital tactics (LCP / CLS / INP / FCP / TBT) and the WCAG 2.2 AA checklist.

## Golden Rule

**Optimize the implementation — not the design.** Improve the code, rendering strategy, architecture, and runtime performance while preserving the exact visual experience, animation quality, interaction patterns, branding, and overall feel.

> Think like a Principal Frontend Engineer improving the engine of a high-performance sports car without changing its exterior, interior, or driving experience.

## Targets

Performance 100 (or highest legitimately achievable) · Accessibility 100 · Best Practices 100 · SEO 100. Never manipulate or fake scores. Never optimize by removing functionality, animations, accessibility, or design.

## Iron Rules

1. The existing UI/UX/animation is production-approved and **locked**. Change **implementation only** (allowed list: contract).
2. Every change must be **visually identical** — appearance, layout, animation, motion, timing, responsiveness, interaction, branding. The only acceptable visual diffs: fixing accessibility, browser rendering bugs, CLS, broken layouts.
3. Never remove/disable/simplify/shorten animations, delays, stagger, or effects to gain score.
4. If a change requires altering appearance or animation → **STOP**, go to Phase 6. Never assume permission.

## Per-Change Decision Process

Apply to **every** optimization: (1) identify the actual bottleneck — measure, don't guess; (2) estimate its Lighthouse impact; (3) apply the smallest safe fix; (4) verify UI/UX/animation/function remain identical; (5) continue only if measurable gain remains. No speculative changes; prefer measurable improvements over unnecessary refactoring.

## Project Understanding

Before making any optimization:
- Understand the project architecture.
- Follow existing coding conventions.
- Preserve naming patterns, component patterns, and state management.
- Avoid unnecessary rewrites; prefer localized optimizations.
- Never replace a working implementation simply because another approach is preferred.

---

# Workflow — 6 Phases

Run in order. Do not skip Phase 1. No fixes in Phase 1–2.

## Phase 1 — Lock & Baseline
Before any change:
- Understand the project architecture and conventions first (see **Project Understanding**).
- Read the design-preservation contract; treat design + animations as locked.
- Run Lighthouse (mobile **and** desktop); record all 4 category scores + LCP, CLS, INP, FCP, TBT.
- Identify the **real LCP element** — don't guess.
- Screenshot every page and state: hover, active, focus, dark/light mode, open modals/menus/dropdowns, each breakpoint.
- Inventory every animation (GSAP, ScrollTrigger, Framer Motion, CSS keyframes, Motion One, page/route transitions, parallax, stagger, reveal/entrance/exit, hover, loading skeletons) with timing, duration, easing, delay, sequence.
- Save all artifacts as the parity ground-truth for Phase 5. **Zero code change this phase.**

## Phase 2 — Audit
Diagnose only. Map each bottleneck to the vital it hurts — **LCP / CLS / INP / FCP / TBT** (tactics per vital in the playbook). For each finding: identify bottleneck → estimate impact → tag `safe` (implementation-only → Phase 3/4) or `needs-approval` (touches visuals → Phase 6). Rank by impact; drop speculative items.

## Phase 3 — Safe Optimize (implementation only)
Apply `safe` findings via the Per-Change Decision Process. Scope = the contract's **"What You May Change"** list (Server-Component conversion, dynamic import, `next/image` with `priority` on the LCP image only + accurate `sizes`, `next/font`, memoization, bundle/dead-code reduction, reduced hydration, Suspense, caching, metadata/structured-data/SEO, etc.). Run a dedicated **accessibility pass to WCAG 2.2 AA** (full checklist in the playbook): never trade a11y for performance; never remove a focus indicator without an accessible replacement; never use non-semantic elements where semantic HTML fits.

## Phase 4 — Animation Optimize (implementation only, visual identical)
Preserve the animation; optimize **how** it runs, never **what** it does. Allowed vs forbidden optimizations = the contract's **"Animation Preservation"** section (GPU transforms, no layout thrash, ScrollTrigger config, lazy-load libs, rAF/lifecycle, fewer re-renders). Output must look, feel, and time **exactly** the same.

## Phase 5 — Verify Parity
- Re-run Lighthouse (mobile + desktop); record deltas vs Phase 1.
- Re-screenshot every page/state; diff against baseline.
- Re-check every animation (timing, duration, easing, delay, sequence, feel) = identical.
- Confirm the only visual diffs (if any) are allowed fixes: accessibility, browser render bug, CLS, broken layout.
- Report scores before → after per category + metric, and parity confirmation. If parity fails, revert the offending change and re-evaluate.

## Phase 6 — Gate (when unsure)
Any optimization that requires changing visual appearance or animation behavior → **STOP.** (1) Explain why the change is needed; (2) explain the performance benefit; (3) wait for approval. Never assume permission to modify the UI or animations.

## Definition of Done
Targets pursued honestly (no faking); every change is implementation-only OR Phase-6-approved; visual + animation parity verified vs the Phase 1 baseline; accessibility at 100 (WCAG 2.2 AA) with no design regressions.
