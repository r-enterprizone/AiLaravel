---
name: safe-refactor
description: "Safely refactor Laravel, Livewire, and PHP code while preserving behavior. Use for dead-code cleanup, consolidation, method/class extraction, dependency cleanup, or structural simplification. Requires focused tests, Larastan, and Pint evidence."
license: MIT
metadata:
  origin: "Adapted from ECC refactor-cleaner"
---

# Safe Refactoring

Refactoring must preserve externally observable behavior unless the user explicitly requests a behavior change.

## Before Refactoring

1. Identify the behavior being preserved.
2. Read nearby tests and conventions.
3. Run or add focused tests that protect the behavior when coverage is missing.
4. Inspect dynamic Laravel references before declaring code unused.

## Laravel Dynamic Reference Checklist

Before deleting a class, method, route, config entry, listener, command, view, or component, check for references in:

- service container bindings;
- route definitions and route names;
- Blade and Livewire templates;
- events/listeners and discovery conventions;
- queued jobs;
- scheduled commands;
- notifications/mailables;
- model casts/observers/scopes;
- config files and class-string arrays;
- factories/seeders/tests;
- reflection or third-party package configuration.

A simple text search is evidence, not proof of non-use.

## Refactor Workflow

1. Make one coherent refactor batch.
2. Run the smallest relevant Pest tests.
3. Run Larastan on the project or relevant scope.
4. Run Pint.
5. Review the diff for accidental behavior changes.
6. Continue with the next batch only after the current batch is clean.

## Preferred Checks

```bash
vendor/bin/phpstan analyse --memory-limit=2G
vendor/bin/pint --test
php artisan test
```

Use project-defined Composer shortcuts when available.

## Refactor Rules

- Do not introduce a new architectural layer solely to make code look cleaner.
- Do not add dependencies for a refactor unless explicitly justified and approved.
- Prefer deleting duplication only when the shared abstraction has a stable meaning.
- Preserve public interfaces unless the requested refactor includes a migration plan.
- Do not mix a large refactor with unrelated feature work.
- When in doubt about dynamic usage, keep the code and flag it for manual confirmation.
