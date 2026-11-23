# Folder structure

A web app and its API in one monorepo (pnpm workspaces), each app split into cross-cutting core and one module per bounded context.

## Rule

```
apps/api            the API: src/core/, src/modules/<context>/, test/
apps/web            the web app: src/routes/ (thin), src/core/, src/modules/<context>/
packages/domain     business rules, value objects, repository interfaces, business-rule constants; no dependencies
packages/contracts  request/response schemas and routes, one folder per context; depends only on zod
packages/config     shared tsconfig and eslint presets
```

- `apps/` share code only through `packages/`. `domain` and `contracts` stay independent of each other; the apps translate between them.
- In each app, `src/core/` holds cross-cutting code with no business rules, one folder per job (config, database, time, http, errors; in the web app api, layout, ui), and never imports from `src/modules/`.
- `src/modules/` holds one folder per context, its files grouped by kind: in the API `controllers/`, `services/`, `repositories/`, `queries/` (read-only views), `adapters/` (code that talks to something outside the process); in the web app `screens/` (one per route), `queries/`, `components/`, `hooks/`. A folder exists only when the context has a file of that kind.
- Route files stay thin: a guard built from a module's loader, then a module's screen or a core layout.
- Each context's tables are read and written only by that context; contexts call each other through the services their module exports, and hold each other's ids without a cross-context foreign key.
- A business-rule number (a limit, lifetime, count the product decides) is a named constant in `packages/domain`, imported by the API, the web app and the tests.
- Prefer fewer, deeper files: add a file or layer only when it holds a rule.

## Why

The layout answers "where does this go?" the same way every time, so neither a person nor an agent has to search. `core` never importing `modules` keeps the dependency direction one-way, so a context can be read, changed or removed alone. Shared packages with tight dependency limits let the browser run the same rules and schemas as the API without dragging the API's stack along. Thin routes keep the framework's file-based routing a table of contents, not a place logic hides. Full DDD layering only where rules are many: elsewhere it makes files that only pass data through.
