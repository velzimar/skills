# Zod contracts

Request and response shapes are Zod schemas in `packages/contracts`, shared by the API and the web app, and written before either side.

## Rule

- Every request body, query and response is a Zod schema in `packages/contracts`, one folder per context, with its TypeScript type inferred from it. `packages/contracts` depends only on `zod`.
- The API validates incoming bodies and queries with those schemas (one validation pipe), and the web app parses responses with them. No second copy of a shape: no per-module DTO classes.
- A new endpoint starts with its contract; the API and the web app are then built against it.
- A check a schema cannot express (one needing the domain, or the database) belongs in the service, and fails with the same validation error the pipe would.

## Why

One schema is both the type and the runtime check, on both sides, so the API and the web app cannot drift: a change to a shape breaks the build wherever it is used. Writing it first fixes the interface before either side's details pull it their way. Decorator-based DTOs (`class-validator`) would be a second copy of every shape; keeping the package to `zod` alone keeps it usable from any runtime.
