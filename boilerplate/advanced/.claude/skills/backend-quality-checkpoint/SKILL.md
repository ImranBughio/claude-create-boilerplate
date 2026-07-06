---
name: backend-quality-checkpoint
description: Milestone and pre-deployment quality audit for Python/FastAPI backend services (SQLAlchemy, SQLite or cloud MySQL, Docker). Covers performance, dependencies, API design, security, architecture, code quality, testing, and deploy readiness. Use when a milestone is reached and again before deploying.
---

# Backend quality checkpoint

Two moments, one audit. Ask which this is:

- **Milestone** — catch problems while they're cheap.
- **Pre-deployment** — everything below, plus the deploy gate.

You already know how to write good code — this is direction, not a tutorial. Judge everything
against the house defaults: **Python · FastAPI · SQLAlchemy · SQLite or cloud MySQL · Docker
Compose (backend only) · Ruff · Pytest**, kept as simple as the requirement allows.

Go area by area. Fix what's unambiguous. Flag anything needing a decision, each with a
**severity** (blocker / should-fix / nice-to-have). End with a short written report.

## Checklist

**Performance** — Lightweight by default, no premature optimization. `async def` only with
async-compatible I/O — never block the event loop. Paginate list endpoints; bound payload sizes.
Index frequently filtered/joined columns; avoid N+1; load only the columns you need. No caches,
queues, or workers unless a measured need justifies them.

**Dependencies** — Compare installed vs latest; for each, confirm it's maintained, stable, and
actually needed — check its docs, not memory. Pin or lock versions; separate dev from runtime;
drop unused ones. No deprecated, abandoned, or unsupported packages.

**API design** — Pydantic schemas on every request/response; explicit response models; meaningful
status codes; consistent, machine-readable error shapes; never leak stack traces. Accurate
OpenAPI (tags, summaries, examples); clear field names over `data`/`value`/`info`.

**Security** — No hardcoded or committed secrets (`.env.example` only). Validate all input via
Pydantic; least-privilege DB credentials; explicit CORS (no wildcard without reason); debug off
in production-like environments. Parameterized queries / query builders only — never concatenate
user input into SQL. For auth, use proven libraries and modern password hashing — no custom
crypto. Confirm SSL for cloud MySQL. Never log secrets or tokens.

**Architecture** — API, business logic, and persistence stay separated. Explicit modules over
generic abstractions; no repositories/services/DI introduced unless they add clarity. Small,
single-purpose functions; no duplicated validation or business logic; domain logic independent of
FastAPI where practical.

**Code quality** — Type hints throughout; clear names; short functions; no hidden side effects or
global mutable state. Ruff clean (`ruff check .`, `ruff format .`). Deliberate error handling
mapped to consistent HTTP responses. Useful logs — never sensitive data.

**Testing** — Unit (business logic), API (endpoints), and database (persistence) tests, plus
validation and error-path cases. Tests run on SQLite without needing DX/cloud credentials;
isolate and reset state between tests. Practical coverage of core logic, endpoints, DB behavior,
and common failures via `pytest --cov=app` — not percentage-chasing.

**Logging & config** — Startup and key operational events logged with useful fields (timestamp,
level, module, message, request ID). Configuration centralized and environment-driven.

## Pre-deployment gate

- App builds and starts in Docker Compose; `/health` and `/ready` pass; `/ready` confirms DB
  connectivity.
- Migrations apply cleanly from scratch (`alembic upgrade head`) when migrations are used.
- Real secrets are absent from the repo and image; `.dockerignore` excludes `.env`; the container
  doesn't run as root without reason.
- Cloud MySQL credentials, SSL, and network/IP requirements confirmed with the team (DX) for the
  target environment.

## Output

Write `CHECKPOINT.md`: each area marked **pass / fixed / needs-decision** with a short action
list. The operator signs off before work continues or deployment proceeds.
