# 0007. Pin Astro to 4.x locally due to Node 18 on the dev host

- Date: 2026-07-18
- Status: Accepted

## Decision

`package.json` pins `"astro": "^4.16.0"` and the project was hand-scaffolded (config files written directly) instead of running `npm create astro@latest`, which requires Node 22+.

## Context

The local dev machine has Node v18.19.1. `create-astro@5.x` hard-fails below Node 22. Astro 5's own runtime requirement (`^18.20.8 || ^20.3.0 || >=22.0.0`) is also tighter than what's installed locally. Astro 4.x supports `^18.17.1`, which the local Node satisfies.

## Alternatives Considered

- Upgrade local Node via a version manager — out of scope for this task; not installed on this host.
- Use Astro 5 anyway — `npm install` would emit `EBADENGINE` warnings and risked subtler runtime incompatibilities.

## Consequences

- The Docker build stage uses `node:20-alpine` (see `Dockerfile`), which satisfies Astro 5's requirements — so the *container* is not actually constrained by this. This pin is purely a local-dev-host workaround.
- Revisit this pin (bump to Astro 5) once local Node is upgraded, or drop the constraint since the Docker build doesn't need it.
