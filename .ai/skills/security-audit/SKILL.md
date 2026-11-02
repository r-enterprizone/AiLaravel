---
name: security-audit
description: "Perform a focused security audit for Laravel and Livewire changes involving authentication, authorization, user input, uploads, payments, webhooks, external APIs, secrets, or sensitive data. Use Laravel Boost security guidance as the framework source of truth."
license: MIT
metadata:
  origin: "Adapted from ECC security-reviewer and security-review"
---

# Focused Security Audit

Activate this skill for security-sensitive changes rather than for every trivial edit.

## Source of Truth

- Read the relevant Laravel Boost security guidance first.
- Follow the application's existing authentication and authorization conventions.
- Use framework-native protections where possible.
- Do not add security packages unless needed and approved.

## Audit Areas

### Authentication and Authorization

- Authentication is required where intended.
- Authorization is checked for the specific resource/action, not merely the route.
- Livewire public properties and action parameters are treated as client-controlled input.
- Privileged actions cannot be reached through alternate routes or methods.

### Input and Database Safety

- Request and Livewire input is validated server-side.
- Request-derived arrays do not permit unintended mass assignment.
- Raw SQL uses bindings; user-controlled sort/column names use allow-lists.
- State-changing multi-step operations use transactions where atomicity matters.

### Output and Browser Safety

- Blade output is escaped unless sanitized HTML is intentionally rendered.
- CSRF protection remains enabled for normal web actions.
- Routes excluded from CSRF, such as webhooks, verify authenticity independently.

### Files

- Uploads validate content/type and size.
- Filenames and paths are not trusted directly.
- Untrusted files are not placed in executable locations.

### Secrets and Sensitive Data

- No secrets are hardcoded or logged.
- `.env` values are accessed through configuration in application code.
- Sensitive fields are hidden/encrypted when appropriate.
- Errors returned to users do not expose stack traces, credentials, tokens, or private payloads.

### Payments and Webhooks

When applicable:

- Verify signatures using the provider's official mechanism.
- Protect against duplicate/replayed events with idempotent handling.
- Do not trust client-supplied price, amount, status, or completion state.
- Guard concurrent balance/inventory/state transitions using the appropriate transaction or lock.
- Record enough provider identifiers to reconcile failures safely.

### External HTTP

When applicable:

- Set explicit timeouts.
- Treat remote payloads as untrusted input.
- Avoid SSRF by validating/allow-listing user-controlled destinations.
- Define retry behavior only for operations that are safe to retry.

### Dependencies

Run when dependencies changed or before release:

```bash
composer audit
```

Run JavaScript dependency auditing only when the frontend dependency set is relevant to the change.

## Security Tests

Add or confirm tests for applicable boundaries:

- unauthenticated access is rejected;
- unauthorized access is rejected;
- invalid input is rejected;
- dangerous or duplicate webhook/payment processing is prevented;
- file restrictions are enforced.

## Report

Prioritize findings as Critical / High / Medium / Low. Include the affected path, exploit/failure scenario, and the smallest secure fix. Do not produce speculative findings without a reachable code path or concrete misconfiguration.
