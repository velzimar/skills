---
name: ship-docs
description: Ship a change that touches no code (documents, Markdown, agent config) to the default branch on a branch and a squash-merged PR, with CI skipped. --force also ships comment-only edits in code files.
argument-hint: '[--force]'
disable-model-invocation: true
---

# Ship docs

Lands a **docs-only** change on a GitHub repo's default branch without running CI: a branch, a commit, a PR, a squash merge, all carrying `[skip ci]`, which GitHub Actions honours on both `push` and `pull_request`. Code changes take the repo's normal PR flow, where CI runs; this skill exists only because CI has nothing to check in a docs-only change.

## Docs-only

A change is docs-only when every changed path, untracked ones included (`git status --porcelain --untracked-files=all`), is one of:

- a Markdown file (`*.md`) anywhere in the repo
- anything under `docs/` or `.claude/`, except `.claude/worktrees/`: those are other branches' checkouts, never shipped
- `.gitignore` or `LICENSE`
- a path the repo's CI already ignores: a `paths-ignore` pattern on every workflow in `.github/workflows/`

Anything else is code, even a comment-only edit in a source file, a stylesheet, a data file, a workflow, `package.json` or a config file: CI checks those.

## --force

With `--force`, a code path ships too when its diff (`git diff -- <path>`) changes only comments: every added or removed line is inside a comment, or is blank. A changed line of code, an import, a selector, a value or a string still fails, `--force` or not, and so does a new or deleted code file.

## Steps

1. **Check the scope.** List every changed path and test it against docs-only, and under `--force` test each code path's diff for comment-only. If any path fails, stop: name those paths and ask whether to ship the passing paths alone and leave the rest uncommitted, or to send everything through the normal PR flow. Done when every path to ship passes.
2. **Check the base.** Find the default branch (`gh repo view --json defaultBranchRef --jq .defaultBranchRef.name`). Be on it and level with its remote: `git fetch`, then `git pull --ff-only` if behind. On another branch, stop and ask. Done when the local default branch has every commit its remote has.
3. **Format.** CI won't run, so run the formatter its checks use on the shipped paths yourself, such as `pnpm exec prettier --check --ignore-unknown <paths>` for a Prettier repo. If it reports files, run it with `--write` and include those fixes. Done when the check passes, or the repo checks no formatting.
4. **Branch.** `git switch -c <type>/<description>`:
   - `<type>`: `docs` for documents (Markdown, `docs/`, agent instructions, skills); `chore` for repo housekeeping (`.gitignore`, `.claude/` settings).
   - `<description>`: two to five lowercase kebab-case words saying what changed, such as `docs/phases-folder` or `docs/install-steps`.
5. **Commit.** Stage the shipped paths by name, never `git add -A`. The subject reads like the repo's history (`git log --oneline -10`), ending in ` [skip ci]`. Add a short body when the subject doesn't say why, then any commit attribution trailer your instructions give.
6. **Open the PR.** `git push -u origin <branch>`, then `gh pr create --base <default branch> --title "<commit subject>"`, with a body of one or two sentences on what changed and any PR attribution line your instructions give. The title keeps `[skip ci]`, so the squash commit carries it too.
7. **Merge.** `gh pr merge <number> --squash --delete-branch`, or the merge method the repo's history uses when it refuses squash, then `git pull --ff-only` on the default branch.
8. **Confirm CI was skipped.** `gh run list --limit 3`: no run for the PR's branch or the merge commit. If one started, say so; it wastes minutes but changes nothing.

Report the PR's URL and the merge commit on the default branch.
