# CLAUDE.md

Always-on project context for designer-led delivery.
Use this file for stable project constraints, required checkpoints, and long-lived decisions
that agents should follow across sessions.

## Core context

- Primary users are designers with little/no dev background.
- Deployment target is PHP/Apache hosting using static files.
- Next.js is allowed, but the deployed output must be static (`out/`).

## Who you're working with

A **non-programmer designer** is operating this project. Assume no coding knowledge.

- Explain in plain language; avoid jargon, or define it briefly the first time you use it.
- Make the technical decisions yourself rather than asking the designer to choose between
  options they can't evaluate — recommend, then proceed. Only ask when it's a product,
  content, or visual-design call that's genuinely theirs.
- Never assume a command, tool, or config step is "obvious." Spell out what to run and what
  success looks like.
- Surface risk in terms of impact ("this could break the live site"), not internals.

## Non-negotiables

- Re-validate framework, UI library, and key packages at project start. Prefer maintained,
  current, industry-standard choices.
- Configure Next.js for static export compatibility. Do not rely on runtime Node features in
  production (server actions, API routes, middleware-dependent logic).
- Treat the frontend bundle as public. Never ship secrets in client code.
- Before deployment, ensure build output is upload-ready for the PHP host.

## Required quality checkpoints

Run the `quality-checkpoint` skill at:

1. **Milestone checkpoint** (during development when a meaningful milestone is reached)
2. **Pre-deployment checkpoint** (before shipping/upload)

Checkpoint commit practice:
- Before a checkpoint starts making code changes, ask for (or recommend) a quick checkpoint
  commit so fixes are easy to review and roll back if needed.
- If approved, create that commit before applying checklist-driven edits.

The checkpoint covers:
- Performance
- Dependency currency / package relevance
- Accessibility, SEO, GEO
- Security
- Reusable architecture
- Code quality
- Responsive behavior (mobile + tablet)

## Team workflow defaults

- Work via branch + PR flow (avoid direct commits to protected main branch).

## How this file should evolve

Keep this file limited to stable constraints, required checkpoints, and long-lived decisions.
Put detailed workflows/checklists in skills, and only add new rules here when they repeat across tasks.
