---
allowed-tools: Bash(gh pr create:*)
description: Create a github pr
---

## Context

- Current branch: !`git branch --show-current`
- Recent commits: !`git log --oneline -10`

## Your task

Based on the above changes, create a github pull request using `gh` cli. Set the assignee to `@me`.

A pull request should have exactly three sections: Summary, Changes, and Test plan. All contents except the PR title and section headers should be written in Korean.
