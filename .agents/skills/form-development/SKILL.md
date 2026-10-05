---
name: form-development
description: Build or update forms in this Next.js project. Use for field composition, validation, submission, server or API mutations, and form-related state updates. Follow the libraries and patterns actually present in the target feature.
---

# Form development

Build forms under the ownership rules in `AGENTS.md` and the relevant `docs/` pages. Before changing application code, inspect `package.json`, nearby implementations, and the relevant guide in `node_modules/next/dist/docs/`. The current starter has shared coss/Base UI form and field primitives, but no established business form, API client, form library, schema library, or test runner. Check the repository again when applying this skill; these facts may change.

## Place and compose the form

- Search for an existing form, validation rule, and suitable shared controls before creating anything. Inspect their consumers and follow the target feature's conventions.
- Keep routes and route composition in `src/app/`, business actions and form behavior in `src/features/<feature>/`, domain rules in `src/entities/<entity>/` when appropriate, and generic controls in `src/shared/`. Create feature or entity folders only for real capabilities or concepts. Preserve `app → features → entities → shared`.
- Keep the server-rendered page or layout on the server. Make only the interaction that needs browser state or event handlers a Client Component. Use the submission mechanism that fits the feature and the installed Next.js version.
- Reuse the existing `Form`, `Field`, `FieldLabel`, `FieldError`, `Input`, and `Button` where their APIs fit. They are coss/Base UI components, not React Hook Form wrappers. Consult the local `coss` skill and component references when composing or changing these primitives.

## Validate and submit

- Choose local state or an already installed form library according to the form's complexity and existing feature pattern. Do not assume React Hook Form, Zod, Valibot, or a resolver is installed. Add a dependency only when the feature needs it and it fits `AGENTS.md`.
- Keep reusable business validation with its owning feature or entity. Client validation improves feedback; validate untrusted input again at the trusted server boundary. Keep form values distinct from API payloads when their shapes differ.
- Give controls accessible labels, connect errors to fields, and show clear pending, success, and failure feedback. Match messages to the UI language. Do not expose raw backend errors unless they are safe and intended for users.
- Inspect the actual API and data layer before choosing a submission path. If generated operations exist, use them according to their contract and regenerate from their source rather than editing generated files. Do not invent endpoints, a generated client, an Axios mutator, auth storage, or global error handling.
- Keep server state in Server Components by default. If TanStack Query has been installed for a concrete client caching need, update or invalidate affected queries after a successful mutation using the project's query-key pattern. Do not add client cache infrastructure for a form that does not need it.

## Verify

Add behavior-focused tests when a test runner is available and the change warrants them. After application changes, run the scripts required by `AGENTS.md` that exist in `package.json`: `pnpm typecheck`, `pnpm lint`, `pnpm format:check`, and `pnpm build`; run `pnpm test` only when a test script exists. Inspect the diff and report results, unavailable tooling, and remaining issues accurately.
