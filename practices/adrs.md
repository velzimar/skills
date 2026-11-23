# ADRs

A decision that is hard to reverse, or that someone would otherwise question later, is recorded as an **ADR** in `docs/adr/NNNN-<decision-in-kebab-case>.md`, numbered in order.

- Short: a title that states the decision, a paragraph saying what was chosen and why, then **Considered options** with why each was rejected, and the cost accepted.
- An ADR is never rewritten to say something else. A later decision that changes it adds an **Amended** section (or a new ADR that names the one it amends), so the history stays readable.
- Decisions in `docs/adr/` and `CODING_STANDARDS.md` are settled: build on them, and raise a conflict with the person who owns the project before working around one. Read the ADRs touching an area before changing it.
- `CODING_STANDARDS.md` holds the rules; the ADRs hold why. A rule needs an ADR only when its reason isn't obvious.

**Why:** without the reason, a later reader (human or agent) can't tell a deliberate choice from an accident, and either reopens it every time or follows it after it stopped making sense.
