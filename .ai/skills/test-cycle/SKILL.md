---
name: test-cycle
description: "Use a focused Pest-backed development cycle for new Laravel/Livewire behavior, bug fixes, and risky refactors. Reproduce bugs when practical, test observable behavior, run the narrowest relevant tests first, and rely on Laravel Boost testing guidance for framework-specific syntax."
license: MIT
metadata:
  origin: "Adapted from ECC tdd-workflow"
---

# Focused Test Cycle

This skill complements Laravel Boost's testing skill; it does not replace it.

## Principles

- Test observable behavior and business contracts rather than implementation details.
- Prefer a Laravel feature test for framework-connected behavior.
- Use unit tests for isolated logic that does not need the framework.
- Use Pest when the project already uses Pest.
- Do not impose an arbitrary coverage percentage unless the project defines one.
- Do not add browser-test dependencies unless the user asks or the project already uses them.

## New Behavior

1. Identify the changed decisions, validation branches, authorization boundaries, and failure paths.
2. Add the smallest meaningful tests that prove those behaviors.
3. Implement the change.
4. Run the focused tests until green.
5. Run Larastan and Pint before completion.

## Bug Fixes

When practical:

1. Add a regression test that fails for the reported bug for the intended reason.
2. Apply the smallest fix.
3. Confirm the regression test now passes.
4. Run neighboring tests to detect collateral regressions.

If reproducing the bug in an automated test is impractical, state why and use the best available verification rather than creating a brittle test.

## Refactors

- Establish green tests before changing structure.
- Keep behavior unchanged.
- Run focused tests after each coherent batch.

## Commands

Use the project's established commands. Typical defaults are:

```bash
php artisan test path/to/relevant/test
vendor/bin/phpstan analyse --memory-limit=2G
vendor/bin/pint --test
```

Run the full suite at the final verification gate when the scope warrants it.

## Completion Evidence

Report the exact tests/checks run and their outcomes. Do not create separate evidence documentation unless requested by the user or required by the project.
