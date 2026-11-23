# Ship docs

A change that touches no code (Markdown, `docs/`, agent instructions) lands on the default branch with `/velzimar:ship-docs`: a branch, a pull request and a squash merge, all carrying `[skip ci]`, after running the formatter CI would. Anything that touches code takes the normal pull request flow, where CI runs.

**Why:** docs change often (every grilling writes some) and CI has nothing to check in them, so running it wastes minutes; the pull request still leaves a reviewable record, and the formatter run keeps the default branch passing.
