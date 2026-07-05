# Designers as Devs

This repository **authors** a starter kit that helps designers ship production code with
Claude Code. The kit is the product; this repo is where it's written and maintained.

## The deliverable lives in [`boilerplate/`](./boilerplate/)

Everything under `boilerplate/` is the output asset — copy its contents into the root of a
designer's project to set them up:

```
boilerplate/
├── CLAUDE.md                                   # the always-on agent contract
└── .claude/
    ├── settings.local.json                     # permissions + macOS desktop notifications
    ├── settings.local.windows.json             # permissions + Windows desktop notifications
    └── skills/
        └── quality-checkpoint/SKILL.md         # milestone & pre-deploy quality checklist
```

`settings.local.json` ships with **macOS** notification hooks (via `osascript`) as the default.
`settings.local.windows.json` is the Windows variant (via `powershell` system sounds) — on
Windows, rename it to `settings.local.json`. The download button (below) does this swap for you.

The human playbook, [`index.html`](./index.html), stays at the repo root so it can be opened
directly in a browser.

### Two ways to get it into a project

1. **Download button** — open `index.html` and click *Download for macOS* or *Download for
   Windows*. It builds a `.zip` of the boilerplate in the browser (no server needed) with the
   right OS's notifications already named `settings.local.json`. Unzip, then copy `CLAUDE.md`
   and `.claude/` into the project root.
2. **Copy the folder** directly:
   ```sh
   cp -R boilerplate/. /path/to/your-project/   # CLAUDE.md, settings, and the skill
   cp index.html /path/to/your-project/         # the human playbook (optional)
   ```

> The download embeds base64 copies of `CLAUDE.md` and the skill inside `index.html`. After
> editing either file, refresh the embed — see the `DOWNLOAD THE BOILERPLATE` comment in
> `index.html` for the one-line `base64` commands.

## Why `boilerplate/` and not the repo root

The kit files are deliverables, not configuration for *this* repo. Kept at the root,
`CLAUDE.md` and the skill would be loaded as if they governed the kit-authoring work itself.
Nesting them under `boilerplate/` keeps that boundary clear: the repo root is the workshop,
and `boilerplate/` is what ships.
