# Phase flow

Work ships in **phases**: a small, named slice of the product, decided, specced, split into tickets and built, one after another.

## The flow

Each phase goes through the mattpocock-skills plugin's skills, in order (see [the dependency](README.md#dependency)):

1. `/grill-with-docs`: grill the phase against the glossary and ADRs until its scope and decisions are settled. Its decisions go in the phase's file.
2. `/to-spec`: turn the phase's file into a spec issue.
3. `/to-tickets`: split the spec into tickets, each a sub-issue with its blockers.
4. `/implement`: build one ticket per run.

## The files

- `docs/plan.md`: the product, its features and the **phase index**, a table of every phase with its status (Planned, Grilled, Specced, Built). It links each phase's file and repeats nothing from them.
- `docs/phases/phase-<major>.<minor>.md`, one per phase: its status line (with the spec and ticket numbers once they exist), scope, decisions and open questions. Always two numbers (`phase-2.0.md`, never `phase-2.md`), so a file explorer lists them in order. A small follow-up to a phase takes the next minor number.
- `CLAUDE.md`'s status line says which phase is built and which comes next.

Read the phase's file before grilling, speccing or building it.

**Why:** a phase small enough to grill in one sitting keeps decisions explicit and reviewable; one file per phase keeps its reasons next to its scope, so a later phase can find why something was built as it was, and the plan stays a short index instead of sediment.

## Branches

- A phase's tickets merge into a **phase branch**, `phase-<major>.<minor>`, created from the default branch when the phase's first ticket starts.
- A ticket's branch starts from the phase branch, and its pull request targets it.
- Once every ticket is closed, one pull request merges the phase branch into the default branch.
- Work outside a phase's tickets, such as a docs change, goes to the default branch.

**Why:** the default branch only ever holds whole phases, so it is always releasable; and the phase's pull request is one place to review the phase as a whole.
