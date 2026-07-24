# Core Web Vitals, Lighthouse & Accessibility Playbook

Objective: maximize Lighthouse scores through real engineering improvements while preserving the existing UI, UX, animations, and functionality.

**Targets:** Performance = 100 · Accessibility = 100 · Best Practices = 100 · SEO = 100. **Core Web Vitals:** LCP ≤ 2.5s · CLS ≤ 0.1 · INP ≤ 200ms · TBT ≤ 200ms.

- Never manipulate or fake Lighthouse scores.
- Never optimize by removing functionality, animations, accessibility, or design.
- **Two-pass rule:** if any target is unmet on the first pass, repeat the audit → optimize → verify cycle once more (max 2 passes total), then report any remaining blocker.

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

**Advanced LCP tactics (use these to close a stubborn LCP):**
- ✓ Never lazy-load the LCP image — no `loading="lazy"` on it; set `priority` (which adds `fetchpriority="high"` + preload) on that one image only.
- ✓ Serve the LCP image from HTML on first paint — render it in a Server Component, above the fold, not gated behind a Client Component, `useEffect`, state, or an entrance animation that mounts it late.
- ✓ Preload the LCP resource early: `next/image priority`, or `<link rel="preload">` / the Metadata API for a critical hero/background; add `<link rel="preconnect">` only to the origin that actually serves it.
- ✓ Give the LCP image tight, accurate responsive `sizes` so the browser fetches the smallest correct candidate — oversized downloads are the #1 LCP killer.
- ✓ If the LCP is text, preload + `font-display: swap` the heading font and inline critical CSS so text paints without waiting on JS/CSS.
- ✓ Cut the request chain before LCP: fewer redirects, no render-blocking third-party scripts above the fold (load them via `next/script` with `strategy="afterInteractive"` or `"lazyOnload"`).
- ✓ Prefer static/streamed SSR for the above-the-fold region so LCP markup arrives in the first response, not after hydration or a client fetch.

Never preload unnecessary resources. Never mark multiple images as priority. Always identify the real LCP bottleneck first (Lighthouse "Largest Contentful Paint element" + the trace).

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

## Unused Asset & Dead-Weight Elimination

Shipping bytes nothing renders or imports costs LCP, TBT, and total transfer size for zero benefit. Find them with tooling first, then remove **only what is provably unreferenced** — never guess.

