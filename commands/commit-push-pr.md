---
allowed-tools: Bash(git add:*), Bash(git rm:*), Bash(git status:*), Bash(git commit:*), Bash(git push:*), Bash(gh pr create:*), AskUserQuestion
description: Create git commits, push to remote, and create a GitHub PR
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Recent commits: !`git log --oneline -10`
- Commits to push: !`git log --oneline @{upstream}..HEAD 2>/dev/null || echo "No upstream set"`

## Your task

Perform the following steps in order:

### 1. Commit

- Based on the above changes, create git commits. ALWAYS write semantic commit messages in English (eg. `feat: add user authentication with JWT tokens`)
- If the changes contain multiple unrelated modifications, split them into separate commits for better clarity and history tracking.

### 2. Push

**IMPORTANT:** If the current branch is `main` or `master`, ask the user for confirmation before proceeding. Warn them about the risks of pushing directly to the main branch.

- Push the commits to the remote repository.

### 3. Create PR

- Create a GitHub pull request using `gh` cli. Set the assignee to `@me`.
- A pull request should have exactly three sections: Summary, Changes, and Test plan. All contents except the PR title and section headers should be written in Korean.
