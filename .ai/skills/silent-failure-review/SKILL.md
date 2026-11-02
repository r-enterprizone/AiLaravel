---
name: silent-failure-review
description: "Hunt for swallowed exceptions, misleading fallbacks, missing failure propagation, unsafe retries, and success responses that hide failures in Laravel, Livewire, queues, webhooks, database transactions, and external integrations."
license: MIT
metadata:
  origin: "Adapted from ECC silent-failure-hunter"
---

# Silent Failure Review

The goal is not to log every exception. The goal is to ensure real failures are observable and do not masquerade as success.

## Hunt Targets

### Swallowed Exceptions

- Empty `catch` blocks.
- `catch (Throwable|Exception)` returning `null`, `false`, empty arrays, or success without a deliberate contract.
- `rescue()` defaults that hide operational failures.
- Generic rethrows that discard useful context.

### Misleading User Success

- Livewire actions displaying success after only part of a multi-step operation succeeded.
- Redirects or 2xx responses emitted before required persistence or integration work is confirmed.
- Webhooks acknowledged as successfully processed when the handler merely ignored an invalid/failed internal operation.

### Database Atomicity

- Multi-step state changes without a transaction when partial completion would be invalid.
- Exceptions caught inside a transaction in a way that prevents rollback unintentionally.

### Queues

- Jobs whose failure disappears without useful context.
- Retryable jobs that duplicate irreversible side effects.
- Missing handling for exhausted retries where the business process requires follow-up.

### External HTTP

- No timeout on external calls.
- HTTP error responses treated as valid payloads.
- Retries applied to non-idempotent operations without protection.
- Fallback data that looks real enough to conceal an outage.

### Logging

Useful failure logs should contain safe identifiers and operation context, but never secrets or sensitive payloads.

## Output

For each finding report:

- location;
- severity;
- failure that is being hidden;
- user/business impact;
- smallest fix;
- test that would prove the failure path is handled correctly.
