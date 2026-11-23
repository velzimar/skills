# Timeout and retry

Every API call times out, and failures retry only when the failure is worth repeating and the request is safe to repeat. Built on [one error body](error-body.md), and on [client ids](client-ids.md) for safe creates.

## Rule

- The API client (`api.call`) abandons a request after 10 s, as a network failure, combined with a caller's own `signal` through `AbortSignal.any` rather than replaced by it.
- What retries is one rule, in `src/core/api/`, set as the data-fetching library's default (such as TanStack Query's `QueryClient` defaults) for every query and mutation: network failures (timeouts included), `internal_error` and `rate_limited`, which waits its `retryAfterSeconds` instead of the backoff. Never any other error, and never a caller's abort.
- Queries keep the library's default retries. Mutations get 3 retries after the first try, 1 s, 2 s and 4 s apart; meanwhile the button stays pending, and after the last the screen's error message shows with **Try again**.
- A mutation retries only once it is safe to repeat: a create with a client id, a replace, a delete, or a change that answers the same when sent twice. A retry whose answer shows the first try worked though its answer was lost, such as not found after a delete, counts as success.
- A mutation that isn't safe to repeat sets `retry: false` with a comment saying why, such as one that sends an email or consumes a single-use code.

## Why

A phone on a weak network drops requests often; without a timeout a save hangs forever, and without retries a passing blip loses it. Retrying only network, server and rate-limit failures leaves real refusals (validation, not found) to the user, since repeating them can't help. Retrying only safe requests means a retry never sends a second email or makes a second row, and counting a lost answer as success stops a retry from reporting a failure for work that was done. One default in one module covers every call, so it is tested once, end to end.
