# Project context for AI agents

Snapshot: 2026-10-05. This is a portable, copyable summary of the repository at the time of writing. If an agent has access to the repository, the current `AGENTS.md`, `package.json`, source files, and relevant `docs/` pages take precedence over this snapshot. A pasted copy of this document does not give an agent access to local skill files or installed tools.

## What this project is

`bootstrap-project` is a private Next.js 16 / React 19 frontend starter. It provides an App Router shell, a themed coss/Base UI component collection, strict TypeScript, Tailwind CSS 4, Biome, architecture documentation, OpenSpec configuration, and task-specific agent skills. It is not yet a completed ecommerce application or another product. There are no implemented business features, domain entities, backend integration, authentication flow, or business forms.

The only route is `/`. It displays a small “Project ready!” starter page and a shared `Button`. The root layout loads Inter and Geist Mono through `next/font/google`, imports global styles, and wraps the page in `next-themes`. Pressing `d` toggles light and dark mode when focus is not in a typing control.

## Current source layout

```text
src/
├── app/
│   ├── globals.css             Tailwind imports, tokens, light/dark themes
│   ├── layout.tsx              root HTML, fonts, ThemeProvider
│   ├── page.tsx                starter home page
│   └── favicon.ico
└── shared/
    ├── components/
    │   ├── theme-provider.tsx   next-themes and the d hotkey
    │   └── ui/                  54 generic UI component files
    ├── hooks/
    │   └── use-media-query.ts  useMediaQuery and useIsMobile
    └── lib/
        ├── segmented-control.ts shared segmented-control styles
        └── utils.ts            cn() via clsx and tailwind-merge
```

`src/features/` and `src/entities/` do not exist yet. Add them only when a concrete business capability or domain concept requires them. `src/shared/api/generated/` does not exist.

The current shared UI files are: `accordion`, `alert-dialog`, `alert`, `autocomplete`, `avatar`, `badge`, `breadcrumb`, `button`, `calendar`, `card`, `checkbox-group`, `checkbox`, `collapsible`, `combobox`, `command`, `context-menu`, `dialog`, `drawer`, `empty`, `field`, `fieldset`, `form`, `frame`, `group`, `input-group`, `input`, `kbd`, `label`, `menu`, `meter`, `number-field`, `otp-field`, `pagination`, `popover`, `preview-card`, `progress`, `radio-group`, `scroll-area`, `select`, `separator`, `sheet`, `sidebar`, `skeleton`, `slider`, `spinner`, `switch`, `table`, `tabs`, `textarea`, `toast`, `toggle-group`, `toggle`, `toolbar`, and `tooltip` (`.tsx` files under `src/shared/components/ui/`). Many interactive components wrap `@base-ui/react` primitives.

## Packages actually declared in `package.json`

The version strings below are the declared ranges, not a claim about every resolved transitive version. `pnpm-lock.yaml` records the resolved dependency graph.

### Runtime dependencies

| Package | Declared version | Role |
| --- | --- | --- |
| `next` | `16.3.8` | App Router framework |
| `react` | `19.3.0` | React components |
| `react-dom` | `19.3.0` | React DOM renderer |
| `@base-ui/react` | `^1.8.0` | Accessible UI primitives used by coss-style components |
| `@daypicker/react` | `^10.0.2` | Calendar component |
| `class-variance-authority` | `^0.7.1` | Component variants |
| `clsx` | `^2.1.1` | Conditional class names |
| `cn` | `^0.4.0` | Declared dependency; inspect actual code before using it |
| `lucide-react` | `^1.52.0` | Icons |
| `next-themes` | `^0.4.6` | Light/dark/system theme |
| `radix-ui` | `^1.6.7` | Declared dependency; the inspected shared primitives predominantly use Base UI |
| `shadcn` | `^4.21.1` | Component registry/CLI package and styling integration |
| `tailwind-merge` | `^3.7.0` | Merge Tailwind class names in `cn()` |
| `tw-animate-css` | `^1.4.0` | Animation utilities imported by global CSS |

### Development dependencies

| Package | Declared version | Role |
| --- | --- | --- |
| `@biomejs/biome` | `2.5.14` | Linting and formatting |
| `@tailwindcss/postcss` | `^4` | Tailwind PostCSS integration |
| `@types/node` | `^26.6.4` | Node types |
| `@types/react` | `^19` | React types |
| `@types/react-dom` | `^19` | React DOM types |
| `tailwindcss` | `^4` | CSS utility framework |
| `typescript` | `^7.0.2` | Type checking |

