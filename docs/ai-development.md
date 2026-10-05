# AI Development Workflow

## Purpose

AI agents are engineering assistants, not unrestricted code generators.

The repository's architecture and quality gates are the source of truth.

## Current Tooling

`package.json` is authoritative for installed dependencies and scripts. The
current project uses Biome for linting and formatting, and has no test runner or test script.
TanStack Query, Orval, Axios, Vitest, Zustand, react-hook-form, and Zod
appear in architecture guidance as choices for future needs. Do not treat them
as installed infrastructure.

OpenSpec is initialized in `openspec/`. Use a change proposal when requirements
or architecture need an explicit record. For a small fix with clear acceptance
criteria, implement directly. Check for an existing spec or active change first.

## Before Coding

The agent should:

1. Read `AGENTS.md`.
2. Read relevant `docs/*`.
3. Inspect the existing implementation.
4. Search for similar features.
5. Search for existing components, hooks, helpers, formatters and domain calculations.
6. Inspect their call sites before deciding to reuse, extend or create.
7. Identify API/data/state patterns.
8. Identify tests for similar behavior.
9. Check generated-code boundaries.
10. Produce a concise plan.

## Planning

For a non-trivial change, the plan should include:

```text
Goal
Scope
Files likely to change
Architecture/layer
Data flow
State ownership
API impact
Testing strategy
Risks
```

## Implementation

Implement the smallest correct change.

Repository inspection is part of implementation, not an optional preliminary
step. Do not add a new component or helper merely because creating one is faster
than finding the existing owner.

Do not:
- refactor unrelated code
- rename unrelated things
- rewrite existing architecture without approval
- add dependencies unnecessarily
- create speculative abstractions

## Agent Context

Good repository context includes:

```text
AGENTS.md
architecture.md
project-structure.md
coding-rules.md
testing.md
api-guidelines.md
```

Additional context should be added through task-specific instructions rather than making `AGENTS.md` enormous.

## MCP

When MCP tools are available, agents should use only approved tools and scopes.

Examples:
- GitHub for repository/PR work
- Figma for design context
- Playwright for browser verification
- observability systems for diagnostics

Never grant an agent broader permissions than the task requires.

## Review

After implementation, the agent should inspect:

```bash
git diff
git status
```

Then verify:
- architecture boundaries
- unnecessary changes
- error handling
- tests
- type safety
- accessibility
- performance implications

## Quality Gates

Run:

```text
pnpm typecheck
pnpm lint
pnpm format:check
pnpm test (when configured)
pnpm build
```

Use the repository's actual package scripts rather than assuming command names.

## PR Description

An AI-generated PR should clearly state:

- what changed
- why
- architecture impact
- tests
- known limitations

## Human Review

Human review remains required for:
- security-sensitive changes
- architecture changes
- authentication/authorization
- payment logic
- destructive operations
- major dependency changes
- database/API contract changes
- broad refactors

## Agent Success Criteria

A good AI implementation is not the implementation with the most code.

It is the smallest change that:
- solves the requested problem
- fits the architecture
- follows repository rules
- has appropriate tests
- passes quality gates
- is understandable to the next developer
