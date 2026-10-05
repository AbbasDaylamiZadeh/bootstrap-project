# Data Fetching

## Principles

1. Fetch data as close as practical to where it is needed.
2. Avoid duplicate requests.
3. Use the framework's server capabilities where appropriate.
4. Consider adding TanStack Query when client caching/synchronization is required.
5. Keep API infrastructure separate from feature UI.

## Server Data

For server-rendered data, prefer Server Components and server-capable APIs.

## TanStack Query

When TanStack Query is installed, use it for interactive client-side server state such as:
- user-specific data
- frequently refreshed data
- mutation-driven state
- client cache requirements

## Query Keys

Query keys must be deterministic and structured.

Example:

```ts
['products', { category, page }]
```

Prefer centralized query-key conventions for large domains.

## Mutations

After mutation, decide explicitly whether to:
- invalidate
- update cached data
- remove cached data
- refetch

Do not invalidate unrelated queries.

## Optimistic Updates

Use optimistic updates only when:
- the UX benefit is meaningful
- rollback is reliable
- the mutation semantics are well understood

## Waterfalls

Avoid:

```text
request A
  ↓
request B
  ↓
request C
```

when requests can safely execute in parallel.

Use parallel fetching when dependencies do not exist.

## Caching

Caching must reflect business correctness.

Do not cache sensitive user-specific data globally.

Document unusual cache behavior.
