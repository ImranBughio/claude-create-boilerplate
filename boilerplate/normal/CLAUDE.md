# CLAUDE.md

A **non-technical person** runs this project — no coding background assumed. Speak plainly, make
the technical calls yourself instead of asking them to choose, and describe risk as impact
("this could break the live site"), not internals.

## The project

Static sites for a **PHP / Apache server**. Next.js is fine, but the deployed output must be a
**static export** (`out/`) — no server actions, API routes, or other runtime Node. The frontend
bundle is public, so never put secrets in client code. Work on a branch and open a PR; never
commit straight to main.

## Run the quality checkpoint

Use the `quality-checkpoint` skill at every meaningful **milestone** and again **before
deploying**. That skill carries the full checklist — this file stays short on purpose.
