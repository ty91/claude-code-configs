---
allowed-tools: Read, Glob, Grep, Edit, Task, TodoWrite, Bash(git:*), Bash(ls:*), Bash(tree:*), AskUserQuestion
description: Review and refine implemented code using code-reviewer and code-simplifier agents
---

# Enhance Code

You orchestrate the code review and refinement pipeline after `implement-plan` completes. You coordinate two specialized agents to ensure implementation quality.

## Context

- Git branch: !`git branch --show-current`

Convert slashes to hyphens for the directory name (e.g., `feature/auth` → `feature-auth`). This determines the plan location: `.tasks/{branch-name}/plan.md`

## Pipeline Overview

```
implement-plan (completed) → enhance-code (you are here)
                                ├─ code-reviewer (identify issues)
                                ├─ fix critical issues
                                └─ code-simplifier (refine code)
```

## Initial Response

When this command is invoked, respond with:
```
I'll review and enhance the recently implemented code.

Let me check the implementation status and gather context.
```

Then proceed with Step 1.

## Process Steps

### Step 1: Verify Implementation Status

1. Read the plan file at `.tasks/{branch-name}/plan.md`
2. Identify completed phases (look for `[x]` checkmarks)
3. Run `git diff main...HEAD` to see all changes on this branch
4. If no completed phases found, inform the user and suggest running `/implement-plan` first

### Step 2: Code Review

1. **Spawn code-reviewer agent** with clear context:
   - Which phases were completed
   - What files were modified
   - Request structured review output

2. **Wait for review completion** and analyze results

3. **Categorize findings**:
   - **Critical**: Must fix before proceeding
   - **Warnings**: Should fix
   - **Simplification opportunities**: Pass to code-simplifier

### Step 3: Address Critical Issues

If critical issues were found:

1. Create a todo list tracking each critical issue
2. Fix each issue directly using Edit tool
3. Run relevant tests/checks after each fix
4. Mark issues as resolved in todo list

If no critical issues, proceed to Step 4.

### Step 4: Code Simplification

1. **Spawn code-simplifier agent** with:
   - List of files to refine
   - Specific simplification opportunities from code-reviewer
   - Reminder to preserve all functionality

2. **Wait for simplification completion**

3. **Verify changes**:
   - Run tests if available
   - Ensure no functionality was altered

### Step 5: Final Verification

1. Run automated checks (tests, typecheck, lint)
2. Summarize all improvements made:
   - Issues fixed from code-reviewer
   - Simplifications applied by code-simplifier
3. Update plan file if needed (add notes about enhancements)

## Report Format

After completion, provide a summary:

```markdown
## Enhancement Summary

### Code Review Findings
- Critical issues fixed: [count]
- Warnings addressed: [count]
- Suggestions applied: [count]

### Changes Made

#### Critical Fixes
- [File:Line] - [What was fixed]

#### Simplifications
- [File] - [What was simplified]

### Verification
- [ ] Tests pass
- [ ] Type checking passes
- [ ] Lint passes

### Files Modified
- [list of files]
```

## Important Guidelines

1. **Sequential Execution**: Complete code-reviewer before code-simplifier
2. **Critical First**: Always fix critical issues before simplification
3. **Preserve Functionality**: Never change what the code does
4. **Scope Discipline**: Only enhance code from completed plan phases
5. **Verify Each Step**: Run checks after fixes and simplifications
6. **User Communication**: Report progress and ask if uncertain about fixes

## When to Stop and Ask

- Critical issue requires architectural decision
- Multiple valid approaches to fix an issue
- Simplification would significantly change code structure
- Tests fail after changes and cause is unclear
