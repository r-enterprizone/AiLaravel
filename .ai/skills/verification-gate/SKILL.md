---
name: verification-gate
description: "Run the final quality gate for Laravel/Livewire changes using Pint, Larastan, Pest, Composer audit, and conditional migration/deployment checks. Use before declaring a meaningful task complete, before merge, and before release."
license: MIT
metadata:
  origin: "Adapted from ECC laravel-verification"
---

# Laravel Quality Verification Gate

Use two levels: a normal task-completion gate and a broader release gate.

## Task Completion Gate

Run after meaningful code changes before declaring the task complete.

### 1. Formatting

```bash
vendor/bin/pint --test
```

If it fails, format the affected code with the project's normal Pint command, then rerun the check.

### 2. Larastan

```bash
vendor/bin/phpstan analyse --memory-limit=2G
```

Do not hide new errors with ignores or baseline entries unless the user explicitly accepts that tradeoff.

### 3. Pest / Laravel Tests

Run the relevant focused tests during implementation. At completion, run the broadest suite justified by the change:

```bash
php artisan test
```

For a large suite, project-defined groups or CI scripts may be used when they provide equivalent coverage.

### 4. Dependency Audit

Run when dependencies changed, for security-sensitive work, or as part of the project's normal completion policy:

```bash
composer audit
```

## Conditional Database Checks

If migrations changed:

```bash
php artisan migrate:status
php artisan migrate --pretend
```

Review destructive operations, reversibility, locking/runtime risk, and data backfill strategy. Never run destructive production migrations as part of verification.

## Release Gate

Before deployment or a release candidate, add the relevant environment checks:

```bash
composer validate
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan schedule:list
php artisan queue:failed
```

Use Horizon/queue checks only when those systems are installed and relevant.

Be aware that cache commands change local generated cache state. Use them intentionally and clear/rebuild according to the project's deployment process.

## Failure Handling

- Stop on a failing gate.
- Fix the root cause rather than suppressing it.
- Rerun the failed check and any downstream checks affected by the fix.
- Never claim success for a command that was not run.

## Final Report

Summarize:

```text
Pint: PASS/FAIL
Larastan: PASS/FAIL
Tests: PASS/FAIL — <scope>
Composer audit: PASS/FAIL/SKIPPED — <reason>
Migration checks: PASS/FAIL/NOT APPLICABLE
Release checks: PASS/FAIL/NOT RUN
```
