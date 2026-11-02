---
paths:
  - "app/**"
  - "routes/**"
  - "database/**"
  - "resources/views/**"
  - "tests/**"
  - "config/**"
  - "bootstrap/**"
---

# Quality Workflow

## Framework Source of Truth

Laravel Boost, installed package versions, existing project conventions, and version-aware Boost documentation are the source of truth for Laravel, Livewire, Pest, and framework APIs. Custom skills must complement Boost rather than override it.

## Architecture

Preserve the project's existing structure. Do not add architectural layers, base directories, or dependencies merely because a generic best-practice template uses them.

## Normal Change Cycle

For behavior-changing work:

1. inspect the existing pattern;
2. implement the smallest coherent change;
3. run focused Pest tests;
4. run Larastan;
5. run Pint;
6. use `verification-gate` before declaring a meaningful task complete.

Use `implementation-planning` first for complex multi-file work. Use `security-audit` for authentication, authorization, user input, uploads, payments, webhooks, secrets, sensitive data, or risky external integrations. Use `performance-audit` for data-heavy or slow flows. Use `silent-failure-review` when error handling, queues, webhooks, transactions, or external services change.

## Evidence

Do not claim that tests, Larastan, Pint, audits, migrations, or deployment checks passed unless the corresponding command actually ran successfully.
