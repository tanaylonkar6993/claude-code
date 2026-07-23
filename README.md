# claude-code

Monorepo of small projects built with [Claude Code](https://claude.com/claude-code). Each sub-project lives in its own top-level folder:

| Folder | What it is |
|---|---|
| [`portfolio/`](portfolio/) | Personal portfolio website — Astro static site, served by nginx in Docker, exposed via ngrok. See [`portfolio/README.md`](portfolio/README.md) for architecture, setup, and decision records. |
| [`issue-fixer/`](issue-fixer/) | Agent definition for an automated maintainer: a weekly cloud routine that picks one open issue in this repo and opens a pull request fixing it (never merges). |
| [`mock-interview`](https://github.com/tanaylonkar6993/mock-interview) | Claude Code skill for mock technical interviews (DevOps/SRE/Cloud/…) with a session-history dashboard — now its own repo. |

## Repo conventions

- Secrets live in a gitignored `.env` at the repo root (`portfolio/.env.example` documents the expected keys). Never committed.
- `.claude/` wires the sub-projects into Claude Code: `agents/issue-fixer.md` symlinks into `issue-fixer/`, and `skills/mock-interview` symlinks out to the standalone mock-interview repo.
