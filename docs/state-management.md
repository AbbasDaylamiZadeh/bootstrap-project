# State Management

## State Categories

| State | Preferred owner |
|---|---|
| URL/search/filter state | URL |
| Server state | Server Components; TanStack Query when installed for client caching |
| Local UI state | React state |
| Form state | Form library/local form state |
| Cross-cutting client state | An installed store only when justified |
| Theme/preferences | dedicated client store where appropriate |

## Server State

Server data should not be copied into global client state simply to make it accessible.

When TanStack Query is installed, use it for:
- fetching
- caching
- refetching
- mutations
- invalidation
- synchronization

## URL State

If a value affects navigation, sharing, bookmarking or SEO, consider URL state.

Examples:
- search query
- filters
- sort
- pagination
- selected category

## Local State

Use `useState` for local UI state.

Examples:
- modal open/close
- selected tab
- temporary input state
- accordion state

## Zustand

Consider a client store such as Zustand only when state genuinely needs:
- global client ownership
- cross-tree access
- persistence
- client-only synchronization

Do not use a client store as a replacement for server-state caching.

## Derived State

Prefer deriving values:

```ts
const total = items.reduce(...)
```

instead of synchronizing another state variable with an effect.

## State Ownership Rule

Every state value should have one clear owner.

Avoid multiple sources of truth.