### Tools and libraries not currently installed

TanStack Query, Orval, Axios, Vitest, Zustand, React Hook Form, Zod, and Valibot are not current project infrastructure. Some appear in documentation as possible future choices. Do not import them, assume generated API hooks, or refer to a configured test runner unless the current repository has added them. There is no `test` or `api:generate` script in `package.json`.

## Commands and configuration

Use pnpm. The declared scripts are:

```text
pnpm dev           next dev
pnpm build         next build
pnpm start         next start
pnpm lint          biome lint .
pnpm format        biome format --write .
pnpm format:check  biome format .
pnpm check         biome check .
pnpm typecheck     tsc --noEmit
```

Run `pnpm install` and `pnpm dev` for local development, then open `http://localhost:3000`. There is no test script or installed test runner yet. For application changes, `AGENTS.md` requires `pnpm typecheck`, `pnpm lint`, `pnpm format:check`, and `pnpm build`; run `pnpm test` only if a test script is later added. Report the actual outcome of each check.

- `tsconfig.json`: strict TypeScript; `@/*` maps to `./src/*`.
- `biome.json`: recommended lint rules, two-space indentation, 80-column formatting, double quotes, semicolons, and import organization. Biome handles both lint and format.
- `postcss.config.mjs`: uses `@tailwindcss/postcss`.
- `src/app/globals.css`: imports Tailwind, `tw-animate-css`, and `shadcn/tailwind.css`; defines semantic color and radius tokens for light and dark themes.
- `components.json`: component aliases point into `src/shared`; coss registry is `@coss`; `rtl` is set to `true`. The current root HTML still sets `lang="en"`, so inspect actual product requirements before assuming a language or direction.
- `next.config.ts`: currently empty configuration.
- `pnpm-workspace.yaml`: no workspace packages; controls selected dependency build permissions.
- `CLAUDE.md`: forwards to `AGENTS.md`.

## Engineering contract

`AGENTS.md` is the primary repository instruction file. The architecture direction is:

```text
app → features → entities → shared
```

`app` owns routes, layouts, metadata, boundaries, and route composition. `features` owns business capabilities and user actions. `entities` owns domain concepts and reusable domain UI. `shared` owns generic UI, utilities, hooks, and infrastructure. Lower layers must not import higher layers; avoid feature-to-feature dependencies and cycles.

Default to Server Components. Use `"use client"` only for browser APIs, React state/effects, event handlers, subscriptions, or client-only libraries, and keep the client boundary small. The existing theme provider and interactive UI primitives are client components; the starter page and root layout are server components.

Before creating a component, hook, helper, calculation, or abstraction, search for an equivalent implementation and inspect its consumers. Reuse or compose existing code where it fits. Keep authoritative business rules in their owning domain, not duplicated inside components. Use local state for local interaction, URL state for shareable navigation state, and server rendering for server data. Introduce a client cache or global store only for a demonstrated need.

Keep components focused: aim for at most 150 lines per component implementation; review and split hand-written component files above 200 lines unless cohesion justifies them. Preserve accessibility, explicit loading/empty/error/success states, strict types, and security boundaries. Do not manually edit generated code if it is introduced later.

For non-trivial changes, inspect existing patterns and relevant docs, state a brief implementation plan, implement the smallest correct change, review the diff, and run the quality checks. Leave unrelated changes alone. Before writing Next.js application code, read the relevant **version-matched** guide under `node_modules/next/dist/docs/`; this project warns that its Next.js APIs and conventions may differ from prior versions.

## Project documentation

| File | Main purpose |
| --- | --- |
| `AGENTS.md` | Primary agent and engineering rules |
| `docs/README.md` | Documentation index and source-of-truth guidance |
| `docs/architecture.md` | Layer boundaries and dependency direction |
| `docs/project-structure.md` | Directory ownership and file placement |
| `docs/coding-rules.md` | TypeScript, imports, dependencies, configuration |
| `docs/frontend-guidelines.md` | Components, forms, accessibility, UX, rendering |
| `docs/state-management.md` | Ownership of URL, server, local, and form state |
| `docs/data-fetching.md` | Server data and optional client caching guidance |
| `docs/api-guidelines.md` | Guidance if API/Orval/Axios infrastructure is introduced |
| `docs/testing.md` | Behavior-focused test strategy |
| `docs/performance.md` | Rendering, bundle, and fetching performance |
| `docs/security.md` | Secrets, auth, validation, URLs, and sensitive data |
| `docs/ai-development.md` | Agent workflow and quality gates |

