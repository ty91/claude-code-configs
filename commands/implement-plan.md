---
allowed-tools: Read, Glob, Grep, Edit, Write, Task, TodoWrite, Bash(pnpm:*), Bash(npm:*), Bash(uv:*), Bash(python:*), Bash(node:*), Bash(npx:*), Bash(git:*), Bash(ls:*), Bash(tree:*), Bash(find:*), Bash(cat:*), Bash(mkdir:*), AskUserQuestion
description: Execute approved implementation plans phase by phase
---

# Implement Plan

You are tasked with implementing an approved technical plan. These plans contain phases with specific changes and success criteria.

## Context

- Git branch: !`git branch --show-current`

Convert slashes to hyphens for the directory name (e.g., `feature/auth` → `feature-auth`). This determines the plan location: `.tasks/{branch-name}/plan.md`

## Getting Started

1. Locate the plan file at `.tasks/{branch-name}/plan.md`
2. Read the plan completely and check for any existing checkmarks (- [x])
3. Read all files mentioned in the plan
4. **Read files fully** - never use limit/offset parameters
5. Create a todo list to track your progress
6. Start implementing if you understand what needs to be done

If the plan file doesn't exist, inform the user and suggest running `/create-plan` first.

## Implementation Philosophy

Plans are carefully designed, but reality can be messy. Your job is to:
- Follow the plan's intent while adapting to what you find
- Implement each phase fully before moving to the next
- Verify your work makes sense in the broader codebase context
- Update checkboxes in the plan as you complete sections

When things don't match the plan exactly:
```
Issue in Phase [N]:
Expected: [what the plan says]
Found: [actual situation]
Why this matters: [explanation]

How should I proceed?
```

## Verification Approach

After implementing a phase:
- Run the automated verification checks (tests, typecheck, lint)
- Fix any issues before proceeding
- Update your progress in both the plan and your todos
- Check off completed items in the plan file using Edit
- **Do NOT perform manual verification yourself** - leave those checkboxes for the user to verify
- **If the phase has manual verification items**: Stop and ask the user to verify before proceeding to the next phase. Manual verification must be completed before moving forward.

## Working Process

1. **Phase by Phase Implementation**:
   - Complete one phase entirely before moving to next
   - Run all automated checks for that phase
   - Update plan checkboxes as you go
   - Mark the Phase header checkbox as complete when all items in that phase are done

2. **When You Get Stuck**:
   - First, ensure you've read and understood all relevant code
   - Consider if the codebase has evolved since plan was written
   - Present the mismatch clearly and ask for guidance

3. **Progress Tracking**:
   - Use TodoWrite to track implementation tasks
   - Update plan file with [x] checkmarks as you complete items
   - Keep user informed of progress

## Resuming Work

If the plan has existing checkmarks:
- Trust that completed work is done
- Pick up from the first unchecked item
- Verify previous work only if something seems off

Remember: You're implementing a solution, not just checking boxes. Keep the end goal in mind and maintain forward momentum.
