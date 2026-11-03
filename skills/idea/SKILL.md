---
name: idea
description: Append an idea raised for later to the project's ideas.md, creating the file if it doesn't exist. Use when the user types /idea or asks to note an idea for later.
argument-hint: <the idea>
---

Add this idea to the project's ideas file: $ARGUMENTS

If no idea was given, ask for it and stop.

## 1. Find the file

Use the first that exists: `docs/ideas.md`, then `ideas.md` at the repo root. If neither exists, create `docs/ideas.md` when a `docs/` folder exists, otherwise `ideas.md` at the root, starting with:

```markdown
# Ideas

Ideas raised for later, not yet decided. Each one is discussed before it joins the roadmap.
```

Read the whole file before writing: its intro may set rules for what goes in, and its existing entries set the format to copy.

## 2. Check for overlap

Before writing, look for a roadmap or plan doc (e.g. `docs/plan.md`, a `docs/phases/` folder, `ROADMAP.md`) and compare the idea against its phases, features and open questions, and against the ideas already in the file.

- If a phase or feature already covers the idea, wholly or in part, tell the user which one and which part, and leave the covered part out of the ideas file. A gap the phase leaves open belongs in the plan's open questions, tagged with that phase; ask before editing the plan.
- If an existing idea is the same or close, add to that entry instead of making a new one.

## 3. Write the entry

Append at the end of the file, matching the existing entries' format. With no entries to copy, use:

```markdown
## <Short title>

<One or two sentences: what the idea is.>

- <Where it touches the project's glossary, ADRs or existing code, if any.>
- Open: <questions that need deciding before it can be planned.>
```

Use the project's domain terms if it has a glossary (e.g. `CONTEXT.md`). Keep it to the idea and its open questions; don't design the solution.

## 4. Report

Tell the user in one or two lines what was added and where, and any overlap found.
