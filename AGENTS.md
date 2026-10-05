<!-- BEGIN:nextjs-agent-rules -->

# This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` (resolved from this file's directory; in monorepos the `next` package may not be visible from the repo root) before writing any code. Heed deprecation notices.

This block is written and re-added by `next dev` — verify at `node_modules/next/dist/server/lib/generate-agent-files.js`. Removing it from a diff only re-creates the uncommitted change; committing it with your work keeps the tree clean.

<!-- END:nextjs-agent-rules -->


# AGENTS.md — Next.js Frontend Engineering Guide

## Purpose

This file is the primary engineering contract for AI coding agents working in this repository.

Before changing code, an agent MUST:
1. Read this file.
2. Read the relevant documents under `docs/`.
3. Inspect existing patterns before introducing a new pattern.
4. Understand the dependency direction and layer ownership.
5. Produce a short implementation plan for non-trivial changes.

## Current Technology Baseline

- Next.js App Router
- React
- TypeScript with strict type checking
- pnpm
- Server Components by default
- Biome for linting and formatting (see `package.json` scripts)

TanStack Query, Orval, Axios, Vitest, Zustand, react-hook-form, and Zod
are documented options, not installed project defaults. Do not import or require
them until a concrete feature needs them and they have been added deliberately.
Inspect `package.json` and existing code before choosing a library or command.

## Architecture

The dependency direction is:

`app → features → entities → shared`

Rules:

- `app` owns routes, route composition, metadata, loading, error and not-found boundaries.
- `features` own business capabilities and user actions.
- `entities` own domain models and reusable domain UI.
- `shared` owns generic UI, utilities, infrastructure and cross-cutting primitives.
- `shared` MUST NOT import from `entities`, `features`, or `app`.
- `entities` MUST NOT import from `features` or `app`.
- `features` SHOULD NOT depend directly on other features.
- Avoid circular dependencies.
- Do not create a new abstraction until an existing abstraction has been inspected.

See:
- `docs/architecture.md`
- `docs/project-structure.md`

## Server vs Client Components

Default to Server Components.

Use `"use client"` only when the component requires:
- browser APIs
- React state/effects
- event handlers
- client-only libraries
- client-side subscriptions

Keep client boundaries as small as practical.

Do NOT make an entire route/page client-rendered merely because one child needs interactivity.

## Data and API Rules

- Use TanStack Query for client-side server state only if it has been added for a
  concrete caching/synchronization need.
- Do not duplicate server state in a client store or arbitrary React state.
- If Orval is introduced, its generated code is read-only.
- Never manually edit generated API files.
- Put custom API behavior in the appropriate non-generated infrastructure layer.
- Query keys must follow a consistent factory/pattern.
- Mutations must invalidate or update affected queries deliberately.
- Handle loading, empty, error and success states explicitly.

See `docs/data-fetching.md` and `docs/api-guidelines.md`.

## Reuse Before Creation

Before creating a component, hook, utility, helper, formatter, or domain
calculation, the agent MUST:

1. Search the repository for exact and semantically equivalent implementations.
2. Inspect how existing implementations are used and which layer owns them.
3. Reuse or compose an existing implementation when it satisfies the requirement.
4. Extend an existing implementation only when the new behavior fits its
   responsibility without creating misleading props or breaking existing consumers.
5. Create a new implementation only when no suitable implementation exists or
   changing the existing one would mix unrelated responsibilities.

Do not duplicate business rules inside components.

A business calculation must have one authoritative implementation in its owning
domain. Components should consume that implementation rather than creating
component-specific helper functions.

Do not create a separate helper merely to wrap a trivial one-time expression.
Extract logic when it represents a business rule, is reused, requires testing,
or materially improves readability.

## State Management

Choose state based on ownership:

- URL state → search params / router
- Server state → Server Components; TanStack Query when installed for client caching
- Local component state → `useState` / reducer
- Cross-component client state → a shared store only when genuinely required
- Form state → local state or an installed form library when needed
- Validation → an installed schema library when its benefit justifies it

Do not introduce global state for data that can remain local.

## TypeScript

