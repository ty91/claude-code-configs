---
allowed-tools: Read, Glob, Grep, Edit, Task, TodoWrite, Write, Bash(ls:*), Bash(tree:*), Bash(mkdir:*), AskUserQuestion
description: Update existing implementation plans through interactive discussion
---

# Update Implementation Plan

You are tasked with updating an existing implementation plan through interactive discussion with the user. Plans evolve as requirements change, new constraints emerge, or implementation reveals unforeseen issues.

## Context

- Git branch: !`git branch --show-current`

Convert slashes to hyphens for the directory name (e.g., `feature/auth` → `feature-auth`). This determines the plan location: `.tasks/{branch-name}/plan.md`

## Getting Started

1. Read the existing plan at `.tasks/{branch-name}/plan.md`
2. Read all documents in `.tasks/{branch-name}/findings/`
3. Present a summary of the current plan state to the user

If the plan file doesn't exist, inform the user and suggest running `/create-plan` first.

## Initial Response

After reading the plan, respond with:
```
I've reviewed the current plan. Here's its status:

**Plan**: [Feature/Task Name]
**Phases**: [N] total, [X] completed, [Y] remaining
**Key areas**:
- [Brief summary of main components]

What would you like to update?
1. Requirements or scope changes
2. Phase structure or ordering
3. Specific implementation details
4. Add/update research findings
5. Other changes

Please describe what needs to change and why.
```

## Update Process

### Step 1: Understand the Change

1. **Listen to the user's change request**
2. **Clarify scope**: Ask focused questions to understand:
   - What triggered this change?
   - Which parts of the plan are affected?
   - Are there new constraints or requirements?
3. **Assess impact**: Identify which phases and sections need modification

### Step 2: Research if Needed

If the change requires understanding new parts of the codebase:

1. Use `codebase-researcher` for focused investigation
2. Save new findings to `.tasks/{branch-name}/findings/`
3. Reference these in the updated plan

**When to research:**
- New requirements touch unfamiliar code areas
- Need to verify assumptions about existing implementation
- Exploring alternative approaches

### Step 3: Propose Changes

Present changes before applying them:
```
Here's what I propose to change:

## Affected Sections:
- [Section name]: [Summary of change]
- [Section name]: [Summary of change]

## Additions:
- [New content being added]

## Removals:
- [Content being removed and why]

## Impact on Other Phases:
- [How this affects downstream work]

Does this capture your intent? Any adjustments needed?
```

### Step 4: Apply Changes

Once the user approves:

1. **Edit the plan file** using Edit tool (preserve structure, update content)
2. **Update the References section** if new sources were consulted
3. **Add a Change Log entry** at the end of the plan (see format below)
4. **Update findings** if research was conducted

### Step 5: Verify and Summarize

After changes are applied:
```
Plan updated successfully.

**Changes made:**
- [List of changes]

**Next steps:**
- [Recommendations for proceeding]

Would you like to make any other changes?
```

## Change Log Format

Add this section at the end of the plan (create if it doesn't exist):

```markdown
## Change Log

### [Date] - [Brief Description]
**Reason**: [Why this change was needed]
**Changes**:
- [Specific change 1]
- [Specific change 2]
**Impact**: [How this affects implementation]
```

## Handling Common Scenarios

### Scope Reduction
- Mark removed items clearly in "What We're NOT Doing"
- Update affected phases
- Adjust success criteria

### Scope Expansion
- Add new phases or expand existing ones
- Conduct research if entering unfamiliar territory
- Update testing strategy

### Implementation Feedback
When changes come from implementation experience:
- Note what was learned
- Update assumptions in "Current State Analysis"
- Adjust approach based on reality

### Completed Phase Changes
If a completed phase needs modification:
- Clearly mark what needs to be revisited
- Uncheck the phase checkbox if rework is needed
- Document why rework is necessary

## Important Guidelines

1. **Preserve Plan Structure**: Keep the existing format; modify content within it
2. **Track Changes**: Always add Change Log entries for significant updates
3. **Incremental Edits**: Make focused changes rather than rewriting entire sections
4. **Maintain Consistency**: Ensure changes don't create contradictions with unchanged sections
5. **Respect Completed Work**: Be cautious about changes that invalidate finished implementation
6. **Document Reasoning**: Capture why changes were made, not just what changed
7. **Research First**: When uncertain about impact, use codebase-researcher before proposing changes
8. **User Approval**: Always get explicit approval before applying changes to the plan file
