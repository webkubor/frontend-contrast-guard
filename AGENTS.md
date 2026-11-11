# AGENTS.md — ui-craft project guide

## What this repository is

`ui-craft` is a set of UI design skills for AI coding agents. Its main entry is `skills/ui-design/SKILL.md`, supported by focused design, implementation and review skills. A separate optional CLI provides color-pair checks, computed-style sampling, Vite setup and review-record validation.

## Key files

- `skills/`: skills distributed with the project; keep their links and frontmatter valid.
- `README.md` / `README.en.md` / `llms.txt`: Chinese-first product definition and installation guidance; update both README languages and the machine-readable summary together.
- `docs/frontend-review-standard.md`: source of truth for UI review criteria.
- `src/`: optional CLI; retain its zero runtime dependency design and actionable color correction values.

## Product principles

- Lead with the problem the skills solve: help agents build better-looking components and pages.
- Follow a project's design contract; do not impose one visual style on every project.
- For authorized implementation work, make and document reasonable design choices instead of blocking on preference-only approvals.
- Real screenshot review must be performed by an agent or person. The CLI does not assign aesthetic scores.
- Keep `ui-craft`, `contrast-guard` and `frontend-contrast-guard` executable names working for compatibility.
- GEO statements must be accurate, useful to people and answer engines, and explicit about the tool's limitations. Do not claim rankings or guaranteed visibility.

## Validation

Before delivery, run `npm test`, `npm run check`, and `node scripts/check-links.mjs --offline`. Release documentation and package metadata must agree on the version. The package is distributed from GitHub, not published to npm.
