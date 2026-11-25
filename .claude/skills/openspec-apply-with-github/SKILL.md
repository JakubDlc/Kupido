---
name: openspec-apply-with-github
description: Updates the Github Issue and execute opsx:apply
---

# OpenSpec Apply with GitHub

Use this skill to apply the current OpenSpec change to the local repository and synchronize with GitHub Issues and the configured GitHub Project.
Always use this Github project: `Kupido`

## Workflow

1. Make sure you are on the issue branch (`feature/<issue-number>-...`), never on `main`.
2. Update the corresponding GitHub Issues for the current OpenSpec change. Set status to "In Progress" and add a comment with the OpenSpec change reference.
3. Execute `opsx:apply` to apply the OpenSpec change to the local repository. Write tests for every scenario and mark them with the REQ-ID (`[Trait("Req", "REQ-...")]`).
4. Commit with the issue number (e.g. `feat(inserate): create listing (#12)`), push and open a pull request with `Closes #<issue-number>`.
5. Do not set the status to "Done" or archive the OpenSpec change.

## GitHub tools

Prefer the GitHub CLI `gh`.

Check existing issues:

```bash
gh issue list --limit 100
```

Comment and open the pull request:

```bash
gh issue comment <issue-number> --body "OpenSpec change: openspec/changes/<change>/"
gh pr create --base main --title "<issue title>" --body "Closes #<issue-number>"
```
