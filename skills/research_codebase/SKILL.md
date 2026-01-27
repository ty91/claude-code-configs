---
name: research_codebase
description: Research codebase and external resources for implementation planning. Use when the user asks to analyze, research, or understand how parts of the codebase work.
context: fork
agent: codebase-researcher
---

## Research Task

Research the following topic thoroughly:

$ARGUMENTS

## Context

- Working directory: !`pwd`
- Project structure overview: !`ls -la`
- Git branch name: !`git branch --show-current | tr '/' '-'`

## Deliverable

Save research findings to `.tasks/{git-branch-name}/findings/{topic-of-research}.md`.

- Use the git branch name from Context above (slashes already replaced with `-`)
- Use a concise, kebab-case topic name (e.g., `authentication-flow.md`, `api-error-handling.md`)
