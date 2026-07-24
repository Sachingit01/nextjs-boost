# nextjs-lighthouse-skill

A Claude Code skill that optimizes Next.js apps for **Lighthouse** (Performance, Accessibility, Best Practices, SEO) and **Core Web Vitals** (LCP, CLS, INP, FCP, TBT) — **without changing** the existing UI, UX, animations, or branding.

> Optimize the implementation — not the design. Improve the engine of the sports car without touching its exterior, interior, or driving experience.

## What it does

- Enforces a **design-preservation contract**: layout, colors, typography, spacing, animations, transitions, and effects are locked. Implementation-only changes.
- Runs a **6-phase workflow**: Lock & Baseline → Audit → Safe Optimize → Animation Optimize → Verify Parity → Gate.
- Pursues Lighthouse **100 / 100 / 100 / 100** honestly — never fakes scores.
- Verifies **visual + animation parity** against a captured baseline before finishing.
- **Gates** any change that would alter visuals/animation — stops and asks for approval.

## Install

With the [skills CLI](https://github.com/vercel-labs/skills) (recommended) — run from your project root:

```bash
npx skills add Sachingit01/nextjs-lighthouse-skill
```

Auto-detects your agent (Claude Code, Cursor, Codex, etc.) and installs non-interactively. Preview what's inside first:

```bash
npx skills add Sachingit01/nextjs-lighthouse-skill --list
```

Or clone manually into your Claude Code skills directory:

```bash
git clone https://github.com/Sachingit01/nextjs-lighthouse-skill.git ~/.claude/skills/nextjs-lighthouse-skill
```

## Use

In any Next.js project, run:

```
/nextjs-lighthouse-skill
```

…or just ask Claude Code to optimize the app's Lighthouse scores / Core Web Vitals. The skill auto-triggers from its description.

## Structure

```
nextjs-lighthouse-skill/
├── SKILL.md                                   # objective, golden rule, iron rules, per-change loop, 6 phases
└── references/
    ├── design-preservation-contract.md        # full DO NOT CHANGE list, allowed changes, parity, when-unsure
    └── vitals-and-accessibility-playbook.md    # LCP / CLS / INP / FCP+TBT playbooks, WCAG 2.2 AA, decision process
```
