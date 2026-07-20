# 0005. Content curated by planning assistant, reviewed/edited by user before publish

- Date: 2026-07-18
- Status: Accepted

## Decision

GitHub repo data was fetched live via `gh api` / GraphQL (name, primary language, fork status, README content) and used to draft `profile.json` and `projects.json` content. The user reviews and can further edit this content (descriptions, fork-inclusion, featured flags) at any time.

## Context

Avoids a live/build-time GitHub API dependency at runtime (see 0001) while still bootstrapping realistic, accurate content instead of placeholder text.

## Alternatives Considered

- User writes all copy from scratch — slower to reach a reviewable first draft.
- Automated description generation at build time — reintroduces the live-fetch dependency rejected in 0001.

## Consequences

- All 6 of the user's public repos are included. Only `Jenkins_SharedLib` is an actual GitHub fork (verified via GraphQL `isFork`) and is fork-badged; `wanderlust-azure` and `Taska-ToDo-List-App` are the user's own repos even though their READMEs credit tutorial/mega-project origins, so descriptions frame that honestly without a fork badge.
- `language` fields were verified against GitHub's actual `primaryLanguage` (e.g. `Taska-ToDo-List-App` is `EJS`, not `JavaScript`) rather than guessed from descriptions.
