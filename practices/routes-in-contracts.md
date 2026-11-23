# Routes in the contracts

Each API route is a constant in `packages/contracts`, beside its schemas, and both the API and the web app bind to it. Builds on [Zod contracts](zod-contracts.md).

## Rule

- A route is a plain object made with `route()`: `{ method, path: '/api/things/:id', status, body?, query?, response }`, the last three Zod schemas (`z.undefined()` for no body). Each context has a routes object; one `routes` export gathers them.
- The API serves a route only through its own `@Route(routes.x)` decorator, which applies the framework's method decorator with the full path, sets the status and validates the body and query with the route's schemas. Controllers have no path prefix. A lint rule (`no-restricted-imports`) bans the framework's own method decorators in controllers, and an integration test checks that the served routes and the contracts' routes are the same set, both ways.
- The web app calls through `api.call(routes.x, { params, query, body, signal })`, which fills the path, serialises the query, sends the body and parses the answer with the route's response schema. Path params are typed from the template (`:id` gives `{ id: string }`) and stay strings the service checks.
- Controllers are thin: let `@Route()` validate, call one service method, present the result. A check a schema cannot express (one needing the domain, or the database) belongs in the service, and fails with the same validation error the pipe would.

## Why

A path and its schemas live in one place, so a call cannot pair a path with the wrong schema, a route changed on the API is followed by the web app without an edit, and a missing param fails to type-check. The lint rule and the set test catch a route that skips the contract. A library such as ts-rest does the same, but would make the contracts depend on more than `zod`.
