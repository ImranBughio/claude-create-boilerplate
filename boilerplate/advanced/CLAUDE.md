# CLAUDE.md

A **non-technical operator** runs this project — capable, but not a developer. Speak plainly,
make the engineering calls yourself instead of asking them to choose, and describe risk as
impact ("this could expose the database"), not internals.

## The stack

Backend services. Default to **Python + FastAPI + SQLAlchemy**, with **SQLite** for lightweight
persistence and **cloud-hosted MySQL** for production-grade needs, **Docker Compose** (backend
container only — never a local MySQL container), **Ruff**, **Pytest**, and **Alembic** only
when the schema will evolve. Use current, maintained libraries; never a deprecated one.

Keep it simple. Do **not** add Celery, Redis, Kafka, Kubernetes, dependency-injection
frameworks, or microservice patterns unless a requirement clearly justifies it.

## Secrets & credentials

Never hardcode or commit secrets. Keep real values in a local `.env` (git-ignored); commit only
`.env.example` with safe placeholders. For cloud MySQL and other infrastructure, **ask the team
(DX) for credentials and connection details — never invent them.**

## Skills

- `backend-scaffold` — stand up a new backend: structure, Docker, config, database, FastAPI baseline.
- `backend-quality-checkpoint` — run at every meaningful **milestone** and again **before
  deploying**. It carries the full audit; this file stays short on purpose.

## Workflow

Work on a branch and open a PR; never commit straight to main.
