---
name: openspec-archive-with-github
description: Archives the current OpenSpec change and updates the corresponding GitHub Issue.
---

# OpenSpec Archive with GitHub

Use this skill to archive the current OpenSpec change and update the corresponding GitHub Issue.
Always use this Github project: `Kupido`

## Workflow

1. Check that the pull request for the issue is merged. If not, stop and tell the user.
2. Update the corresponding GitHub Issues for the current OpenSpec change. Set status to "Done" and add a comment with the OpenSpec change reference (if not done before).
3. Archive the current OpenSpec change. `opsx:archive`
4. Commit the archived change (`chore(spec): archive <change> (#<issue-number>)`) and push it.

## GitHub tools

Prefer the GitHub CLI `gh`.

Check existing issues:

```bash
gh issue list --limit 100
```

Check the pull request:

```bash
gh pr list --state merged --search "<issue-number>"
```
