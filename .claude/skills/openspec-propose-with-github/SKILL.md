---
name: openspec-propose-with-github
description: Executes opsx:propose and creates the corresponding GitHub Issue in the Kupido project.
---

# OpenSpec Propose with GitHub

Use this skill to create a new OpenSpec change and a matching GitHub Issue in the configured GitHub Project.
Always use this Github project: `Kupido`

## Workflow

1. Execute `opsx:propose` with the user's request to create the OpenSpec change.
2. Check if a GitHub Issue for this change already exists. If not, create one:
   - Title: `<REQ-IDs> <short title>` (e.g. `REQ-INS-01 Inserat erstellen`)
   - Body: summary of the proposal, the REQ-IDs, the OpenSpec change reference (`openspec/changes/<change>/`) and the tasks from `tasks.md` as a checklist
   - Assign it to the current user (`@me`) and add it to the project `Kupido`
3. Set the status of the issue in the project to "Todo".
4. Create a branch for the issue: `feature/<issue-number>-<short-name>` (use `gh issue develop`) and commit the OpenSpec change there.
5. Do not execute `opsx:apply`.

## GitHub tools

Prefer the GitHub CLI `gh`.

Check existing issues:

```bash
gh issue list --limit 100
```

Create the issue and link the branch:

```bash
gh issue create --title "<title>" --body-file <file> --assignee @me --project "Kupido"
gh issue develop <issue-number> --name feature/<issue-number>-<short-name> --checkout
```
