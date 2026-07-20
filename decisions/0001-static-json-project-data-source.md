# 0001. Use a hand-curated static JSON file as the project data source

- Date: 2026-07-18
- Status: Accepted

## Decision

Project listings live in `src/data/projects.json`, a hand-curated static JSON file checked into the repo. No live GitHub API calls at runtime, no build-time fetch script.

## Context

v1 has no backend or database. Project descriptions benefit from human editing (marketing framing, honesty about forks/tutorial origins) rather than raw GitHub API text. Keeping it static avoids GitHub API rate limits, build-time network dependencies, and runtime fetch latency.

## Alternatives Considered

- Live GitHub API call from the browser at runtime — adds client JS, rate-limit risk, slower first paint.
- Build-time fetch script — adds a build-time network dependency and complexity not needed for a handful of hand-picked repos.

## Consequences

- Adding/removing/reordering a project requires a manual JSON edit + redeploy.
- The schema (`src/data/projects.json`) is wrapped in an envelope (`source`, `updatedAt`, `projects`) so it matches what a future `/api/projects` endpoint could return — swapping the data source later doesn't require touching component code, only the fetch mechanism.
