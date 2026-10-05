# Testing

## Testing Philosophy

Test behavior and contracts, not implementation details.

A useful test suite has multiple levels:

```text
unit
 ↓
component/integration
 ↓
E2E
```

## Unit Tests

Use for:
- pure functions
- domain calculations
- validation
- transformations
- business rules

## Component Tests

Test meaningful user behavior:
- clicking
- submitting
- loading
- error
- empty
- success states

Avoid asserting internal implementation details.

## Integration Tests

Use when multiple pieces must work together:
- feature + API abstraction
- form + validation + mutation
- state synchronization

## E2E

Use for critical user journeys:
- authentication
- checkout
- purchase
- important account flows

## Test Naming

Tests should describe behavior:

Good:

```text
adds a product to the cart when the user clicks Add to Cart
```

Bad:

```text
calls handleClick
```

## Mocking

Mock at the appropriate boundary.

Avoid excessive mocking that causes tests to validate mocks instead of application behavior.

## Regression Tests

Every bug fix should consider a regression test.

## Test Quality

A passing test suite is not enough.

Ask:
- Does the test fail when the bug returns?
- Does it represent user/business behavior?
- Is it stable?