## OpenSpec status

OpenSpec has been initialized with the `spec-driven` schema in `openspec/config.yaml`. The configuration restates the real stack and architecture. At this snapshot, `openspec/specs/` and `openspec/changes/` have no active specification or change files beyond `.gitkeep` placeholders. Use OpenSpec for a new capability, changed user-facing contract, or meaningful architecture/API decision. A small, well-defined fix may be implemented directly. A proposal is planning; implementation begins when requested.

## Local agent skills

The following 21 skills have `SKILL.md` files under `.agents/skills/`. They guide agent behavior; they are not application runtime dependencies. A copied version of this document describes them but does not install their full instructions or supporting resources in another environment.

| Skill | When to use / what it does |
| --- | --- |
| `project-feature-delivery` | Implement an authorized feature or fix with repository ownership, reuse, and verification rules. |
| `project-change-review` | Review a diff for missed requirements, duplicated business logic, wrong layer ownership, and unsupported verification claims. |
| `form-development` | Build or update forms using current field primitives and the form/data libraries actually installed. |
| `nextjs-app-architecture` | Design or audit Next.js 16 App Router, RSC composition, data/action placement, Suspense, and client boundaries. |
| `vercel-react-best-practices` | Apply React/Next.js performance guidance for fetching, bundles, rendering, and rerenders. |
| `coss` | Compose coss/Base UI primitives correctly, including overlays, controls, styling, and accessibility. |
| `coss-particles` | Find practical coss UI patterns and adapt their component examples. |
| `openspec-explore` | Explore requirements and design choices before or during an OpenSpec change. |
| `openspec-propose` | Create planning artifacts for a new OpenSpec change; does not implement application code. |
| `openspec-update-change` | Reconcile existing OpenSpec planning artifacts with new decisions; does not edit application code. |
| `openspec-apply-change` | Implement the tasks of an existing OpenSpec change and track completion. |
| `openspec-sync-specs` | Apply a change's delta specifications to the main specifications without archiving. |
| `openspec-archive-change` | Check and archive a completed OpenSpec change. |
| `caveman` | Use a brief, direct response style while preserving technical facts. |
| `caveman-commit` | Write concise Conventional Commits messages. |
| `caveman-review` | Present code review findings in a terse location/problem/fix format. |
| `caveman-compress` | Compress memory or instruction text to save tokens while retaining a readable backup. |
| `caveman-discover` | Identify and label LLM workflows for Caveman Cloud cost grouping; requires its separate tooling/setup. |
| `caveman-optimize` | Evaluate a Caveman observation with an operator-selected candidate and paired baseline evaluation; requires separate tooling and approval. |
| `grill-me` | Intended to question and sharpen a plan or design. Its file currently delegates to a `Skill` tool named `grilling`; verify availability before relying on it. |
| `improve-codebase-architecture` | Intended to scan for architecture improvement opportunities, produce an HTML report, and discuss a selected candidate. It references other `Skill` tools plus `GLOSSARY.md` and `docs/adr/`, which are not currently present here; verify or adapt before use. |

`skills-lock.json` tracks the upstream sources of six imported skills: `coss`, `coss-particles`, `grill-me`, `improve-codebase-architecture`, `nextjs-app-architecture`, and `vercel-react-best-practices`. The other local skills exist in `.agents/skills/` but are not entries in that lockfile.

## Verification snapshot

These are observed results from 2026-10-05 while creating this documentation file, not a permanent claim about future builds:

- `pnpm typecheck`: passed.
- `pnpm format:check`: passed.
- `pnpm lint`: failed on three existing accessibility diagnostics in `src/shared/components/ui/group.tsx` and `src/shared/components/ui/input-group.tsx`. This documentation task did not change those files.
- `pnpm build`: failed in this execution environment because Turbopack's CSS processing could not create a local process or bind to a port (`Operation not permitted`). Retrying with elevated command permissions produced the same environment error; no application compilation result was established.
- `pnpm test`: unavailable because no test script exists.

## What another agent should check first

1. Read the current `AGENTS.md`, `package.json`, and relevant `docs/` page if repository access is available.
2. Inspect `git status` and preserve any user changes already in the working tree.
3. Search existing source and shared UI before creating files or abstractions.
4. Read the installed Next.js guide relevant to the proposed code change.
5. Choose only skills, dependencies, scripts, and APIs that are actually available in that environment.
6. For non-trivial work, explain the short plan; after implementation, inspect the diff and report real quality-check results.
