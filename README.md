# Next.js AI-ready starter

A Next.js 16 and React 19 starter with clear architecture guidance for developers and AI coding agents. It uses TypeScript, Tailwind CSS, coss/Base UI components, Biome, and OpenSpec.

## Get started

```bash
pnpm install
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000).

## Project structure

- `src/app/` contains routes, layouts, and route-level composition.
- `src/shared/` contains generic UI, hooks, and utilities.
- Add `src/features/` for user-facing capabilities and `src/entities/` for reusable domain concepts when needed.
- `AGENTS.md` and `docs/` define the engineering rules and agent workflow.
- `openspec/` holds specifications and planned changes.
- `.agents/skills/` contains task-specific agent skills.

The dependency direction is `app → features → entities → shared`. Read [AGENTS.md](AGENTS.md) and [the engineering docs](docs/README.md) before changing code.

## Agent skills

The repository includes skills for Next.js architecture, coss components, OpenSpec workflows, feature delivery, and change review. It also includes **Caveman** skills for concise replies, commit messages, reviews, and memory compression. These skills guide agent behavior; they do not add a runtime dependency to the app. Caveman Cloud workflows require separate setup.

## Quality commands

```bash
pnpm typecheck
pnpm lint
pnpm format:check
pnpm check
pnpm build
```

Run `pnpm format` to apply Biome formatting. The project does not yet have a test runner or `test` script. See [AGENTS.md](AGENTS.md) for the required verification and reporting process.
