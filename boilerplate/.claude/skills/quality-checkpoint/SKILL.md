---
name: quality-checkpoint
description: Runs the milestone and pre-deployment quality checklist for designer-built static sites deployed to a PHP server. Covers performance, dependency currency, accessibility/SEO/GEO, security, reusable architecture, code quality, and mobile/tablet responsiveness. Use when a development milestone is reached and again before deploying.
---

# Quality checkpoint

A guided audit a designer can trigger at two moments. Run it, report findings by severity,
fix the clear-cut issues, and hand the judgment calls back to the designer.

Deploy target context: **static export to a PHP / Apache server, no Node runtime.** Judge
everything against that.

## When to run

Ask which checkpoint this is:

- **Milestone** — a meaningful chunk of the site is done. Catch problems while they're cheap.
- **Pre-deployment** — before any upload to the server. Everything below, plus the
  deploy-readiness gate.

## How to run

Go area by area. For each finding, state: **severity** (blocker / should-fix / nice-to-have),
where it is, why it matters, and the fix. Apply unambiguous fixes; list anything that needs a
design or product decision. Finish with a written report.

## The checklist

### 1. Performance
- Images: modern formats (WebP/AVIF), correctly sized, lazy-loaded below the fold. (Static
  export uses `images.unoptimized`, so correct sizing is on us, not an optimizer.)
- Fonts: subset, preloaded, few weights; no layout shift.
- Bundle: no oversized dependencies; code-split heavy routes; inspect the production build
  output for large chunks.
- Run Lighthouse (or the Chrome DevTools MCP) against the **built `out/` site**, not dev.

### 2. Dependency currency
- List dependencies; compare installed vs latest (`npm outdated`).
- For each major one: is it still maintained and still the industry-standard pick? Verify
  against its docs / recent releases — not memory. Flag deprecated or abandoned packages and
  propose the current standard.
- `npm audit` for known vulnerabilities; resolve or note them.

### 3. Accessibility, SEO & GEO
- **A11y (WCAG 2.2 AA):** semantic HTML, heading order, alt text, labels, focus order,
  4.5:1 contrast, a full keyboard walk, `prefers-reduced-motion`. (axe via the DevTools MCP
  catches ~40%; the keyboard pass catches the rest.)
- **SEO:** per-page title/description, canonical, Open Graph / Twitter cards, `sitemap.xml`,
  `robots.txt`, valid JSON-LD structured data.
- **GEO (generative-engine optimization):** clean semantic structure and headings,
  descriptive real text (not text-in-images), self-contained answers, and an `llms.txt` where
  appropriate — so AI engines can parse and cite the site.

### 4. Security (static-site lens)
- The whole bundle is public: confirm **no secrets, keys, or tokens** in the shipped JS — any
  live on the external API only.
- External API calls don't expose privileged keys; sensitive logic is behind the API, not
  client-side gating.
- Input handling goes through the validated forms (RHF + Zod); nothing hand-rolled.
- Security headers: with no Node, these are set on Apache (`.htaccess`) — CSP, HSTS,
  X-Content-Type-Options, Referrer-Policy. Note what's missing.

### 5. Reusable architecture
- Components are reused, not copy-pasted; shared logic is factored out.
- Design tokens reused (no stray hex / magic numbers); one component with variants beats four
  near-duplicates.
- Clear separation: UI vs data access (`lib/api/`) vs content.

### 6. Code quality
- Lint, typecheck, and tests all green. No dead code, stray `console.log`, or commented-out blocks.
- Descriptive names; no `any` without cause; consistent with the rest of the codebase.

### 7. Responsive (mobile & tablet)
- Test at **375px** (mobile), **768px** (tablet), and a desktop width. No horizontal scroll;
  nothing overlaps or clips.
- Touch targets ≥ 44px; tap states work; nav/menus usable on touch.
- Fluid layout (Flexbox / Grid / `clamp()`), not hardcoded pixel widths.

## Pre-deployment gate (additional)
- `npm run build` succeeds with static export; preview `out/` via `npx serve out` and click
  through — no broken routes, images, or assets.
- Asset/link paths resolve for the server's directory (`basePath` / `assetPrefix`);
  `trailingSlash` behavior matches Apache; a working 404 page.
- Confirm the previous build is retained so rollback = re-upload.

## Output
Write `CHECKPOINT.md` (or paste a report): each area marked **pass / fix-applied /
needs-decision**, with a short action list. The designer signs off before the milestone
continues or deployment proceeds.
