# Start here — run this first

You don't need to be a developer. Before anything else, open this project folder in **Claude
Code** and paste the prompt below. It reviews your project, sets it up the house way, decides how
password protection should work, and gets it onto the company GitHub.

> **Access mode for this project: {{MODE}}**

## The setup prompt

Copy everything between the lines and paste it into Claude Code:

---

Set up this project for me. I'm not a developer — explain each step in plain language before you
do it, and make the technical decisions yourself.

1. **Review the folder first.** Flag anything that should never be uploaded — passwords, API keys,
   `.env` files, private client files, temporary or personal files.

2. **Build the backend the house way** (follow `CLAUDE.md` and run the `backend-scaffold` skill):
   Python + FastAPI + SQLAlchemy, SQLite for lightweight data or cloud MySQL for production,
   Docker Compose for the backend only, Ruff, Pytest. Keep it simple — no Celery, Redis, Kafka,
   Kubernetes, or a local MySQL container unless a requirement clearly needs it. For cloud MySQL
   credentials, tell me to ask DX — never invent them.

3. **Access protection: this project is {{MODE}}.**
   - If PRIVATE: add authentication so only logged-in users can reach protected data. Use a
     proven library and modern password hashing (never custom crypto), keep secrets in `.env`
     only, and enforce access on the API — not in the browser. Tell me exactly what credentials
     or infrastructure you need from DX.
   - If PUBLIC: no login is required, but still never expose secrets and validate every input.

4. **Prepare a clear `README.md` and a `.gitignore`.** Never commit real secrets — commit only
   `.env.example` with safe placeholder values.

5. **Put it on the company GitHub:** initialize Git, make the first commit, connect it to the
   correct company repository, and push to the `main` branch. Ask me for the repository link when
   you need it.

Go step by step, and pause for my confirmation before anything that changes files or uploads.

---

## Before you run it — the GitHub checklist

You need an empty company GitHub repository ready. If you don't have one yet:

1. Register on GitHub with your **company email**, then ask Imran or Oksana to add you to the
   company organization.
2. Sign in with that company email.
3. Create a **completely empty** repository in the organization — no README, `.gitignore`,
   license, or starter files.
4. Copy its link (e.g. `https://github.com/company-name/project-name.git`) — you'll paste it when
   Claude asks in step 5.

## Never upload

Passwords · API keys · `.env` files · private client files · temporary or personal files · any
confidential company files that aren't part of the project.

## What's in this kit

- `CLAUDE.md` — the always-on brief the agent follows (default stack, boundaries, ask-DX rule).
- `.claude/skills/backend-scaffold` — stands up the FastAPI backend structure.
- `.claude/skills/backend-quality-checkpoint` — the milestone & pre-deploy audit.
- `.claude/settings.local.json` — command permissions, desktop notifications, `.env` read-block.
