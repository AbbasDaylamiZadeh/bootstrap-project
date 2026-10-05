# Architecture

## Architectural Goal

The project is organized as a vertical, domain-oriented frontend architecture that keeps route composition, business capabilities, domain objects and shared infrastructure separate.

## Dependency Direction

```text
app
 ↓
features
 ↓
entities
 ↓
shared
```

Allowed dependencies:

| Layer | Can depend on |
|---|---|
| app | features, entities, shared |
| features | entities, shared |
| entities | shared |
| shared | shared only |

A lower layer must never import a higher layer.

## Layer Responsibilities

### app

Owns:
- routes
- layouts
- route-level metadata
- loading boundaries
- error boundaries
- not-found boundaries
- route composition
- route-specific server orchestration

The app layer should compose existing capabilities rather than implement reusable business logic.

### features

Owns business capabilities.

Examples:
- add-to-cart
- wishlist
- checkout
- product-search
- authentication
- review-submission

A feature should represent a meaningful user/business action rather than a generic UI primitive.

### entities

Owns domain concepts.

Examples:
- product
- cart
- user
- order
- category

An entity may contain:
- domain types
- domain-specific UI
- small domain helpers

Entities should not know how a larger business feature works.

### shared

Owns reusable infrastructure and generic primitives.

Examples:
- UI primitives
- API infrastructure
- HTTP client
- validation utilities
- date/number formatting
- configuration
- generic hooks

Shared code must remain domain-agnostic.

## Vertical Slices

Prefer grouping code by domain/capability rather than by technical type.

Prefer:

```text
features/
  add-to-cart/
    ui/
    hooks/
    model/
```

over:

```text
components/
hooks/
utils/
services/
```

for business functionality.

## Cross-Feature Dependencies

Avoid:

```text
features/cart → features/wishlist
```

If two features need common behavior, consider:
- moving a domain concept into `entities`
- extracting truly generic infrastructure into `shared`
- composing both features from `app`

Do not use `shared` as a dumping ground.

## Server/Client Architecture

Prefer:

```text
Server Component
  ↓
domain data / composition
  ↓
small Client Component
  ↓
interaction
```

Avoid:

```text
entire page → "use client"
```

unless the page genuinely requires client execution.

## Architectural Decision Test

Before adding a file, ask:

1. Who owns this behavior?
2. Which layer should own it?
3. Is there already an abstraction for it?
4. Does the dependency direction remain valid?
5. Will this create a cycle?
6. Can the same behavior be achieved with fewer abstractions?
