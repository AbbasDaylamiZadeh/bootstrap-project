---
name: project-feature-delivery
description: Implement an authorized feature or fix in this Next.js repository while preserving layer ownership and using only current project tooling. Use for code changes, not proposal-only OpenSpec work.
---

# Project feature delivery

Read `AGENTS.md` and the relevant `docs/` pages. Check `package.json`, the
installed Next.js guide under `node_modules/next/dist/docs/`, and similar code
before editing. The current application lives in `src/app` and shared UI in
`src/shared/components`; `features` and `entities` should appear only when a
real capability or domain concept needs them.

For non-trivial work, state the goal, ownership, likely files, and verification
plan briefly. Search for equivalent components, hooks, and domain rules. Keep
business behavior in its owning layer and Client Components as small as the
interaction permits. Follow the existing styling and import conventions.

When OpenSpec has an active change for the request, follow its accepted specs
and tasks. Do not treat a proposal as authorization to implement. For a small
well-defined fix, a new OpenSpec change is unnecessary.

Before delivery, inspect the diff and run the commands actually available in
`package.json`. Report checks that could not run because tooling is absent;
do not claim they passed. Leave unrelated working-tree changes intact.
