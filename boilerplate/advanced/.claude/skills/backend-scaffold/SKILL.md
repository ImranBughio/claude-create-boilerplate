---
name: backend-scaffold
description: Stand up a new Python backend service — project structure, Docker Compose, configuration, database (SQLite or cloud MySQL), and a FastAPI baseline. Use when starting a new backend or adding the backend skeleton to an empty project.
---

# Backend scaffold

Set up a new backend the house way. You know Python and FastAPI — this is direction and the
house defaults, not a tutorial. Prefer the simplest thing that meets the requirement; add
infrastructure only when it clearly earns its place.

## Default stack

Python · FastAPI (when an HTTP API is needed) · SQLAlchemy · SQLite (lightweight) or
cloud-hosted MySQL (production) · Docker Compose (backend container only) · Ruff · Pytest ·
Alembic (only when the schema will evolve). Use current, maintained versions; pin or lock them.

Do **not** scaffold Celery, Redis, Kafka, Kubernetes, a local MySQL container, DI frameworks, or
microservice splits. One backend service, cleanly layered.

## Structure

```
project/
  app/
    main.py
    api/routes/        # thin handlers, grouped by resource
    api/dependencies.py
    core/              # config.py, logging.py, security.py
    db/                # base.py, session.py, migrations/
    models/            # SQLAlchemy models
    schemas/           # Pydantic request/response
    services/          # business rules
    repositories/      # data access
  tests/               # unit/  api/  integration/
  data/                # local SQLite file lives here
  docker-compose.yml
  Dockerfile
  pyproject.toml
  .env.example
  .gitignore
  README.md
```

Very small services may flatten this, but keep API, business logic, data access, and config
clearly separated.

## Database — pick one

- **SQLite** — internal tools, prototypes, low concurrency, simple data. Store the file at a
  predictable path and mount it as a Docker volume when persistence matters.
  `DATABASE_URL=sqlite:///./data/app.db`
- **Cloud MySQL** — stronger concurrency, production persistence, frequent multi-writer access.
  `DATABASE_URL=mysql+pymysql://USER:PASSWORD@HOST:3306/DATABASE_NAME`

Support switching between the two through configuration. **Never run MySQL in Docker Compose** —
it is cloud-hosted. Before wiring MySQL, ask the team (DX) for host, port, database, username,
password/secret reference, and SSL/network requirements. Never invent or hardcode credentials.

## Docker Compose

Runs the **backend container only**. It reads `.env` and, for SQLite, mounts `./data`. For MySQL
it only passes environment variables through — there is no database container.

## Configuration & secrets

Centralize in `app/core/config.py` (pydantic-settings). Everything configurable comes from
environment variables: app env, debug, `DATABASE_URL`, CORS origins, secret keys, external URLs.
Ship `.env.example` with safe placeholders. Never commit a real `.env`; add it to `.gitignore`
and `.dockerignore`.

## FastAPI baseline

Pydantic schemas for every request/response; thin routes that delegate to services; repositories
for data access. Explicit response models, meaningful status codes, consistent error shapes
(never leak stack traces), pagination on list endpoints. Add `GET /health` (basic liveness) and
`GET /ready` (confirms DB connectivity).

## Migrations

Use Alembic only when the schema will change. Commit migrations, name them clearly, test them
locally. Tiny throwaway prototypes may skip migrations if that's explicitly acceptable.

## README must cover

Local setup, Docker Compose commands, test commands, lint/format commands (`ruff check .`,
`ruff format .`), database setup + migration commands, required environment variables, the API
docs URL (`/docs`), and the SQLite-vs-cloud-MySQL notes.

## Deliverables

Source + `Dockerfile` + `docker-compose.yml` + `pyproject.toml` + `README.md` + `.env.example`,
SQLAlchemy set up, SQLite working and MySQL-ready, a FastAPI app with a health check, and
unit/endpoint/database tests. When the skeleton stands, run `backend-quality-checkpoint`.
