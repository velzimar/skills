# One error body

Every API error answers the same shape, with a stable code the client can switch on.

## Rule

- Every error body is `{ error: { code, message } }`, with `code` a stable snake_case string listed in `packages/contracts`, plus only the extra fields the schema allows (such as `issues` for validation, `retryAfterSeconds` for rate limits).
- Errors from every context extend one `DomainError` in `packages/domain`, which carries its code, a message and those extra fields, and knows nothing about HTTP. One exception filter maps codes to statuses in a single table in the API; anything else answers `internal_error`.
- An error that only stops the API from booting (a bad environment, invalid seed data) extends `Error`, never reaches HTTP, and is printed by `main.ts` before it exits.

## Why

The client handles every failure with one parser and switches on `code`, never on a message or a status that several errors share. Codes in the contracts make the list of failures part of the interface, so the web app knows each one at compile time. Keeping HTTP out of the domain lets the same rules run in the browser, and one status table keeps the mapping reviewable in one place.
