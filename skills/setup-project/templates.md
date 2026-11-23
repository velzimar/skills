# Templates

The files `/velzimar:setup-project` writes. `<Name>` is the project's name and `<Sentence>` its one-sentence description.

## `CLAUDE.md`

```markdown
# <Name>

<Sentence>

Status: nothing is built yet. The first phase is to be grilled with `/grill-with-docs`.

## Docs

- `CONTEXT.md`: domain glossary. Name code, tests and tickets with its terms.
- `CODING_STANDARDS.md`: the rules every change follows; code review checks against it.
- `docs/adr/`: why decisions were made. Read the ones touching an area before changing it.
- `docs/plan.md`: product, features and the phase index. Read it before planning, grilling or speccing a phase.
- `docs/phases/`: one file per phase, `phase-<major>.<minor>.md` (`phase-1.0.md`, never `phase-1.md`, so the explorer lists them in order), with its status, scope, decisions and open questions. Read the phase's file before grilling, speccing or building it.
- `docs/ideas.md`: ideas raised for later and not yet grilled. Add one with `/velzimar:idea`; grill one before it joins a phase.

Decisions recorded in `docs/adr/` and `CODING_STANDARDS.md` are settled: build on them, and raise a conflict with the user before working around one.

## Workflow

Each phase goes through the mattpocock-skills plugin: `/grill-with-docs` → `/to-spec` → `/to-tickets` → `/implement` per ticket. Docs-only changes ship with `/velzimar:ship-docs`.

## Branches

A phase's tickets merge into a phase branch, never straight into the default branch.

- The phase branch is named after the phase: `phase-<major>.<minor>`, such as `phase-1.0`. Create it from the default branch when the phase's first ticket starts, if it doesn't exist yet.
- A ticket's branch starts from the phase branch, and its pull request targets the phase branch.
- Once every ticket of the phase is closed, one pull request merges the phase branch into the default branch.
- Work that isn't one of a phase's tickets, such as a docs change, still goes to the default branch.
```

## `CONTEXT.md`

```markdown
# <Name>

<Sentence>

## Language

Each term the product uses, added as it is grilled, in this form:

**Term**:
What it means, and what it is not.
_Avoid_: synonyms not to use
```

## `CODING_STANDARDS.md`

```markdown
# Coding standards

Rules every change in this repo follows, each with its reason; decisions that need more say why in `docs/adr/`. The vocabulary is in `CONTEXT.md`.
```

Then one section per chosen code practice, as step 3 says. With none chosen, the file is this intro alone.

## `docs/plan.md`

```markdown
# <Name> plan

Product, features and the phase index. Each phase's scope, decisions and open questions are in its own file in `docs/phases/`. Settled decisions are in `docs/adr/` and `CODING_STANDARDS.md`; this file does not repeat them.

## Product

<Sentence>

## Features

To be decided while grilling the first phase.

## Phases

Each phase goes through the main flow: `/grill-with-docs` → `/to-spec` → `/to-tickets` → `/implement` per ticket. Phases are built in the order below.

| Phase | Status |
| ----- | ------ |
```

## `docs/ideas.md`

```markdown
# Ideas to grill

Ideas raised for later, not yet decided. Each one goes through `/grill-with-docs` before it joins a phase in `docs/phases/`. The notes under each are where it touches `CONTEXT.md` and `docs/adr/`, so the grilling starts from there.

An idea that an existing phase already covers, wholly or in part, doesn't go here: the covered part stays with that phase, and anything it leaves open becomes an open question in that phase's file in `docs/phases/`.
```

## `docs/adr/` and `docs/phases/`

Empty folders, each holding a `.gitkeep` so git keeps them. The first ADR and the first phase file are written while grilling.
