---
name: code-review
description: "Review changed Laravel, Livewire, and PHP code for correctness, security, maintainability, Eloquent issues, and regressions. Use after meaningful PHP changes. Relies on Laravel Boost for framework conventions and Larastan, Pest, and Pint for automated evidence."
license: MIT
metadata:
  origin: "Adapted from ECC php-reviewer"
---

# Laravel / Livewire Code Review

Review the change, not the entire application unless explicitly requested.

## Source of Truth

- Laravel Boost guidance and the project's existing conventions come first.
- Do not enforce stylistic preferences such as `final`, `readonly`, `declare(strict_types=1)`, DTOs, Actions, Services, or Repositories unless the project already uses them or they solve a concrete problem.
- Use Larastan for static analysis, Pest for behavioral proof, and Pint for formatting.

## Review Workflow

1. Inspect `git diff` and the surrounding code.
2. Identify behavior changes and affected boundaries.
3. Review the changed code against the relevant Laravel Boost skill/rules.
4. Run the narrowest practical automated checks.
5. Report only actionable findings with evidence.

## Review Priorities

### Critical

- Authorization bypass or missing authorization on protected actions.
- SQL injection, command injection, unsafe deserialization, path traversal, exposed secrets.
- Unsafe file uploads.
- Financial or state-changing operations that can duplicate, race, or partially commit.
- Data corruption or irreversible migration risk.

### High

- Missing input validation.
- Mass-assignment mistakes with request-derived input.
- N+1 queries or unbounded dataset loading in changed paths.
- Incorrect Eloquent relationship usage or query assumptions.
- Livewire actions that trust client-controlled identifiers or properties without server-side authorization/validation.
- Queue jobs that are unsafe to retry when retries are possible.
- Exceptions swallowed or converted to misleading success states.

### Medium

- Missing return/parameter types where project conventions expect them.
- Duplicate or unnecessarily complex logic.
- Queries in views.
- Debug statements (`dd`, `dump`, `var_dump`) left in changed code.
- Missing tests for changed branches or failure modes.

## Automated Checks

Prefer focused checks during development and the full verification gate at completion.

```bash
vendor/bin/pint --test
vendor/bin/phpstan analyse --memory-limit=2G
php artisan test
```

If the project defines equivalent Composer scripts, prefer those.

## Output Format

For each finding:

```text
[severity] Title
File: path:line
Problem: concise explanation
Impact: what can go wrong
Fix: smallest appropriate correction
Evidence: test, Larastan error, query pattern, or code path
```

If no material issues are found, state which checks were actually performed. Never claim a command passed unless it was run successfully.
