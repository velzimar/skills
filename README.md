# velzimar's skills

Agent skills for Claude Code, packaged as one plugin, and the practices they set up.

## Install

```
/plugin marketplace add velzimar/skills
/plugin install velzimar@velzimar
```

The skills then appear as `/velzimar:<skill>`. The phase flow the practices describe also needs the [mattpocock-skills](https://github.com/mattpocock/skills) plugin:

```
/plugin install mattpocock-skills@claude-plugins-official
```

## Skills

| Skill                     | What it does                                                                                                                                                                                                                                 |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/velzimar:setup-project` | Scaffolds a new repo's `CLAUDE.md`, `CONTEXT.md`, `CODING_STANDARDS.md`, `docs/adr/`, `docs/phases/`, `docs/plan.md` and `docs/ideas.md` from the practices, asking which code practices to write into `CODING_STANDARDS.md`.                |
| `/velzimar:idea`          | Appends an idea raised for later to the project's `ideas.md` (creating it if needed), after checking the roadmap and the existing ideas for overlap. Claude can also reach it on its own when you ask to note an idea.                       |
| `/velzimar:ship-docs`     | Lands a docs-only change (Markdown, `docs/`, `.claude/`) on the default branch through a branch, a PR and a squash merge, all with `[skip ci]`, after running the formatter CI would. `--force` also ships comment-only edits in code files. |

`setup-project` and `ship-docs` are invoked by name; Claude never runs them on its own. `ship-docs` needs a GitHub repo, the `gh` CLI signed in, and GitHub Actions for CI.

## Practices

[`practices/`](practices/README.md) holds one file per practice, each with the reason behind it: the process (phases, ideas, grilling, ADRs, the glossary, shipping docs) and the code (folder structure, Zod contracts, routes in the contracts, tests, client ids, Temporal, one error body, timeout and retry). They are generalised from a working project and updated by hand when its practices change.

## Licence

MIT. See [LICENSE](LICENSE).
