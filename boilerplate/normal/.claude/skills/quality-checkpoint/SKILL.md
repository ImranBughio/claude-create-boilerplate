---
name: quality-checkpoint
description: Milestone and pre-deployment quality audit for static sites deployed to a PHP/Apache server. Covers performance, dependencies, accessibility/SEO/GEO, security, architecture, and responsiveness. Use when a meaningful milestone is reached and again before deploying.
---

# Quality checkpoint

Two moments, one audit. Ask which this is:

- **Milestone** — catch problems while they're cheap.
- **Pre-deployment** — everything below, plus the deploy gate.

You already know how to write good code — this is direction, not a tutorial. Judge everything
against where it ships: a **static export served by Apache/PHP, no Node runtime**. The stack
varies (Next.js, Astro, Vite, plain HTML); that constraint doesn't.

Go area by area. Fix what's unambiguous. Flag anything needing a design or product call, each
with a **severity** (blocker / should-fix / nice-to-have). End with a short written report.

## Checklist

**Performance** — Images in modern formats, sized for their slot (the export has no runtime
optimizer, so sizing is on you), lazy below the fold. Fonts subset and preloaded, no layout
shift. No oversized dependencies; scan the built bundle for heavy chunks. Measure with
Lighthouse against the **built** site, not dev.

**Dependencies** — Compare installed vs latest; for each major one, confirm it's still
maintained and still the standard pick — check its docs, not memory. Run the audit; resolve or
note vulnerabilities.

**Accessibility (WCAG 2.2 AA)** — Semantic HTML, heading order, alt text, labels, visible
focus, 4.5:1 contrast, `prefers-reduced-motion`. Automated axe catches ~40% — do the keyboard
walk for the rest.

**SEO & GEO** — Per-page title/description, canonical, Open Graph, `sitemap.xml`, `robots.txt`,
valid JSON-LD. For AI engines: real text over text-in-images, self-contained answers, clean
headings, an `llms.txt` where it fits.

**Security** — The bundle is public: no secrets, keys, or tokens in shipped code — they live
behind the external API, which also enforces access (client-side gating is not security).
Validate every input; don't hand-roll it. Response headers (CSP, HSTS, X-Content-Type-Options,
Referrer-Policy) live in Apache `.htaccess` — note what's missing.

**Architecture** — Reused components and tokens over near-duplicates; data access kept out of
UI. One thing, done once.

**Responsive** — Works at 375, 768, and a desktop width: no horizontal scroll, nothing clips,
touch targets ≥ 44px, fluid layout over fixed pixels.

Green lint, types, and tests are table stakes — confirm them, don't belabor them.

## Pre-deployment gate

- The static build succeeds; preview the exported output and click through — no broken route,
  image, or asset.
- Paths resolve from the server's directory; `trailingSlash` matches Apache; the 404 works.
- The previous build is kept, so rollback = re-upload.

## Output

Write `CHECKPOINT.md`: each area marked **pass / fixed / needs-decision** with a short action
list. The operator signs off before work continues or deployment proceeds.
