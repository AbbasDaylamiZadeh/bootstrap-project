# Frontend Engineering Documentation

This directory contains the engineering contract used by developers and AI coding agents.

## Documents

- `../AGENTS.md` — primary AI/developer instructions
- `architecture.md` — architectural boundaries and dependency direction
- `project-structure.md` — directory and file organization
- `coding-rules.md` — TypeScript, React and general coding rules
- `frontend-guidelines.md` — component, rendering, accessibility and UX guidance
- `state-management.md` — state ownership rules
- `data-fetching.md` — server state and TanStack Query guidance
- `api-guidelines.md` — Orval, Axios and API rules
- `testing.md` — testing strategy
- `performance.md` — frontend performance rules
- `security.md` — frontend security rules
- `ai-development.md` — AI coding-agent workflow
- `../openspec/config.yaml` — OpenSpec context and artifact guidance
- `../.agents/skills/` — task-specific agent skills

## Source of Truth

`AGENTS.md` is the entry point.

When a document conflicts with the actual codebase, do not silently invent a third convention. Inspect the repository and resolve the discrepancy explicitly.

## Updating These Documents

Architecture changes should be reflected in the documentation.

When introducing a new pattern:
1. prove the pattern is needed
2. document ownership and boundaries
3. update the relevant rule
4. update examples
5. keep the rule concise and enforceable