**Find it (don't guess):**
- Bundle: `@next/bundle-analyzer` (or `next build` output) to see what dominates the JS.
- Unused code: Chrome DevTools **Coverage** tab + Lighthouse "Reduce unused JavaScript/CSS" and "treemap".
- Unused files: grep the codebase for each asset in `public/` and `src/`; anything with zero references is a removal candidate.
- Unused deps: `depcheck` / `knip` for dependencies and exports imported nowhere.

**Remove / fix (all implementation-only, no visual change):**
- ✓ Delete unreferenced images, videos, SVGs, and other static assets in `public/` and the source tree.
- ✓ Delete unused **font files/weights/subsets** that are shipped but never applied. **Do NOT change the typefaces that are in use** — keep the exact fonts, weights, and styles the design uses; only drop weights/subsets nothing references and optimize *loading* (see below).
- ✓ Remove dead code, unused exports, unused components, and unused npm dependencies.
- ✓ Eliminate unused CSS (ensure Tailwind `content` globs are correct; drop dead stylesheets/selectors) — verify no visual diff after.
- ✓ Fix heavy imports: replace barrel imports with deep imports, and use `experimental.optimizePackageImports` for large icon/UI libraries so only used code ships.
- ✓ Compress and right-size the assets that remain (AVIF/WebP for images; poster + lazy/`preload="none"` for below-the-fold video; never re-encode in a way that changes appearance).

**In-use font loading (keep the look, cut the cost):**
- ✓ Use `next/font` to self-host, subset, and preload the in-use fonts, removing render-blocking external requests and font-shift.
- ✓ Ship only the weights/styles actually used; keep `font-display` behavior consistent with current rendering so text does not reflow or restyle.

---

## Closing the Last 10 Points (stuck near ~90)

When Performance plateaus around 90, the remaining points are almost always in this list. Work them in order of Lighthouse's own "Opportunities/Diagnostics" impact estimates:

1. **Reduce unused JavaScript/CSS** — code-split, dynamic-import below-the-fold and interaction-only code, purge unused CSS, deep-import large libs (`optimizePackageImports`).
2. **Eliminate render-blocking resources** — inline critical CSS, defer non-critical CSS/JS, move third-party scripts to `next/script` `afterInteractive`/`lazyOnload`.
3. **Fix the LCP request chain** — preload + `priority` the true LCP image, right-size `sizes`, no lazy-load on it, no client-gated mount (see Advanced LCP tactics).
4. **Cut main-thread / long tasks (TBT)** — less hydration via Server Components, memoization, smaller client bundles, avoid large synchronous work on load.
5. **Serve modern, right-sized images** — AVIF/WebP, correct dimensions, `next/image`; remove oversized originals.
6. **Optimize the in-use fonts** — `next/font` self-host + subset + preload (without changing the typeface).
7. **Enable compression & caching** — Brotli/gzip, long-lived immutable caching for static assets, HTTP/2+.
8. **Trim third parties** — audit analytics/tag-manager/embeds; defer, lazy-load, or facade them (they routinely cost the last 5–10 points).
9. **Reduce DOM size & layout work** where a component renders far more nodes than needed (no visual change).
10. **Re-measure after each** — keep only changes with a measurable gain; verify parity every time.

Do all of the above **without** touching the design or animations — everything here is implementation-only.

---

## Systematic Investigation Checklist

The investigation surface for the **Lighthouse Completion Policy**. For each item below, find the **root cause** of any deduction, estimate its impact, apply the smallest implementation-only fix, and verify parity. Do not stop after generic optimizations — work every item until no safe improvement remains.

**Performance**
- Largest Contentful Paint (LCP) · First Contentful Paint (FCP) · Interaction to Next Paint (INP) · Cumulative Layout Shift (CLS) · Time to First Byte (TTFB)
- Render-blocking resources · critical rendering path · network request chains
- JavaScript execution time · long main-thread tasks
- Unused JavaScript and CSS · bundle size · code splitting · dynamic imports
- Hydration overhead · unnecessary client-side rendering · Server Component opportunities
- Image optimization · font loading · CSS efficiency
- Caching opportunities · third-party scripts

**Best Practices**
- Console errors and warnings · deprecated browser APIs · browser compatibility issues
- Responsive image delivery · image sizing and aspect ratios
- Security-related issues · source maps · mixed content
- Third-party integrations · any implementation that negatively impacts browser best practices

**Termination.** Continue iterating until every remaining deduction has been either:
- **fixed**, or
- **documented** with a valid technical reason why it cannot be improved without changing the existing design, functionality, animations, or project requirements.

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

## Best Practices (target 100)

Lighthouse deducts here for small, easily-fixed correctness issues. Check every one:
- ✓ **Zero browser console errors** on load and interaction (fix the underlying error — never silence it).
- ✓ **No deprecated APIs** and no browser warnings.
- ✓ Served over **HTTPS**; no mixed content (all requests https).
- ✓ **Images have correct aspect ratio** and are displayed at natural resolution (no distorted/upscaled bitmaps).
- ✓ **No known-vulnerable libraries** (patch/upgrade flagged deps).
- ✓ Valid **source maps** for shipped JS in production (or intentionally omitted).
- ✓ A **Content-Security-Policy** effective against XSS, plus `X-Content-Type-Options: nosniff` and sensible security headers.
- ✓ Correct `<!DOCTYPE html>`, `<meta charset>`, and a viewport meta with no `user-scalable=no`/`maximum-scale` that blocks zoom.
- ✓ Don't request **geolocation/notification/camera** permissions on page load.
- ✓ External links use `rel="noopener"`/`noreferrer` where appropriate.
- ✓ No use of `document.write`, no unload listeners, no third-party cookies you don't need.
- ✓ Pass paste into password fields; no deprecated `<blink>`/quirks-mode issues.

## SEO (target 100)

Use the Next.js **Metadata API** so these are correct on every route:
- ✓ Unique, descriptive `<title>` and **`meta description`** per page.
- ✓ `<html lang>` set correctly.
- ✓ Viewport meta present (Next.js default) so content is mobile-friendly.
- ✓ Page is **indexable** — no accidental `noindex`/`robots: none`, not blocked by `robots.txt`.
- ✓ **Canonical URL** (`alternates.canonical`) to avoid duplicate-content dilution.
- ✓ **`robots.txt`** and **`sitemap.xml`** present (Next.js `robots.ts` / `sitemap.ts`).
- ✓ **Legible font sizes** and adequate **tap-target sizes/spacing** (Lighthouse SEO + mobile checks).
- ✓ Links are **crawlable** (`<a href>`, not JS-only click handlers) with **descriptive link text** (no "click here").
- ✓ Images have `alt`; `hreflang` set if the app is internationalized.
- ✓ **Structured data** (JSON-LD) valid where relevant (Article, Product, Breadcrumb, Organization).
- ✓ Open Graph / Twitter Card metadata for correct sharing (not scored, but expected on production pages).

## Minute-Details Sweep (the 1–3 point leaks)

After the big wins, do a final pass for the small things that quietly cap a category below 100. None of these change the design:
- Console: **no errors or warnings** in production build.
- Every `<img>`/`next/image`: has `alt`, has width+height (or `fill` + sized parent), correct format, not oversized, not lazy if it's the LCP.
- Exactly **one** `priority` image (the LCP); everything else below the fold `loading="lazy"`.
- `next/font` for all in-use fonts (no render-blocking `<link>` to font CDNs); `preconnect` only to origins actually used.
- One `<h1>` per page; headings in order (no skipped levels).
- All form controls have associated `<label>`s; buttons/links have accessible names; icon-only buttons have `aria-label`.
- Sufficient **color contrast** (≥ 4.5:1 text) — if a real fail, treat as an allowed accessibility fix.
- `meta description`, `title`, `lang`, canonical present on **every** route.
- No `maximum-scale`/`user-scalable=no` blocking zoom.
- Third-party scripts loaded via `next/script` with the right `strategy`; analytics deferred.
- Caching headers on static assets; compression enabled; no 404/redirected requests in the critical path.
- `robots.txt` + `sitemap.xml` present and correct; no stray `noindex`.
- Remove leftover `console.log`, debug code, and dev-only imports from production.

Re-run Lighthouse after the sweep and confirm each category; keep only changes with a measurable gain and verify visual parity.

---

> Per-Change Decision Process (stated in full in `SKILL.md`): identify bottleneck → estimate impact → smallest safe fix → verify parity → continue only if measurable gain.
