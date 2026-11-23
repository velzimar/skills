# Practices

How velzimar's projects are run and built, one practice per file, each with the reason behind it. Written for agents to follow and for people to read. `/velzimar:setup-project` scaffolds a new repo from them.

## Dependency

The process practices run on the [mattpocock-skills](https://github.com/mattpocock/skills) plugin: `/grill-with-docs`, `/to-spec`, `/to-tickets`, `/implement`, `/grilling` and `/domain-modeling` come from it, and are named here, never copied. Install it beside this plugin:

```
/plugin install mattpocock-skills@claude-plugins-official
```

## Process

Every project scaffolded by `/velzimar:setup-project` follows all of these.

| Practice                    | In short                                                                           |
| --------------------------- | ---------------------------------------------------------------------------------- |
| [Phase flow](phase-flow.md) | Work ships in phases, each with its own file in `docs/phases/` and its own branch. |
| [Ideas](ideas.md)           | Ideas for later go to `docs/ideas.md` through `/velzimar:idea`, ungrilled.         |
| [Grilling](grilling.md)     | Every phase and idea is grilled before it is specced.                              |
| [ADRs](adrs.md)             | Settled decisions and their reasons live in `docs/adr/`.                           |
| [Glossary](glossary.md)     | `CONTEXT.md` names the domain; code, tests and tickets use its terms.              |
| [Ship docs](ship-docs.md)   | Docs-only changes land on the default branch with CI skipped.                      |

## Code

Chosen one by one: `/velzimar:setup-project` writes the chosen ones' rules into the new repo's `CODING_STANDARDS.md`. Each file's **Rule** section is the text that goes there.

| Practice                                          | In short                                                                                  |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| [Folder structure](folder-structure.md)           | A monorepo of `apps/` and `packages/`; `src/core/` and one module per context.            |
| [Zod contracts](zod-contracts.md)                 | Request and response shapes are Zod schemas in one shared package, written first.         |
| [Routes in the contracts](routes-in-contracts.md) | Each route's method, path and schemas are a constant both sides bind to.                  |
| [Tests](tests.md)                                 | Integration tests through HTTP only, plus browser end-to-end tests for the critical flow. |
| [Client ids](client-ids.md)                       | The client makes UUIDv7 ids; rows soft-delete; the schema is ready for sync.              |
| [Temporal](temporal.md)                           | Time is `Temporal` everywhere, behind an injected clock.                                  |
| [One error body](error-body.md)                   | Every error answers `{ error: { code, message } }` with a stable code.                    |
| [Timeout and retry](retry.md)                     | Every call times out; only failures worth repeating, on requests safe to repeat, retry.   |
