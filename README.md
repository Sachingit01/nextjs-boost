# nextjs-boost

A **Claude Code skill** that optimizes Next.js applications for **Lighthouse** (Performance, Accessibility, Best Practices, SEO) and **Core Web Vitals** (LCP, CLS, INP, FCP, TBT) — through real engineering improvements, **without changing** the existing UI, UX, animations, or branding.

> **Golden rule:** optimize the *implementation*, not the *design*. Improve the engine of a high-performance sports car without touching its exterior, interior, or driving experience.

---

## The problem this solves

Most "performance optimization" quietly degrades the product. An assistant told to "improve the Lighthouse score" will often strip animations, delete transitions, simplify effects, or flatten a carefully designed UI — because that's the fastest way to move the number. You get a green score and a worse product.

`nextjs-boost` refuses that trade. It treats the shipped design and its animations as **production-approved and locked**, and only changes the code *behind* them. The rule is strict: a user comparing the app before and after must not notice **any** visual, motion, or interaction difference — the score goes up, everything else stays pixel-for-pixel identical.

---

## What it optimizes

The skill honestly pursues:

| Category | Target |
|---|---|
| Performance | 100 (or the highest legitimately achievable) |
| Accessibility | 100 |
| Best Practices | 100 |
| SEO | 100 |

…and the underlying **Core Web Vitals**, each with its own tactics:

- **LCP** (Largest Contentful Paint) — find the real LCP element, prioritize it correctly (`next/image` with `priority` on that one image only + accurate `sizes`), cut render-blocking work and JS executed before it paints.
- **CLS** (Cumulative Layout Shift) — reserve image/video/iframe dimensions, prevent font shift, use transform-based animations instead of layout-changing ones.
- **INP** (Interaction to Next Paint) — reduce re-renders, memoize expensive components, optimize event handlers, split heavy work.
- **FCP / TBT** — remove unused JS, split bundles, dynamic-import, reduce hydration and long tasks.

**Scores are never faked or manipulated** — only earned through engineering.

---

## What it will never change

Without your explicit permission, the skill treats these as locked: layout, color palette, typography, spacing, shadows, gradients, glassmorphism, border radius, component sizing/position, grid/flex layouts, responsive behavior, breakpoints, theme/dark mode, navigation, icons/SVGs — and **all** animation: GSAP timelines, ScrollTrigger, Framer Motion, CSS keyframes, Motion One, page/route transitions, parallax, stagger, reveal/entrance/exit, hover effects, loading skeletons, and their exact timing, duration, easing, delay, and sequence.

It will **never** remove, disable, simplify, or shorten an animation to gain a score. Instead it optimizes *how* animations run (GPU transforms, no layout thrash, better ScrollTrigger config, lazy-loaded libraries, fewer re-renders) so they look and feel **exactly** the same.

The only visual changes it is ever allowed to make are those required to fix: **accessibility issues, browser rendering bugs, CLS (layout shift), or a broken layout.**

---

## How it works — the 6-phase workflow

The skill runs a disciplined, in-order workflow. No fixes happen until the baseline is captured and the bottlenecks are understood.

1. **Lock & Baseline** — Understand the project first. Run Lighthouse (mobile + desktop), record every score and metric, identify the *real* LCP element, screenshot every page and state (hover/focus/active, dark/light, modals, breakpoints), and inventory every animation. This becomes the ground-truth for verifying parity later. *No code changes.*
2. **Audit** — Diagnose bottlenecks and map each to the vital it hurts. Tag every finding as `safe` (implementation-only) or `needs-approval` (would touch visuals). *Still no fixes.*
3. **Safe Optimize** — Apply the `safe` findings: Server-Component conversion, dynamic imports, `next/image` / `next/font`, memoization, bundle/dead-code reduction, reduced hydration, Suspense, caching, metadata/SEO — plus a dedicated **accessibility pass** to WCAG 2.2 AA.
4. **Animation Optimize** — Improve animation *implementation* only, keeping visual output identical.
5. **Verify Parity** — Re-run Lighthouse, re-screenshot, re-check every animation against the baseline, and report the before → after score deltas with a parity confirmation. If anything diverges, revert.
6. **Gate** — If an optimization *requires* a visual or animation change, the skill **stops**, explains why and the performance benefit, and waits for your approval. It never assumes permission.

Every single change also runs through a **per-change decision loop**: identify the actual bottleneck → estimate its impact → apply the smallest safe fix → verify parity → continue only if a measurable gain remains. No speculative refactoring.

---

## Guardrails it follows before optimizing

- Understand the project architecture and follow existing coding conventions.
- Preserve naming, component patterns, and state management.
- Avoid unnecessary rewrites; prefer localized optimizations.
- Never replace a working implementation just because another approach is preferred.

---

## How the skill is structured

The skill uses **progressive disclosure** so it costs almost nothing until it's actually working: only its `name` + `description` sit in context by default; the workflow (`SKILL.md`) loads when the skill triggers, and the detailed rule sheets load on demand.

```
nextjs-boost/
├── SKILL.md                                   # objective, golden rule, iron rules, per-change loop, 6-phase workflow
└── references/
    ├── design-preservation-contract.md        # full "DO NOT CHANGE" list, allowed changes, visual-parity rule, when to stop
    └── vitals-and-accessibility-playbook.md    # per-vital tactics (LCP/CLS/INP/FCP/TBT) + the WCAG 2.2 AA checklist
```

---

## Install

With the [skills CLI](https://github.com/vercel-labs/skills) (recommended) — run from your project root:

```bash
npx skills add Sachingit01/nextjs-boost
```

It auto-detects your agent (Claude Code, Cursor, Codex, etc.) and installs non-interactively. Preview what's inside first:

```bash
npx skills add Sachingit01/nextjs-boost --list
```

Or clone manually into your Claude Code skills directory:

```bash
git clone https://github.com/Sachingit01/nextjs-boost.git ~/.claude/skills/nextjs-boost
```

## Use

In any Next.js project, run:

```
/nextjs-boost
```

…or simply ask Claude Code to *"improve this app's Lighthouse scores / Core Web Vitals without changing the design"* — the skill auto-triggers from its description.

**What a run looks like:** the assistant first captures a baseline (scores + screenshots + animation inventory), audits and ranks the bottlenecks, applies implementation-only fixes, verifies the result is visually identical, and reports the before → after scores. Anything that would alter the look or motion is paused for your approval rather than done silently.

---

## Requirements & compatibility

- **Next.js** App Router (also works with Pages Router); tactics target modern Next.js + **React 19**.
- Works in any agent supported by the skills CLI; designed primarily for **Claude Code**.
- No dependencies to install — the skill is instructions + reference docs, not code that runs.

---

## Sharing

The skill is a plain folder. Share it by pointing people at the install command above, or have them clone the repo into their own `~/.claude/skills/`.

## License

[MIT](./LICENSE)
