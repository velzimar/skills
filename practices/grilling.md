# Grilling

Before a phase or an idea is specced, it is **grilled**: questioned one decision at a time until nothing in its scope is left open, with `/grill-with-docs` from the mattpocock-skills plugin (`/grilling` for a plan with no docs to check).

- Grilling reads the glossary, the ADRs and the coding standards, and challenges anything that contradicts them.
- Each answer is written down as it is settled: decisions in the phase's file, new terms in `CONTEXT.md`, decisions worth their reasons in `docs/adr/`.
- A question nobody can answer yet goes in the phase's open questions, never silently decided by the builder.

**Why:** a decision made while coding is made alone, late and invisibly; grilling makes it early, with the person who owns the product, and leaves a record the tickets and reviews can check against.
