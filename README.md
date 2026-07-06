# Claude Code Starter Pack

This repository **authors** the starter kits that help non-technical people ship production code
with Claude Code. The kits are the product; this repo is where they're written and maintained.

## Two kits live in [`boilerplate/`](./boilerplate/)

Both are run by non-technical operators. Copy a kit's contents into the root of a project to set
it up.

```
boilerplate/
├── normal/                                     # static sites → PHP/Apache server
│   ├── CLAUDE.md                               # short, always-on brief
│   └── .claude/
│       ├── settings.local.mac.json             # permissions + macOS notifications
│       ├── settings.local.windows.json         # permissions + Windows notifications
│       └── skills/
│           └── quality-checkpoint/SKILL.md     # milestone & pre-deploy audit
└── advanced/                                   # backend services (Python/FastAPI)
    ├── CLAUDE.md                               # short brief: stack, boundaries, ask DX
    └── .claude/
        ├── settings.local.mac.json             # Python/Docker perms + macOS notifications
        ├── settings.local.windows.json         # Python/Docker perms + Windows notifications
        └── skills/
            ├── backend-scaffold/SKILL.md               # stand up a new FastAPI backend
            └── backend-quality-checkpoint/SKILL.md     # milestone & pre-deploy audit
```

- **Normal** — static sites (Next.js/Astro/Vite/plain HTML) exported to a PHP/Apache server.
- **Advanced** — Python + FastAPI + SQLAlchemy backends with SQLite or cloud MySQL, Docker,
  Ruff, and Pytest. Still operated by a non-technical person, following the documented setup.

Each `CLAUDE.md` stays deliberately short — the detailed guidance lives in the skills, and the
full human-readable setup walkthrough (GitHub onboarding, do's/don'ts, what your team handles)
lives on the website, [`index.html`](./index.html).

### Settings: two source files, one shipped name

Per kit, both `settings.local.{mac,windows}.json` carry the same permissions and differ only in
the notification hooks — `osascript` on macOS, `powershell` system sounds on Windows. Claude Code
loads the file named **`settings.local.json`**, so the download picks the right OS variant and
ships it under that name.

## Getting a kit into a project

1. **Download page** — open [`index.html`](./index.html), switch between the **Normal** and
   **Advanced** tabs, and click *Download for macOS* / *Download for Windows*. It builds a `.zip`
   in the browser (no server) with the right OS's notifications already named
   `settings.local.json`. Unzip, then copy `CLAUDE.md` and `.claude/` into the project root.
2. **Copy the folder** directly — then rename the OS settings file so Claude Code loads it:
   ```sh
   cp -R boilerplate/advanced/. /path/to/your-project/          # or boilerplate/normal/
   cd /path/to/your-project/.claude
   mv settings.local.mac.json settings.local.json               # or settings.local.windows.json
   rm -f settings.local.mac.json settings.local.windows.json    # drop the unused variant
   ```

> `index.html` embeds base64 copies of every shipped file (both kits' `CLAUDE.md`, skills, and
> settings). After editing any source file, refresh its embed — see the `DOWNLOAD THE
> BOILERPLATE` comment in `index.html` for the one-line `base64` command.

## Why `boilerplate/` and not the repo root

The kit files are deliverables, not configuration for *this* repo. Kept at the root, `CLAUDE.md`
and the skills would be loaded as if they governed the kit-authoring work itself. Nesting them
under `boilerplate/` keeps the boundary clear: the repo root is the workshop, and `boilerplate/`
is what ships.
