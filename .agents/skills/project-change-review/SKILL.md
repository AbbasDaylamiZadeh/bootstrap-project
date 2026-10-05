---
name: project-change-review
description: Review a code diff in this repository for missed requirements, duplicate domain rules, wrong architectural ownership, and unsupported verification claims. Use for review requests or a final review of a substantial implementation.
---

# Project change review

Read the requested diff, `AGENTS.md`, relevant `docs/`, and any applicable
OpenSpec change. Identify pre-existing working-tree changes before attributing
code to the current task.

Prioritize findings that affect behavior or maintainability:

1. Missing or changed acceptance behavior, including loading, empty, error, and
   success states where relevant.
2. Duplicated business rules or a new helper/component that should reuse an
   existing owner.
3. Imports against `app → features → entities → shared`, oversized client
   boundaries, and unnecessary abstractions.
4. Unsafe types, accessibility failures, or missing behavior-focused tests.
5. Claims that rely on an uninstalled dependency or an unrun quality check.

Verify each finding against actual code and cite the file and line. Distinguish
observed defects from optional improvements. If there are no actionable
findings, say so and state what verification was performed and what remains
unverified. Do not edit code during a review-only request.
