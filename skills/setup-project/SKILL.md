---
name: setup-project
description: Scaffold a new repo's docs (CLAUDE.md, CONTEXT.md, CODING_STANDARDS.md, docs/adr, docs/phases, docs/plan.md, docs/ideas.md) from velzimar's practices, writing the chosen code practices into CODING_STANDARDS.md.
argument-hint: "[project name]"
disable-model-invocation: true
---

# Set up a project

Scaffold the docs a project run on velzimar's practices starts from. The practices are the `practices/` folder at this plugin's root, two levels above this skill's folder (`../../practices/` from this file); its `README.md` is the index. The file contents to write are in [templates.md](templates.md).

## 1. Check the repo

Run in the repo's root. List which of these exist: `CLAUDE.md`, `CONTEXT.md`, `CODING_STANDARDS.md`, `docs/adr/`, `docs/phases/`, `docs/plan.md`, `docs/ideas.md`. If any does, name them and ask whether to skip them or stop; never overwrite one. Done when every path to write is free or approved to skip.

## 2. Ask

Ask, in one message:

- The project's name (default: the argument, else the folder's name) and one sentence on what it is.
- Which code practices to adopt: list each row of the index's **Code** table with its one-line summary, numbered, and take a list of numbers, `all` or `none`. Say that Routes in the contracts needs Zod contracts, and Timeout and retry needs One error body, Client ids and Routes in the contracts; when a chosen practice needs an unchosen one, ask whether to add it.

The process practices are not asked about: every project follows them.

Done when you have a name, a sentence and a final list of code practices.

## 3. Write

Write each file in [templates.md](templates.md), filling its placeholders. For `CODING_STANDARDS.md`, add one section per chosen code practice, in the index's order: `## <practice title>`, then that practice file's **Rule** section copied as it is, then its **Why** section as a paragraph starting `**Why:**`. Adjust a rule's paths only where the user's answers say the layout differs.

Done when every file is written and `CODING_STANDARDS.md` has exactly the chosen practices: no section for one that wasn't chosen.

## 4. Check the dependency

The phase flow runs on the mattpocock-skills plugin. If `/grill-with-docs` or `/mattpocock-skills:setup-matt-pocock-skills` isn't among your skills, tell the user to install it with `/plugin install mattpocock-skills@claude-plugins-official`. Either way, tell them to run `/mattpocock-skills:setup-matt-pocock-skills` next, which writes `docs/agents/` (issue tracker, triage labels, domain docs) and adds an **Agent skills** section to `CLAUDE.md`; it should record `CONTEXT.md` as the glossary.

## 5. Report

List the files written and the code practices adopted, and the next step: grill the first phase with `/grill-with-docs`.
