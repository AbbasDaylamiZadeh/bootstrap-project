# Performance

## Goals

Optimize for:
- fast initial render
- small client JavaScript
- responsive interaction
- efficient data fetching
- stable caching
- predictable rendering

## Next.js

Prefer framework capabilities:
- Server Components
- streaming
- route-level loading
- metadata
- optimized image handling
- appropriate caching

## Client JavaScript

Every Client Component creates a client boundary.

Keep client boundaries small.

Do not move a complete page to the client to support one interactive control.

## Images

Use the project's established image optimization strategy.

Avoid shipping unnecessarily large source images.

## Dependencies

Before adding a package, consider:
- bundle size
- tree shaking
- server/client compatibility
- maintenance
- whether the functionality is already available

## Rendering

Avoid unnecessary rerenders.

But do not blindly add memoization.

Measure first when performance is uncertain.

## Data Fetching

Avoid request waterfalls.

Parallelize independent work.

Do not fetch the same data repeatedly from multiple components without a reason.

## Performance Review

For meaningful performance work, record:
- current behavior
- bottleneck
- proposed change
- expected impact
- measurement method
- result
