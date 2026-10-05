# Frontend Guidelines

## Rendering

Default to Server Components.

Use Client Components for:
- interaction
- browser APIs
- local client state
- subscriptions
- client-only libraries

## Component Design

A component should have a clear responsibility.

Prefer:

```text
ProductPage
 ├── ProductGallery
 ├── ProductInfo
 └── AddToCart
```

over a single 500-line component.

### Reuse Before Creation

Repository inspection is required before adding a component or abstraction.

Search for:

- an existing component with the same semantic responsibility
- a primitive that can be composed to produce the requested UI
- an existing hook, formatter, mapper, validator or calculation
- an existing domain model or presentation component that should be extended

Prefer, in order:

1. reuse
2. composition
3. a coherent extension of the current owner
4. a new implementation

Do not force unrelated behavior into an existing component merely to avoid a new
file. Create a focused component when reuse would produce unrelated modes,
boolean-prop combinations or unclear ownership.

### Component and File Size

Line count is a warning signal, not a substitute for design judgment. Still,
hand-written component code must follow these limits:

- Aim for 150 lines or fewer for a component implementation.
- Keep a component file at 200 lines or fewer whenever practical.
- Above 200 lines, split the file by responsibility before adding more behavior.
- If a file must remain above 200 lines for cohesion, document the reason in the
  implementation plan or completion report.

Imports, types and simple constants do not by themselves justify extraction. A
split should create meaningful boundaries such as:

- data orchestration versus presentation
- list/carousel versus item/card
- interactive Client Component versus Server Component parent
- primary UI versus a substantial skeleton/loading implementation
- domain calculation versus rendering

Do not create many one-line wrapper components only to satisfy a line limit.

### Subcomponents

Never declare a React component inside another component's function body. React
receives a new component type on every parent render, which can cause remounting,
lost state and repeated effects.

Small, single-use subcomponents may be non-exported module-level functions in the
same file when they are tightly coupled to the parent and the file remains focused
and within the size limit.

Move a subcomponent to its own file when one or more of these apply:

- the containing file exceeds 200 lines
- it is reused outside the file
- it has its own state, effects or substantial behavior
- it represents a distinct domain/UI responsibility
- separating it creates a smaller Client Component boundary
- it needs focused tests or Storybook/examples of its own

### Rendering and Business Logic

Components may derive trivial display values, but they must not own duplicated
business rules. Pricing, discounts, totals, eligibility and similar domain rules
belong in an authoritative domain module and should be unit tested there.

Before writing a new helper, search for an existing implementation and inspect its
semantics. Reuse it when the rule is the same. If the existing behavior is wrong or
incomplete, update the owning implementation and its tests instead of creating a
component-specific variant.

Do not extract a helper merely to wrap a trivial one-time expression. Extract it
when it represents a business rule, is reused, needs independent tests or
materially improves readability.

## Props

Prefer explicit, meaningful props.

Avoid passing large objects when only a few fields are needed.

## Forms

Use the established form solution consistently.

A typical structure:

```text
Form UI
 ↓
validation
 ↓
submission
 ↓
server/API operation
 ↓
query invalidation/update
```

Validate at the appropriate server boundary even when client validation exists.

## Accessibility

Interactive UI must be keyboard accessible.

Use semantic HTML where possible.

Buttons should be buttons.
Links should be links.

Provide accessible names for icon-only controls.

Do not use color as the only way to communicate state.

## Loading States

Use appropriate loading boundaries.

Avoid showing a global loading spinner for a small local operation when a local pending state is clearer.

## Empty States

Design explicit empty states for:
- lists
- search
- carts
- user-owned collections

## Error States

User-facing errors should be understandable.

Do not expose raw backend errors directly unless they are intentionally safe and user-facing.

## Responsive Design

Design mobile-first when the product requires it.

Avoid JavaScript-based viewport detection when CSS responsive behavior can solve the problem.

## Performance

Prefer:
- Server Components
- streaming where useful
- small client bundles
- optimized images
- stable caching strategy
- parallel data fetching where safe
- lazy loading for non-critical UI

Avoid:
- unnecessary client providers
- large dependencies for tiny features
- duplicate data fetching
- unnecessary serialization across server/client boundaries
