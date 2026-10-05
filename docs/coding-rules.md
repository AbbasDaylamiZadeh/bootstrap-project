# Coding Rules

## General

1. Prefer the simplest correct implementation.
2. Follow existing patterns before introducing new ones.
3. Keep functions focused.
4. Avoid premature abstraction.
5. Make ownership explicit.
6. Prefer composition over inheritance.
7. Keep side effects at clear boundaries.

## TypeScript

Use strict typing.

Avoid:

```ts
const data: any = ...
```

Avoid:

```ts
const value = something as SomeType;
```

unless the boundary is genuinely trusted and the cast is justified.

Prefer narrowing:

```ts
if (isProduct(value)) {
  // value is Product
}
```

## Error Handling

Errors should be handled at the appropriate boundary.

Do not silently swallow errors:

```ts
try {
  await save()
} catch {
  // nothing
}
```

If an error is intentionally ignored, document why.

## Async Code

Prefer readable async flows.

Avoid unnecessary promise nesting.

Use `Promise.all` only when operations are independent.

Do not parallelize operations when ordering or rate limits require sequential execution.

## React

Prefer pure components.

Do not use effects for derived values.

Avoid:

```ts
useEffect(() => {
  setFullName(`${firstName} ${lastName}`)
}, [firstName, lastName])
```

Prefer:

```ts
const fullName = `${firstName} ${lastName}`
```

Use memoization only when it solves an identified performance or referential-equality problem.

## React.memo / useMemo / useCallback

Do not add these by default.

Use them when:
- profiling identifies expensive work
- stable references are required
- memoized children benefit from stable props
- computation is genuinely expensive

Memoization is not a substitute for good component architecture.

## Client Components

Keep `"use client"` boundaries narrow.

If only one button needs interaction, make the button client-side instead of converting its parent tree unnecessarily.

## Comments

Write comments explaining WHY, not WHAT.

Bad:

```ts
// Increment count
setCount(count + 1)
```

Good:

```ts
// The API requires a fresh cart version before checkout.
```

Remove obsolete comments.

## Imports

Keep imports clean and ordered according to project conventions.

Do not use relative paths that violate architectural boundaries.

## Dependencies

Before adding a dependency:
1. Check whether the project already provides the capability.
2. Check whether a small local implementation is sufficient.
3. Consider bundle size and maintenance.
4. Confirm the dependency is compatible with the project's Next.js/React versions.

Do not add a package for trivial functionality.

## Configuration

Configuration changes are high-impact.

Before changing:
- `next.config.*`
- TypeScript config
- lint/format config
- build config
- environment handling

inspect existing behavior and explain the reason.

## Environment Variables

Never expose secrets to client code.

Only variables explicitly intended for browser use should use the framework's public environment variable mechanism.

Never commit secrets.
