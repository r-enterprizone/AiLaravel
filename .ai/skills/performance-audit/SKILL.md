---
name: performance-audit
description: "Audit Laravel and Livewire changes for measurable performance risks: Eloquent query count, large datasets, indexes, Livewire payload/query behavior, caching, queues, and external I/O. Use for slow flows, data-heavy screens, reports, imports, dashboards, or performance-sensitive changes."
license: MIT
metadata:
  origin: "Adapted from ECC performance-optimizer"
---

# Laravel / Livewire Performance Audit

Optimize only when there is evidence or a clear scaling risk. Laravel Boost performance and Eloquent guidance remains authoritative.

## Audit Order

1. Database query count and query shape.
2. Amount of data loaded into memory.
3. Livewire request frequency and payload/query behavior.
4. External HTTP and filesystem I/O.
5. Cache opportunities and invalidation cost.
6. Queue suitability for slow, retryable work.

## Database Checks

- N+1 relationship access in loops, resources, Livewire rendering, exports, and jobs.
- Loading full models/relationships when only counts or selected columns are needed.
- Unbounded `get()` / `all()` on potentially large tables.
- Missing pagination, `chunkById`, `lazy`, or cursor-style processing where appropriate.
- Repeated aggregate queries that can use `withCount`, `withSum`, subqueries, or a single grouped query.
- Indexes evaluated against actual frequent query patterns, not added mechanically.
- Stable ordering for pagination.

When Boost database tools are available, use schema inspection and read-only queries to verify assumptions before recommending schema changes.

## Livewire Checks

- Expensive database work repeated on every render/request.
- Large public properties or collections serialized between requests.
- Search/filter inputs generating excessive requests when debounce/lazy behavior would be more appropriate for the installed Livewire version.
- Recomputing expensive derived values that can be calculated once or cached appropriately.
- Large lists rendered without pagination or incremental loading.

Use Boost's version-specific Livewire guidance for exact APIs.

## I/O and Background Work

- Independent external calls are not made sequentially without reason.
- Outbound HTTP has appropriate timeout/retry behavior.
- Slow, durable, retryable work is queued when appropriate.
- Lightweight post-response work may use the Laravel mechanism recommended for the installed version.

## Caching

Recommend caching only when:

- the read is materially expensive or frequent;
- invalidation semantics are clear;
- stale data behavior is acceptable.

Do not add caching as a substitute for fixing an obviously inefficient query.

## Report Format

For each issue include:

- Location
- Observed/risk pattern
- Why it matters
- Evidence or expected scaling behavior
- Lowest-complexity fix
- How to verify the improvement

Avoid arbitrary performance scores and claims without measurement.