- Avoid `any`.
- Prefer explicit domain types at boundaries.
- Do not hide errors with unsafe casts.
- Do not use `as` to silence legitimate type errors.
- Use discriminated unions when they improve state modeling.
- Keep types close to the domain that owns them unless they are truly shared.

## Components

Prefer small, composable components.

Component size is a design constraint:

- Aim for no more than 150 lines for one component implementation.
- A hand-written component file SHOULD stay at or below 200 lines.
- A component file above 200 lines MUST be reviewed and split by responsibility,
  unless keeping it together has a documented cohesion reason.
- Do not define a React component inside another component's function body.
- Small private subcomponents may remain in the same file while the file stays
  focused and within the size limit.
- Do not extract meaningless wrappers merely to reduce the line count.

Do not put business logic into generic UI components.

See `docs/frontend-guidelines.md` for extraction rules and examples.

## Styling

Follow the existing project styling system and design tokens.

Do not introduce a second styling solution without explicit approval.

Avoid hard-coded values when an existing design token or shared primitive exists.

## Testing

Every meaningful feature/change should include appropriate tests.

At minimum:
- business logic → unit tests
- important component behavior → component tests
- critical user flows → integration/E2E tests when available

Do not write tests that merely mirror implementation details.

## Required Quality Checks

After implementation, run:

1. `pnpm typecheck`
2. `pnpm lint` and `pnpm format:check`
3. `pnpm test` when a test script exists; otherwise report that no test runner is configured
4. `pnpm build`

Report every failure and distinguish:
- application failure
- test failure
- tooling/environment failure

Never claim checks passed if they were not actually run.

## OpenSpec

OpenSpec is initialized in `openspec/`. Its CLI and change skills support
planning and tracking changes. Use it for a new capability, a changed user-facing
contract, or a meaningful architecture/API decision. A small, well-defined fix
can go directly through the implementation workflow. Do not create a spec merely
to satisfy a process step.

- Check `openspec/specs/` and active changes before proposing overlapping work.
- Keep proposals, specs, design, and tasks consistent with the actual installed
  stack and this repository's architecture.
- A proposal is planning; implementation starts only when the user requests it.
- Mark tasks complete only after their specified behavior is implemented and
  verified. Archive after the change is complete.

See `docs/ai-development.md` and `openspec/config.yaml`.

## Project Skills

- `project-feature-delivery`: implement an authorized feature or fix using this
  repository's current stack and layer ownership.
- `project-change-review`: review a diff for requirement, architecture, and
  verification gaps before delivery.

Use the relevant skill when its task applies. Skills supplement this file and
the relevant `docs/` pages; they do not override them.

## Change Discipline

Agents MUST:
- avoid unrelated refactors
- avoid changing public APIs without a reason
- avoid renaming unrelated files
- avoid formatting the entire repository unnecessarily
- preserve existing behavior unless the task explicitly changes it
- inspect git diff before finishing

For risky changes, explain the impact before implementation.

## Generated Code

Generated files must not be manually edited.

Typical generated areas may include:

`src/shared/api/generated/`

If generated code needs to change:
1. identify the source contract/configuration
2. update the source
3. regenerate
4. inspect the generated diff

## AI Workflow

For a non-trivial task:

```text
Understand
  ↓
Inspect existing patterns
  ↓
Plan
  ↓
Implement smallest correct change
  ↓
Test
  ↓
Review diff
  ↓
Run quality checks
  ↓
Report
```

Agents should optimize for correctness and maintainability, not number of files changed.

## Completion Report

After implementation, report:

### Files Changed
- ...

### Architecture Decisions
- ...

### Tests
- ...

### Quality Checks
- Typecheck: PASS/FAIL
- Biome: PASS/FAIL
- Tests: PASS/FAIL
- Build: PASS/FAIL

### Remaining Issues
- ...

## Forbidden Behaviors

Do not:
- bypass TypeScript errors
- disable lint rules to make a change pass
- modify generated code manually
- introduce duplicate API clients
- create arbitrary global state
- move code between layers without architectural justification
- refactor unrelated code
- install dependencies without necessity
- change configuration files casually
- claim success without verification
