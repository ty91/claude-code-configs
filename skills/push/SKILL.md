---
allowed-tools: Bash(git push:*), AskUserQuestion
description: Push commits to remote repository
---

## Context

- Current branch: !`git branch --show-current`
- Commits to push: !`git log --oneline @{upstream}..HEAD 2>/dev/null || git log --oneline -5`

## Your task

**IMPORTANT:** If the current branch is `main` or `master`, ask the user for confirmation before proceeding. Warn them about the risks of pushing directly to the main branch.

- Push the commits to the remote repository.
