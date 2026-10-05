# Project Structure

## Recommended Structure

```text
src/
├── app/
│   ├── (routes)/
│   ├── api/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── loading.tsx
│   ├── error.tsx
│   └── not-found.tsx
│
├── features/
│   ├── authentication/
│   │   ├── ui/
│   │   ├── hooks/
│   │   ├── model/
│   │   └── index.ts
│   ├── add-to-cart/
│   └── checkout/
│
├── entities/
│   ├── product/
│   │   ├── ui/
│   │   ├── model/
│   │   └── index.ts
│   ├── cart/
│   ├── user/
│   └── order/
│
└── shared/
    ├── api/
    │   ├── generated/
    │   ├── client/
    │   └── query/
    ├── ui/
    ├── lib/
    ├── config/
    ├── hooks/
    └── types/
```

## Route Organization

Routes should remain thin.

Example:

```text
app/products/[slug]/page.tsx
```

The route should:
- receive route parameters
- load/compose required data
- set metadata
- render feature/entity components

It should not contain large business implementations.

## Feature Structure

A feature can use:

```text
feature-name/
├── ui/
├── hooks/
├── model/
├── lib/
└── index.ts
```

Only create directories that are actually needed.

Do not create empty architectural ceremony.

## Entity Structure

```text
product/
├── ui/
├── model/
└── index.ts
```

Entity UI should describe the domain object, not a business workflow.

Example:

Good:
`ProductCard`

Potentially wrong:
`ProductCheckoutCard`

The latter may belong to a feature because checkout behavior is a business capability.

## Shared Structure

Keep shared code generic.

Good:

```text
shared/ui/Button
shared/lib/formatCurrency
shared/api/client
```

Bad:

```text
shared/ui/ProductCheckoutButton
shared/lib/calculateWishlistDiscount
```

If it knows about a business domain, it probably does not belong in shared.

## Helper and Calculation Ownership

Do not use `utils`, `helpers` or `common` as dumping grounds.

Before creating a helper:

1. Search for the same behavior and inspect its consumers.
2. Reuse the existing implementation when its semantics match.
3. Place new behavior beside the domain or feature that owns the rule.
4. Move behavior to `shared` only when it is genuinely domain-agnostic.

Examples:

```text
entities/product/model/pricing.ts       # product price/discount rules
features/checkout/lib/order-total.ts    # checkout-specific calculation
shared/lib/formatCurrency.ts            # generic presentation formatting
```

A component should not define its own price or discount algorithm when the same
domain rule is needed elsewhere. Keep one authoritative calculation and let each
component render its result.

## File Naming

Use the repository's existing naming convention consistently.

Prefer clear names:

```text
ProductCard.tsx
useProduct.ts
product.types.ts
queryKeys.ts
```

Avoid meaningless names:

```text
helper.ts
common.ts
utils2.ts
manager.ts
stuff.ts
```

## Barrel Files

Use `index.ts` only when it improves public boundaries.

Avoid creating deep barrel chains that obscure ownership or create circular dependencies.

## Route-Level Files

Use Next.js conventions where applicable:

```text
page.tsx
layout.tsx
loading.tsx
error.tsx
not-found.tsx
template.tsx
default.tsx
```

Keep these files focused on route concerns.
