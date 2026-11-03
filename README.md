# velzimar's skills

Agent skills for Claude Code, packaged as one plugin.

## Install

```
/plugin marketplace add velzimar/skills
/plugin install velzimar@velzimar
```

The skills then appear as `/velzimar:<skill>`.

## Skills

| Skill                   | What it does                                                                                                                                                                                                                                 |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/velzimar:idea`        | Appends an idea raised for later to the project's `ideas.md` (creating it if needed), after checking the roadmap and the existing ideas for overlap. Claude can also reach it on its own when you ask to note an idea.                       |
| `/velzimar:ship-docs`   | Lands a docs-only change (Markdown, `docs/`, `.claude/`) on the default branch through a branch, a PR and a squash merge, all with `[skip ci]`, after running the formatter CI would. `--force` also ships comment-only edits in code files. |

`ship-docs` needs a GitHub repo, the `gh` CLI signed in, and GitHub Actions for CI. You invoke it by name; Claude never runs it on its own.

## Licence

MIT. See [LICENSE](LICENSE).
