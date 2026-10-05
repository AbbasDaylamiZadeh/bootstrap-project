# Bootstrap Project

A frontend starter for building Next.js applications. It provides an App Router shell, a collection of reusable UI components, theme support, and documented conventions for adding product features. The repository is a starting point, not a finished application: the home page currently shows a simple “Project ready!” screen.

## What's included

- Next.js 16 App Router, React 19, and strict TypeScript
- Tailwind CSS 4 with light and dark themes
- Reusable coss-style components built primarily with Base UI primitives
- Biome for linting and formatting
- Engineering guidance in [`AGENTS.md`](AGENTS.md) and [`docs/`](docs/README.md)
- OpenSpec configuration for planning larger changes

There are no business features, backend integrations, or automated tests yet. Add those as your application needs them.

## Run locally

Install dependencies with [pnpm](https://pnpm.io/), then start the development server:

```bash
pnpm install
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000). Press `d` to switch between light and dark themes when you are not typing in a control.

## Project structure

```text
src/
├── app/                  Routes, layout, and global styles
└── shared/
    ├── components/       Theme provider and reusable UI
    ├── hooks/            Generic React hooks
    └── lib/              Shared utilities
```

As the application grows, put business capabilities in `src/features/` and reusable domain concepts in `src/entities/`. Those directories are intentionally absent until needed. The dependency direction is `app → features → entities → shared`; see the [architecture guide](docs/architecture.md) for ownership rules.

## Available commands

| Command | Purpose |
| --- | --- |
| `pnpm dev` | Start the development server |
| `pnpm build` | Create a production build |
| `pnpm start` | Serve a production build |
| `pnpm typecheck` | Check TypeScript types |
| `pnpm lint` | Run Biome linting |
| `pnpm format:check` | Check formatting with Biome |
| `pnpm format` | Apply Biome formatting |
| `pnpm check` | Run Biome checks |

No `test` script is configured yet.

## Contributing to this starter

Read [`AGENTS.md`](AGENTS.md) before changing code. The [documentation index](docs/README.md) links to the architecture, frontend, data, testing, and security guidelines. For a new capability or a significant contract change, use the initialized [`openspec/`](openspec/) workflow to plan the work before implementation.
