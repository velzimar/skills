# Glossary

`CONTEXT.md` at the repo root is the domain glossary: each term the product uses, capitalised, with a short definition and the synonyms to avoid.

```markdown
**Term**:
What it means, in a sentence or two, and what it is not.
_Avoid_: synonym, other synonym
```

- Code, tests, tickets and user-facing text use its terms: a class, a test name and an issue title all say the same word.
- A new concept gets its term while it is grilled, through `/domain-modeling` from the mattpocock-skills plugin, before code names it.
- The mattpocock-skills plugin calls this file `GLOSSARY.md`; say in `CLAUDE.md` (or `docs/agents/domain.md`) that `CONTEXT.md` is that file.

**Why:** one name per concept removes the translation between what the product owner says, what the ticket says and what the code says; synonyms drift into different meanings, and an agent searching for one finds nothing under the other.
