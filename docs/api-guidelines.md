# API Guidelines

## Generated API

If Orval is used, generated files are source-of-truth outputs.

Example:

```text
src/shared/api/generated/
```

Never manually edit generated files.

## API Layers

A recommended separation:

```text
shared/api/
├── generated/
├── client/
├── query/
└── errors/
```

Generated code handles contract-derived APIs.

Infrastructure handles:
- HTTP client
- authentication transport
- serialization
- common error normalization

Feature/domain code handles business meaning.

## Axios

If Axios is used:
- configure a single shared instance where appropriate
- centralize transport concerns
- do not create ad-hoc Axios instances inside features
- configure timeouts deliberately
- preserve useful error information

## Errors

Normalize transport errors at the infrastructure boundary.

Feature code should be able to reason about meaningful error categories without depending on raw Axios internals everywhere.

## Authentication

Authentication transport should be centralized.

Do not scatter token handling across components.

Never log credentials, tokens or sensitive headers.

## Query/Mutation Hooks

Generated hooks should be consumed according to project conventions.

Do not wrap every generated hook automatically.

Create a custom wrapper only when it adds meaningful domain behavior, normalization or reusable policy.

## API Contract Changes

When the backend contract changes:

1. update the contract/source
2. regenerate Orval output
3. inspect generated changes
4. update consumers
5. update tests
6. run all quality checks
