---
name: implementation-planning
description: "Plan complex Laravel or Livewire features and refactors before implementation. Use for multi-file changes, schema changes, integrations, workflows, or risky refactors. Laravel Boost and the existing codebase remain the source of truth for framework conventions and versions."
license: MIT
metadata:
  origin: "Adapted from ECC planner"
---

# Implementation Planning

Use this skill to produce an actionable implementation plan before making a complex change.

## Source of Truth

- Treat Laravel Boost guidance, installed package versions, and existing project conventions as authoritative.
- Inspect nearby code before proposing new structure.
- Do not create new base directories, dependencies, or architectural layers unless the task clearly requires them or the user approves them.
- For version-sensitive Laravel or Livewire APIs, use Boost documentation search rather than relying on memory.

## Planning Workflow

1. Define the requested behavior and acceptance criteria.
2. Inspect the current implementation and identify reusable patterns.
3. Identify affected files, database changes, routes, Livewire components, jobs, events, policies, tests, and integrations.
4. Identify data integrity, authorization, validation, concurrency, queue, webhook, and failure-path risks when relevant.
5. Break the work into the smallest independently verifiable steps.
6. Pair each behavior-changing step with the relevant Pest test or verification step.
7. Prefer extending existing patterns over introducing a second pattern.

## Plan Format

```markdown
# Implementation Plan: <feature>

## Goal
<short description>

## Existing Pattern
- Relevant files and conventions already used by the project.

## Changes
1. <specific change>
   - Files: <paths>
   - Behavior: <what changes>
   - Verification: <test/check>
   - Risk: <only when meaningful>

## Data / Migration Impact
- <only when applicable>

## Security / Authorization Impact
- <only when applicable>

## Test Strategy
- Focused feature tests
- Unit tests only for isolated logic
- Regression tests for bugs

## Completion Gate
- Pint
- Larastan
- Relevant Pest tests
```

## Planning Rules

- Do not estimate effort unless explicitly asked.
- Do not prescribe Service/Action/Repository layers automatically.
- Do not rewrite working architecture merely to match a preferred pattern.
- For bug fixes, identify a reproducible failing behavior before changing production code when practical.
- For migrations, describe rollback and data-loss risk.
- For external integrations, account for timeouts, retries, idempotency, and failure states where relevant.
